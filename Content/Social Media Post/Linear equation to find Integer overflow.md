---
up: "[[Writing MoC]]"
tags:
  - type/writing/social-media
status: todo
title: Integer overflow using equation
created-date: 2024-12-10
---

```cpp
 20 int main() {
 21     int sched_req_size;
 22     int num_links = -15;
 23     sched_req_size = sizeof(struct cam_req_mgr_sched_request_v3) + (num_links * sizeof(int));
 26     printf("Size : %u\n", sizeof(struct cam_req_mgr_sched_request_v3) );
 27     printf("Res : %lu\n", sched_req_size);
 28 }

64 + (x * 4) = y // start
x = (y - 64) / 4  // result
x -> num_links
y -> TARGET VALUE
```

```cpp
soc_req_v2 = (struct ope_clk_bw_request_v2 *)blob_data;

size_t clk_update_size = sizeof(struct ope_clk_bw_request_v2) + ((soc_req_v2->num_paths - 1) * sizeof(struct cam_axi_per_path_bw_vote));
			
if (soc_req_v2->num_paths > CAM_OPE_MAX_PER_PATH_VOTES) {
	CAM_ERR(CAM_OPE, "Invalid num paths: %d",
		soc_req_v2->num_paths);
	return -EINVAL;
}

/* Check for integer overflow */
if (soc_req_v2->num_paths != 1) {
	if (sizeof(struct cam_axi_per_path_bw_vote) >
		((UINT_MAX -
		sizeof(struct ope_clk_bw_request_v2)) /
		(soc_req_v2->num_paths - 1))) {
		CAM_ERR(CAM_OPE,
			"Size exceeds limit paths:%u size per path:%lu",
			soc_req_v2->num_paths - 1,
			sizeof(
			struct cam_axi_per_path_bw_vote));
	return -EINVAL;
	}
}
```

1. The problem of integer overflow occurs when there some calculation carried out to estimate the size of buffer which needs to be allocated and result is feed into malloc to get the buffer.
2. 
3. You can convert the memory calculation into a linear equation. Since the operation will only be combination of addition, subtraction, division and multiplication i can safely say its a linear equation not a quadratic equation and its very rare you will memory size calculation been done using and exponentiation equation.
4. The result of the equation will be the value you want to achieve for example in this case you want UINT_MAX. This will be another variable.
	1. So you will have two category of variables.
		1. Group of user-controlled variables
		2. The another variable which will the value you want to overflow to 
5. the you rearrange the equation such that you have all the user controlled variable on one-side and non-user controlled variable on the other side.
6. The non-user controlled variable can be assigned some numeric value based on the variable value. You can even deduce the value of the variable base on multiple factor
	1. If there is a size of struct you can statically compute that value.
	2. if there is some other variable which is passed down from some other function you can debug the program and get a possible value or set of possible values.
7. you have to solve for the variable which you cannot control then you will end up with an equation like `x + y = z (constant numberic value)` where x and y are user-controlled.
	1. Now as a that `x` and `y` are attacker controlled so you can give any combination of this value which can result in `z`.
	2. 