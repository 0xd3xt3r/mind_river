---
up: "[[Qualcomm MoC]]"
tags: "#qpsi/project"
created-date: 2026-04-24
status: todo
related: 
summary: 
participants: 
---

# Secure Coding FAQ

- Support Email - [channels.support@qualcomm.com](mailto:channels.support@qualcomm.com "mailto:channels.support@qualcomm.com")

| Question                                                | Solution                                                                                                               |
| ------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| Issue not able to join the list                         | Contact - PoC Balakrishnan Gurumurthy                                                                                  |
|                                                         | go/p4sh                                                                                                                | 
| training is completed but don't have access             | wait for 48 hours to be fully updated                                                                                  |
| People done only 1 training and register the issue      | complete both the secure code training 1                                                                               |
| People complete both training but still not have access | may be the infra is down check the history of securecodetraining, email to listbuilder@qualcomm.com                    |
| People who have completed 2 year secure code training.  | People are blacklisted from Perforce and added to SCRPastDue automatically.  <br>They have to complete the training    |
| To push code to for linux kernel driver                 | They must complete 3 training 1. Secure Code 1 & 2 and Linux Kernel  2. Exception List - securecodelinux_tempexemption |
| People asking excemption for service account            | Add the account to SecureTrainingService list                                                                          |

## Group Info

| Group                         | Group ID                          | Purpose                                                                                              |
| ----------------------------- | --------------------------------- | ---------------------------------------------------------------------------------------------------- |
| SecureCode Group list         | securecodetraining                | People who have completed the training will be added to this group automatically                     |
| Secure Code exception list    | SecureCodeExemptions              | Temporary Exception                                                                                  |
| Past Due Date                 | SCRPastDue                        | People who have passed due date. This is automatic list. Remind them to complete the training        |
| Training Refresher List       | scrassignment                     | People who have to refresh their training. Automatic list                                            |
| Linux Kernel Secure Code List | SecureCodeLinuxKernel             | People who have completed Linux kernel secure code                                                   |
| Kernel Exception List         | securecodelinux_tempexemption     | Linux Kernel training exception                                                                      |
| Bot/Service Account           | SecureCodeTrainingServiceAccounts | Only Service Account should be added to the group, they are bots who scan the code                   |
| Perforce Blacklist            | SCR_Revoke                        | People who didn't complete the training on time are added to this blacklisted group. Automatic list. |
|                               | SCRPastDueManual                  | Orphan group                                                                                         |

## Channels

- This filter will give you list of all channel - user could be added to one of the below channels
	- ![[channels.png]]
