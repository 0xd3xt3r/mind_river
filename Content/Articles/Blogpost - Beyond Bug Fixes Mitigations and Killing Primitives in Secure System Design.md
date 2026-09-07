---
up: "[[Writing MoC]]"
tags:
  - "#type/writing/article"
status: todo
created-date: 2026-08-23
---

# Beyond Bug Fixes: Mitigations and “Killing Primitives” in Secure System Design

Finding and fixing vulnerabilities is the most direct form of security work. If a bug is removed before release, an attacker loses the entry point they needed. In an ideal world, every unsafe memory access, authorization mistake, race condition, and input-validation failure would be detected the moment a developer writes or modifies the code.

Real software development is not ideal.

Large systems include legacy code, external dependencies, complex parsers, drivers, privileged services, and millions of lines of code that change continuously. Some bugs will survive code review, testing, and static analysis. Some fixes may take time because they require compatibility testing, redesign, or coordination across teams.

That is why secure engineering needs more than one strategy.

A practical security program combines:

1. **Preventing bugs from being introduced**
2. **Detecting bugs early**
3. **Fixing bugs at their root cause**
4. **Enabling broad exploit mitigations**
5. **Removing the specific capabilities attackers need**
6. **Limiting impact if a compromise still occurs**

Two important ideas in this model are **mitigations** and **killing exploit primitives**. They are related, but they solve different parts of the problem.

---

## A bug is not automatically an exploit

A vulnerability is the starting point of an attack, not necessarily the end result.

For example, a memory-safety bug may initially produce only a crash. To turn that crash into code execution, data disclosure, privilege escalation, or persistent compromise, an attacker may need several additional capabilities.

They may need to:

- Read a sensitive memory value or address.
- Influence where an object is placed in memory.
- Modify an important value.
- Cause an unsafe type conversion or object reuse.
- Redirect an indirect call or return.
- Reach a privileged operation.
- Bypass a sandbox or authorization boundary.

These attacker-useful capabilities are often called **exploit primitives**.

A simplified model looks like this:

[Bug] -> [Attacker capabilities] -> [Exploit chain] -> [Security impact]

This model matters because defenders can break the chain at more than one point.

- Fixing the bug removes the attacker’s starting point.
- Mitigations make broad categories of exploitation harder.
- Primitive killing removes a specific capability or path the attacker needs.
- Sandboxing and least privilege reduce damage even if an attacker gains control of a component.

---

## The first and best defense: prevent and fix the bug

The best outcome is simple: the bug never reaches users.

Examples include:

- Checking bounds before reading or writing a buffer.
- Correctly managing object ownership and lifetime to prevent use-after-free.
- Validating lengths before allocating, copying, or parsing data.
- Checking permissions before sensitive operations.
- Avoiding unsafe arithmetic when values control memory allocation or copy sizes.
- Using safer APIs, safer abstractions, or memory-safe languages where practical.

This is the only approach that removes the underlying defect itself.

However, fixing a bug is not always immediately easy. The vulnerable behavior may be deeply tied to an API contract, a file format, performance-sensitive code, hardware behavior, or backward compatibility. A correct fix may require more than changing one line—it may require redesigning ownership, restructuring a parser, splitting privileges, or changing how components communicate.

That is exactly why layered defenses matter.

---

## The ideal development experience: security feedback while code is written

The strongest long-term model is one where security feedback appears as close as possible to the moment code is created.

Ideally, developers receive useful signals through:

- Editor and IDE diagnostics
- Compiler warnings
- Static analysis
- Linters and secure coding rules
- Unit and integration tests
- Fuzzing
- Runtime sanitizers during development
- Code-review automation
- Continuous integration checks

The goal is not merely to say, “This code may be unsafe.” A high-quality system should explain:

- What risky pattern was detected
- Why it is dangerous
- Whether attacker-controlled input can reach it
- What impact it may have
- How to fix it safely
- Whether a safer API or design pattern is available

