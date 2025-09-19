---
tags:
  - QPSI
aliases:
  - qpsi-workflow
  - workflow
  - cr workflow
---

EMAIL Group : 
## Terminology

1. SIL Patch - this is the snippet of code used to search for the affected code snippet in all the qcom components
2. Fix - the fix patch created for the buggy code

## SIL Patch Approval Steps

1. Step 1 - Getting the patch link : ![[Pasted image 20240813122438.png]]
2. Step 2 : Get the patch ID ![[Pasted image 20240813122449.png]]
3. Click the ![[Pasted image 20240813122614.png]]
4. Put this command "SIL patch [CR3764816-1] reviewed by qpsi -[ok]" where CR3764816-1 is the patch name

## Fix Approval Steps

1. Go to **Change Tasks** ![[Pasted image 20240918105119.png]]
2. Click the "View Code Changes" button ![[Pasted image 20240918105137.png]]
3. List of all changes ![[Pasted image 20240918105200.png]]
4. For fix you need the URL there is no fix pattern comment. Below is on example of fix security comment
	1. Fix [https://review-android.quicinc.com/c/kernel/msm-5.15/+/5368293 - Patch Set 1] reviewed by qpsi - [ok]
	2.  Fix [swam link] reviewed by qpsi - [ok]

## Withdrawing CR

1. Make the CR non-security CR.
2. Then withdraw option is activated in "CR action" in top-right corner.