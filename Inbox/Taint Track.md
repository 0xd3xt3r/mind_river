---
up: "[[Vibe Project MoC]]"
tags:
  - "#type/prompt-idea"
created-date: 2026-02-21
related:
  - "[[LLM Assisted Fuzzing]]"
summary: Track the tainted variable
---

## Notes

Tainted variable is the attacker controlled data which passed to the attack surface.

We want to track the tainted variable from the source to the sink.

Taint variables:
- argc
- argv

```C
int attack_surface(void * device, int argc, void* argv) {
	func_deapth1((struct wola*) argv);
}

int func_deapth1(struct wola* bola) {
	// now 'data_1' variable is also tainted
	int data_1 = wola->data;
}

int func_deapth2(int var1) {
	printf("var1 is %d", var1);
}
```

Taint functions:
- func_deapth1
- func_deapth3