For example, instead of only reporting “possible out-of-bounds access,” a useful tool might identify that a length field from an untrusted message is used to index a buffer without validation and suggest checking the length against the actual allocated size.

This reduces the cost of secure development. The earlier an issue is found, the cheaper it is usually to fix.

---

## Mitigations: broad defenses against# Beyond Bug Fixes: Mitigations, Exploit Primitives, and Security That Survives Imperfect Code

Modern security engineering cannot assume that all bugs will be found and fixed before release.

That is the ideal, of course: catch the bug as soon as the code is written, warn the developer inside the editor, explain the risk, and suggest the correct fix. But real systems are large, old, layered, performance-sensitive, and constantly changing. Some bugs will survive code review. **Some will hide in rarely used paths**. Some will exist in third-party code, which you don't control. Some will be too expensive to remove immediately without redesigning a subsystem.

So a mature security strategy needs more than one answer.

It needs to ask:

> If the bug exists, can we still stop it from becoming a serious security issue?

That question leads to three related but different ideas:

1. **Fix the bug** — remove the root cause.
2. **Enable mitigations** — broadly reduce what bugs can do.
3. **Kill exploit primitives** — remove the specific attacker capabilities needed to exploit a bug.

These are not competing approaches. They are layers.

---

## The Core Mental Model

A bug is not always an exploit.

A bug is the starting condition. An exploit is what happens when an attacker turns that bug into useful control over the system.

A useful simplified model is:

[Bug] -> [Attacker Capability] -> [Exploit Chain] -> [Security Impact]

For example, a memory corruption bug may initially cause only a crash. To turn that crash into something more serious, the attacker may need additional capabilities:

- read a memory address;
- control where an object is allocated;
- corrupt a specific field;
- redirect execution;
- bypass a security check;
- reuse an existing code path;
- escape a sandbox;
- reach a privileged operation.

Those intermediate capabilities are often called **exploit primitives**.

An exploit primitive is not necessarily a full exploit by itself. It is a building block. The attacker chains several of these building blocks together until the original bug becomes something useful: code execution, privilege escalation, data theft, sandbox escape, persistence, or denial of service.

Security work can break the chain at multiple places.

---

# 1. Fixing the Bug: The Best Outcome

The cleanest solution is to remove the vulnerability itself.

Examples:

- Add bounds checks to prevent an out-of-bounds access.
- Fix object lifetime rules to prevent use-after-free.
- Validate length fields before copying memory.
- Check authorization before performing a sensitive operation.
- Correct integer overflow handling before allocation.
- Remove unsafe parsing assumptions.
- Fix race conditions around shared state.

If the bug is removed, the attacker loses the starting point.

This is why the best long-term security model is to detect issues as early as possible: when the code is being written or modified.

The ideal scenario looks like this:

- A developer writes risky code.
- The editor, compiler, static analyzer, sanitizer, or code-review bot detects the pattern.
- The tool explains the issue in developer-friendly language.
- It suggests a safe replacement or design change.
- The fix happens before the code is merged.

That is the highest-leverage point in the lifecycle. The earlier a bug is found, the cheaper it is to fix and the less chance it has to become part of a released attack surface.

But this ideal is not always achieved. That is why we need additional layers.

---

# 2. Mitigations: Security Even When Bugs Exist

Mitigations are broad defensive mechanisms that make many classes of bugs harder to exploit.

They usually do **not** fix the underlying bug. Instead, they reduce the likelihood that the bug can be turned into a severe security outcome.

A good way to describe mitigations is:

> Mitigations are wide-net protections. They reduce the damage a bug can cause, even when the bug still exists.

They are often attractive because they can be applied broadly through compiler flags, platform features, operating-system policies, build configurations, or runtime settings.

Examples include:

- ASLR
- DEP/NX
- CFI
- stack canaries
- sandboxing
- allocator hardening
- memory tagging
- pointer authentication
- hardened syscalls or API policies
- memory-safe languages for new components

The key point is that mitigations usually protect many code paths at once. A team does not need to know the exact future bug in order to benefit from them.

