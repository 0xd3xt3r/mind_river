---
aliases:
  - AMD Fuzzing
tags:
  - "#fuzzing-project"
  - "#vulnerability-research"
  - "#type/project/personal"
status: todo
up: "[[Personal Project MoC]]"
created-date: 2024-12-21
summary: Hunting for bugs in AMD open-source software.
---
## Tasks and Questions

- [ ]

---

## Objective
> What were you trying to achieve with this project.

---
## Commands
> Commands which you important or used very frequently while working on the project

```bash
# some of the important command in this project
```
---

## Notes
> Random interesting information, observation or anecdotes you observed while working on the projects.
- 

---
## Challenges & Opportunities
> While working on a project you might encounter challenges which could potentially be solved to further the project.
- 

---

## Product Docs
> Some of the important internal documents which would you often need to refer very often.

1. [Install driver](https://amdgpu-install.readthedocs.io/en/latest/)
2. [ROCm Docs](https://rocmdocs.amd.com/en/latest/)

---
## Build Instruction
> Instruction to clone and build the project from source

```bash
# Cloning instruction

# Building instruction
```

---
## Bug Reports


### Existing CVE

1. CVE-2024-49893 - amd display driver bug
2. CVE-2024-26656 - drm/amdgpu: fix use-after-free bug
3. CVE-2024-42228
4. CVE-2022-48990

### Bug 1

```c

// not valid => kvmalloc_array is interger overflow safe
int amdgpu_bo_create_list_entry_array(struct drm_amdgpu_bo_list_in *in,
				      struct drm_amdgpu_bo_list_entry **info_param)
{
	const void __user *uptr = u64_to_user_ptr(in->bo_info_ptr);
	const uint32_t info_size = sizeof(struct drm_amdgpu_bo_list_entry);
	struct drm_amdgpu_bo_list_entry *info;
	int r;
	
	// integer overflow "in->bo_number * info_size", under allocation
	info = kvmalloc_array(in->bo_number, info_size, GFP_KERNEL);
	if (!info)
		return -ENOMEM;

	/* copy the handle array from userspace to a kernel buffer */
	r = -EFAULT;
	if (likely(info_size == in->bo_info_size)) {
		unsigned long bytes = in->bo_number * in->bo_info_size;

		if (copy_from_user(info, uptr, bytes))
			goto error_free;

	} else {
	     // bytes size -> sizeof(struct drm_amdgpu_bo_list_entry);
		unsigned long bytes = min(in->bo_info_size, info_size);
		unsigned i;
		// buffer overwrite
		memset(info, 0, in->bo_number * info_size);
		// array over large value
		for (i = 0; i < in->bo_number; ++i) {
			if (copy_from_user(&info[i], uptr, bytes))
				goto error_free;

			uptr += in->bo_info_size;
		}
	}

	*info_param = info;
	return 0;

error_free:
	kvfree(info);
	return r;
}
```


### Bug 2 

```c
https://github.com/torvalds/linux/blob/feffde684ac29a3b7aec82d2df850fbdbdee55e4/drivers/gpu/drm/amd/amdgpu/amdgpu_cs.c#L195

static int amdgpu_cs_pass1(struct amdgpu_cs_parser *p,
			   union drm_amdgpu_cs *cs)
{

chunk_array = kvmalloc_array(cs->in.num_chunks, sizeof(uint64_t),
				     GFP_KERNEL);
	if (!chunk_array)
		return -ENOMEM;

	/* get chunks */
	chunk_array_user = u64_to_user_ptr(cs->in.chunks);
	if (copy_from_user(chunk_array, chunk_array_user,
			   sizeof(uint64_t)*cs->in.num_chunks)) {
		ret = -EFAULT;
		goto free_chunk;
	}

	p->nchunks = cs->in.num_chunks;
	p->chunks = kvmalloc_array(p->nchunks, sizeof(struct amdgpu_cs_chunk),
			    GFP_KERNEL);
	if (!p->chunks) {
		ret = -ENOMEM;
		goto free_chunk;
	}

	for (i = 0; i < p->nchunks; i++) {
		struct drm_amdgpu_cs_chunk __user *chunk_ptr = NULL;
		struct drm_amdgpu_cs_chunk user_chunk;
		uint32_t __user *cdata;

		chunk_ptr = u64_to_user_ptr(chunk_array[i]);
		if (copy_from_user(&user_chunk, chunk_ptr,
				       sizeof(struct drm_amdgpu_cs_chunk))) {
			ret = -EFAULT;
			i--;
			goto free_partial_kdata;
		}
		p->chunks[i].chunk_id = user_chunk.chunk_id;
		p->chunks[i].length_dw = user_chunk.length_dw; // unsaniztized length varliabel
		
		// data coming from user.
		size = p->chunks[i].length_dw;
		cdata = u64_to_user_ptr(user_chunk.chunk_data);

		p->chunks[i].kdata = kvmalloc_array(size, sizeof(uint32_t),
						    GFP_KERNEL);
		if (p->chunks[i].kdata == NULL) {
			ret = -ENOMEM;
			i--;
			goto free_partial_kdata;
		}
		// integer overflow
		size *= sizeof(uint32_t);
		if (copy_from_user(p->chunks[i].kdata, cdata, size)) {
			ret = -EFAULT;
			goto free_partial_kdata;
		}
		
}
```
---

## Thread Model/Attack Surface
> How can we attack the System?

---
## Meeting Notes
> All the important meeting you have done for the project

---
## Event Logs
> A brief timeline of the project

1. 08-12-2024 - Start project discussion

## Tech Team PoC

| PoC Name | Expertise |
|---|---|
|  |  |

---
## Related Research Papers or Articles
> Any research paper or articles which address some the issues which we are facing
- External Research
- Existing IRVR Tickets
---
## Task

- [x] #task find hardware ⏳ 2024-12-07

## Docs
- https://amdgpu-install.readthedocs.io/en/latest/install-installing.html