---

## Example: ASLR

**ASLR**, or Address Space Layout Randomization, randomizes where important memory regions are placed.

This can include:

- executable code;
- shared libraries;
- stack memory;
- heap memory;
- memory-mapped regions;
- kernel regions, depending on the environment.

Without ASLR, an attacker may know where useful code or data lives in memory. If a library always loads at the same address, the attacker can build an exploit around that fixed address.

With ASLR, those locations move. The attacker may now need an additional bug, such as an information leak, to discover the correct address.

ASLR does not remove the original memory corruption bug. But it changes the attacker’s problem from:

> “I have one memory corruption bug.”

to:

> “I need a memory corruption bug plus a reliable way to discover memory layout.”

That is a meaningful increase in exploit difficulty.

---

## Example: DEP/NX

**DEP**, also called **NX**, prevents writable memory from being executed as code.

Historically, an attacker could sometimes place machine code into a stack or heap buffer and redirect execution to that buffer. DEP blocks that direct approach by marking writable data memory as non-executable.

Again, DEP does not fix the memory corruption bug. A buffer overflow may still happen. But the attacker can no longer use the simplest “write code into memory and jump to it” pattern.

They may have to switch to more complex techniques that reuse existing executable code. That raises the cost and complexity of exploitation.

---

### Control-Flow Integrity (CFI)

CFI restricts where indirect control-flow transfers are allowed to go.

Indirect calls, function pointers, virtual method calls, callback tables, and return-like control transfers are often valuable targets after memory corruption. If an attacker can overwrite a function pointer, they may try to redirect it to an arbitrary code location.

CFI attempts to ensure that indirect control flow only reaches valid, expected destinations.

**Example:**  
Imagine a structure containing a callback pointer:

`struct request {     void (*complete)(struct request *req);    char data[128]; };`

A memory overwrite may corrupt `complete`. Without protection, the attacker may redirect it to an arbitrary function or gadget. With CFI, the runtime or compiler instrumentation can validate whether the target is an allowed destination for that indirect call.

CFI is particularly valuable because many modern exploits depend on diverting control flow after gaining a memory corruption primitive.

---

### Stack Canaries and Stack Protection

Stack canaries place a secret value near sensitive stack data, commonly before a return address. If a stack buffer overflow overwrites that region, the program can detect the modified canary and terminate before returning through a corrupted address.

**Example:**  
A vulnerable function contains a fixed-size local buffer:

```C
void parse_input(char *input) {     
	char buffer[64];
	strcpy(buffer, input);
}
```

If `input` is too large, it can overwrite nearby stack data. A stack canary can detect this corruption before control returns to the caller.

This does not protect all memory regions or all bug types, but it is a low-cost defense against a historically common exploitation pattern.

---

## Example: Sandboxing

Sandboxing limits what a compromised process can do.

A sandbox assumes that a process may contain bugs. Instead of trusting the process completely, the system restricts its access to files, devices, network resources, system calls, or privileged operations.

This changes the security outcome.

Without sandboxing:

[Compromise process] -> [Compromise large part of system]

With sandboxing:

[Compromise process] -> [Still trapped inside limited environment]

The bug may still exist. The process may still be compromised. But the blast radius is smaller.

That is an important defensive win.

---

## The Strength of Mitigations

Mitigations are valuable because they are broad.

A single mitigation may protect against many unknown bugs. This makes mitigations especially useful in large codebases where it is impossible to manually reason about every future vulnerability.

They are also often easier to deploy than targeted redesigns. Enabling a compiler flag or platform security feature can be simpler than studying a specific exploit chain and changing the architecture of a subsystem.

But “easier” does not mean “free.”

Mitigations can create challenges:

- performance overhead;
- compatibility problems;
- debugging complexity;
- legacy-code breakage;
- partial deployment;
- architecture-specific support gaps;
- false confidence if enabled incorrectly.

Still, when mature mitigations are available, enabling them is often one of the highest-return security investments.

---

# 3. Killing Exploit Primitives: Breaking the Attacker’s Building Blocks

Primitive killing is more targeted than mitigation.

Instead of asking:

> What broad protection can we enable across the program?

it asks:

> What exact capability does the attacker need, and can we remove it?

An exploit primitive might be:

- an arbitrary read;
- an arbitrary write;
- a constrained write;
- control over object placement;
- predictable heap reuse;
- control over an indirect call target;
- access to a dangerous parser state;
- ability to forge an object type;
- ability to bypass a reference count;
- ability to trigger a privileged operation;
- ability to repeatedly spray memory;
- ability to leak addresses;
- ability to confuse one object type with another.

Killing a primitive means removing, restricting, validating, randomizing, or redesigning one of these capabilities so that the exploit chain breaks.

A concise definition:

> Primitive killing is exploitation-informed defense. It studies what attackers need and removes one or more of those requirements.

This is different from simply fixing a bug. It may not remove the original bug. Instead, it removes the attacker’s ability to make the bug useful.

---

## Primitive Killing Requires Exploit Knowledge

This is the important distinction.

Mitigations often cast a wide net. They can be enabled without knowing the exact exploit path for every future bug.

Primitive killing is different. It requires understanding exploitation techniques.

To kill a primitive, defenders need to ask:

- What does this bug give the attacker?
- Can the attacker control memory layout?
- Can they force object reuse?
- Can they leak addresses?
- Can they corrupt a useful field?
- Can they influence a function pointer or dispatch table?
- Can they reach this code path remotely?
- Can they repeat the attempt many times?
- Can they recover after a crash?
- What assumptions does the exploit depend on?

This usually requires studying real exploit patterns, proof-of-concept behavior, allocator behavior, object lifetime, parser state, syscall usage, API flow, and trust boundaries.

So primitive killing is more specialized. It is not just “turn on a flag.” It often requires security engineering judgment.

---

## Example: Heap Exploitation

Consider a use-after-free bug.

A simplified flow may look like this:

1. The program allocates an object.
2. The object is freed.
3. A stale pointer to the freed object remains.
4. The attacker causes a different object to be allocated in the same memory.
5. The program uses the stale pointer.
6. The old pointer now interprets attacker-influenced data as if it were the original object.

The bug is the use-after-free.

But the exploit may need several additional primitives:

- reliable heap grooming;
- predictable object reuse;
- control over the replacement object;
- a useful field at the right offset;
- a way to trigger the stale pointer use;
- a way to convert corruption into control flow or privilege.

A defender can attack this chain in several places.

They can fix the bug directly by removing the stale pointer.

But they can also kill primitives:

- quarantine freed objects so they are not immediately reused;
- randomize allocation behavior;
- separate object types into different allocation caches;
- add object lifetime validation;
- add type checks before dispatch;
- clear sensitive fields on free;
- prevent attacker-controlled objects from occupying sensitive slots;
- add metadata integrity checks;
- limit repeated allocation patterns used for heap grooming.

Each change may not remove every bug. But each one can break an assumption that a reliable exploit depends on.

---

## Example: Heap Metadata Sanity Checks

Allocator hardening is a classic example of primitive killing.

Many historical heap exploits depended on corrupting allocator metadata. If an attacker could modify heap bookkeeping fields, they could sometimes trick the allocator into writing attacker-controlled values to useful locations.

Allocator sanity checks changed that.

By validating metadata before using it, allocators made old exploitation patterns fail. Attackers could no longer rely on the same simple assumptions.

This did not mean heap exploitation disappeared. Attackers adapted and developed new techniques. But that is exactly the point: a useful primitive was killed, and attackers were forced into harder, narrower, or less reliable paths.

That is a defensive success.

Primitive killing often works this way. It may not end exploitation forever. But it changes the economics.

---

# Mitigation vs. Primitive Killing

Both approaches make exploitation harder, but they operate differently.

|Dimension|Mitigation|Primitive Killing|
|---|---|---|
|Scope|Broad|Targeted|
|Goal|Reduce impact of many bugs|Break a specific exploit capability|
|Requires knowing exact exploit path?|Usually no|Usually yes|
|Typical mechanism|Compiler, OS, platform, runtime policy|Code change, validation, redesign, hardening|
|Example|Enable ASLR or DEP|Prevent predictable heap reuse|
|Benefit|Wide coverage|Precise removal of attacker leverage|
|Limitation|May be bypassed or incomplete|Requires deep exploit understanding|
|Effort|Often easier to enable|Can require analysis or re-architecture|
|Best use|Baseline hardening|High-risk components and known exploit patterns|

The simplest distinction is:

> Mitigation protects broadly against unknown bugs. Primitive killing protects specifically against known or anticipated exploitation techniques.

Both are needed.

---

# Why Primitive Killing Can Be Powerful

Primitive killing can produce small but meaningful improvements.

Sometimes it is a simple check:

- validate a length;
- reject an impossible state;
- add a type check;
- clear a pointer;
- block a dangerous transition.

Sometimes it is a medium-sized hardening change:

- separate object pools;
- add metadata integrity checks;
- limit untrusted input influence;
- constrain callback registration;
- restrict dynamic dispatch.

Sometimes it requires re-architecture:

- redesign object ownership;
- isolate a parser into a sandboxed process;
- remove shared mutable state;
- split a privileged service into smaller components;
- replace unsafe APIs;
- move risky code into a memory-safe language.

The impact varies.

A small primitive-killing change may not have the broad effect of enabling a major mitigation across the entire program. But it can still be valuable if it breaks a common or reliable exploit path.

Security often improves through many such incremental changes.

---

# The Ideal: Security Feedback During Development

The best place to fix a vulnerability is before it exists in production.

The ideal developer workflow would include multiple feedback loops:

## In the editor

The developer gets warnings while writing code:

- possible null dereference;
- unsafe copy;
- missing bounds check;
- suspicious integer conversion;
- untrusted input reaching sensitive API;
- object lifetime misuse;
- missing authorization check.

The warning should not merely say “bad code.” It should explain:

- what is risky;
- what could go wrong;
- how to fix it;
- what safe pattern should be used instead.

## During build

Compilers and build systems can enforce hardening:

- warnings as errors for dangerous patterns;
- safe compiler flags;
- sanitizers in test builds;
- CFI where appropriate;
- stack protection;
- fortified library calls;
- dependency checks.

## During testing

Dynamic testing can catch runtime behavior:

- fuzzing;
- sanitizers;
- fault injection;
- race detectors;
- memory checkers;
- API misuse testing;
- malformed input testing.

## During code review

Reviewers should look not only for bugs, but for exploitability conditions:

- Can this input be attacker-controlled?
- Is this object lifetime clear?
- Is this length trusted?
- Is this pointer still valid?
- Is this parser state reachable?
- Is there a privilege boundary here?
- Can failure paths leave dangerous state behind?

## During security review

Security teams can study exploit primitives:

- What would an attacker need after this bug?
- Are there existing mitigations?
- Are those mitigations enabled here?
- Is there a useful information leak?
- Can heap layout be influenced?
- Is the target process sandboxed?
- Is the dangerous operation reachable from untrusted input?

This connects bug prevention, mitigation, and primitive killing into one lifecycle.

---

# Edge Cases and Things That Can Go Wrong

The model is useful, but it should not become too simplistic. There are important caveats.

---

## 1. Mitigations Can Be Missing or Misconfigured

A mitigation only helps if it is actually enabled and effective in the relevant context.

Problems can occur when:

- legacy components are built without the mitigation;
- only some libraries are protected;
- the mitigation is disabled for performance or compatibility;
- the architecture does not support the feature;
- debug builds differ from release builds;
- a policy is too permissive;
- deployment configuration differs from build assumptions.

For example, saying “the platform supports CFI” is not the same as proving that the vulnerable component is protected by a strong CFI policy.

Security teams should verify actual coverage.

---

## 2. Mitigations Can Be Bypassed

Mitigations raise the cost of exploitation. They do not guarantee safety.

Examples:

- ASLR may be weakened by an information leak.
- DEP may be bypassed by reusing existing executable code.
- CFI may allow too many valid targets.
- Stack canaries may not protect the corrupted object.
- Sandboxes may contain escape paths.
- Allocator hardening may stop one technique but not another.

This does not make mitigations useless. It means they should be layered.

One mitigation may force the attacker to need a second bug. Another may make that second bug harder to use. Several independent layers can make exploitation unreliable or impractical.

---

## 3. Not Every Bug Has a Serious Exploit Path

Some bugs are real but not meaningfully exploitable in a given threat model.

A bug may only cause:

- a local crash;
- denial of service;
- corruption of non-sensitive state;
- an unreachable error path;
- failure under trusted input only;
- a theoretical issue with no attacker control.

This matters because primitive killing can be expensive.

Before redesigning a component, ask:

- Who can trigger this bug?
- What privileges do they need?
- Is the input remote, local, physical, or trusted?
- What can the attacker control?
- What can they observe?
- Can they repeat attempts?
- What is the worst realistic impact?

A crash should not automatically be treated as arbitrary code execution. But it should also not be dismissed without understanding the context.

---

## 4. A Primitive May Also Be Legitimate Functionality

Sometimes the thing attackers abuse is also something the product legitimately needs.

Examples:

- dynamic loading;
- plug-in systems;
- flexible parsers;
- scripting support;
- shared memory;
- high-performance allocators;
- device access;
- broad syscall access;
- complex callback mechanisms.

Removing or restricting these capabilities can break real use cases.

Primitive killing is therefore not simply “add checks everywhere.” It requires understanding normal behavior and preserving it.

A good primitive-killing change removes attacker leverage while keeping required functionality intact.

---

## 5. A Local Fix May Only Move the Attack

Attackers adapt.

If one path is closed, they may look for:

- another object type;
- another callback;
- another parser state;
- another allocator behavior;
- another information leak;
- another service with weaker mitigations;
- another privilege boundary.

So after killing a primitive, the important question is:

> Did we remove the attacker capability broadly, or did we only block the exact proof-of-concept path?

Both can be useful, but they are not the same.

Blocking one proof of concept may be a short-term patch. Removing the underlying capability is a stronger architectural improvement.

---

## 6. Primitive Killing Can Create False Closure

A team may see a proof of concept fail after a hardening change and conclude that the vulnerability is solved.

That can be dangerous.

There are at least three different outcomes:

1. **The bug is fixed.**  
    The invalid behavior no longer exists.
    
2. **The demonstrated exploit path is broken.**  
    The proof of concept no longer works, but the bug may remain.
    
3. **The impact is reduced.**  
    The bug still exists, but it is harder to exploit or less damaging.
    

These are all different security states.

It is useful to document which one applies.

For example:

- “This patch fixes the use-after-free.”
- “This patch prevents immediate reuse of the freed object.”
- “This patch blocks the known exploit strategy but does not remove the root cause.”
- “This mitigation reduces likely impact from code execution to denial of service.”

That clarity helps future maintainers.

---

## 7. Hardening Can Introduce New Bugs

Security checks are still code. They can be wrong.

New hardening may introduce:

- compatibility regressions;
- performance issues;
- deadlocks;
- memory overhead;
- incorrect rejection of valid input;
- inconsistent behavior across builds;
- new parser confusion;
- new race conditions;
- new error handling bugs.

Therefore, hardening should be tested like any other product change.

A defensive change is only successful if it improves security without breaking required behavior.

---

# A Practical Security Strategy

A strong security program should combine all three approaches.

## Step 1: Remove the root cause where possible

If you can fix the bug, fix it.

Do not rely on mitigations as an excuse to leave known vulnerabilities in place.

## Step 2: Enable broad mitigations by default

Use platform and compiler protections wherever practical:

- ASLR;
- DEP/NX;
- CFI;
- stack protection;
- sandboxing;
- memory tagging;
- hardened allocators;
- safer build flags;
- least-privilege process design.

Treat these as baseline hygiene.

## Step 3: Study exploitability for high-risk areas

For exposed, privileged, complex, or historically vulnerable components, go deeper.

Ask how exploitation would actually work.

Look for primitives such as:

- address leaks;
- object reuse;
- attacker-controlled allocation patterns;
- weak validation;
- dangerous callbacks;
- overly powerful APIs;
- parser confusion;
- unsafe state transitions.

## Step 4: Kill the most valuable primitives

Prioritize primitives that are:

- reusable across multiple bug classes;
- reachable from untrusted input;
- useful for privilege escalation;
- useful for sandbox escape;
- historically common in exploits;
- cheap to remove;
- high impact if left available.

## Step 5: Build detection into developer workflows

Move security feedback earlier:

- editor warnings;
- static analysis;
- secure code templates;
- compiler diagnostics;
- fuzzing;
- sanitizers;
- CI checks;
- code-review guidance;
- exploitability-focused review for dangerous components.

## Step 6: Document what was achieved

Be precise.

Did the change:

- fix the bug;
- reduce exploitability;
- block a known primitive;
- enable a mitigation;
- reduce blast radius;
- improve detection;
- add defense-in-depth?

This prevents confusion and helps future security reviews.

---

# A Simple Example End-to-End

Suppose a media parser has a bug where a malformed file can trigger an out-of-bounds write.

A layered response may look like this:

## Bug fix

Add correct bounds checking so the write cannot exceed the buffer.

## Mitigations

Ensure the parser process is built with:

- ASLR;
- DEP/NX;
- stack protection;
- CFI if applicable;
- hardened allocator;
- sandbox restrictions.

These reduce the chance that a future parser bug becomes full system compromise.

## Primitive killing

Study how an attacker would exploit parser bugs.

Then remove useful attacker capabilities:

- reject malformed structure combinations earlier;
- limit allocation sizes;
- randomize or isolate risky object allocations;
- avoid placing control-sensitive objects near attacker-controlled buffers;
- remove dangerous callbacks from parser-controlled data;
- clear metadata after failure;
- make parser state transitions stricter;
- split parsing into a lower-privilege process.

## Developer feedback

Add tests and tooling:

- fuzz malformed files;
- add regression tests for the bug;
- enable sanitizers in test builds;
- add static checks for dangerous length calculations;
- document safe parser patterns.

This is much stronger than any one fix alone.

---

# The Big Idea

Security should not be framed only as:

> “Find bug, fix bug.”

That is necessary, but incomplete.

A better model is:

> “Prevent bugs where possible, find them early, reduce their impact, and remove the attacker capabilities that make them exploitable.”

Mitigations and primitive killing complement each other.

Mitigations are broad. They are the wide net. They protect against many unknown bugs and are often relatively easy to deploy across a program or platform.

Primitive killing is targeted. It requires understanding how exploitation works. It studies the attacker’s path and removes the building blocks they depend on.

Bug fixing removes the root cause.

Together, they form a layered strategy:

[Prevent] -> [Detect] -> [Mitigate] -> [Kill Primitives] -> [Contain Impact]

The goal is not to pretend that bugs will never exist.

The goal is to build systems where a bug is less likely to become a compromise.

---

## Final Summary

- A bug is the root cause, but an exploit usually needs additional attacker capabilities.
- Those capabilities are exploit primitives.
- Fixing the bug is the best outcome.
- Mitigations broadly reduce the impact of unknown and known bugs.
- Primitive killing removes specific capabilities that attackers rely on.
- Mitigations are often easier to enable because they cast a wide net.
- Primitive killing requires studying real exploitation techniques.
- Both approaches have limits, bypasses, and tradeoffs.
- The ideal security workflow catches bugs during development and also hardens the system so surviving bugs are less dangerous.
- Mature security engineering is not one layer. It is a chain of defenses designed to break the attacker’s chain of exploitation.