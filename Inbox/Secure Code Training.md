---
up: "[[Qualcomm MoC]]"
tags: "#qpsi/project"
created-date: 2026-04-24
status: todo
related:
  - "[[Klockwork]]"
summary: Secure Code Training FAQ
participants:
---

# Secure Coding FAQ

- Support Email - [channels.support@qualcomm.com](mailto:channels.support@qualcomm.com "mailto:channels.support@qualcomm.com")
- Channels Team Lead - Manikantesh Pemmaraju

| Question                                                | Solution                                                                                                               |
| ------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| Issue not able to join the list                         | Contact - PoC Balakrishnan Gurumurthy                                                                                  |
|                                                         | go/p4sh                                                                                                                |
| training is completed but don't have access             | wait for 48 hours to be fully updated                                                                                  |
| People done only 1 training and register the issue      | complete both the secure code training 1                                                                               |
| People complete both training but still not have access | may be the infra is down check the history of securecodetraining, email to listbuilder@qualcomm.com                    |
| People who have completed 2 year secure code training.  | People are blacklisted from Perforce and added to SCRPastDue automatically.  <br>They have to complete the training    |
| To push code to for linux kernel driver                 | They must complete 3 training 1. Secure Code 1 & 2 and Linux Kernel  2. Exception List - securecodelinux_tempexemption |
| People asking exemption for service account             | Add the account to SecureTrainingService list                                                                          |

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

## Channels Team PoC

1. Raju Kaligotla
2. Monica Rana - Senior Manager
	1. Raju Kaligotla - reports to Monica and handles channels support.

## Email

Hi <NAME>,

Please check if you have completed both parts of Secure Code 2023 training. It takes 48 hours to propagate the status and reflect the changes in the system. To check if your status has been updated, please check your name in the `securecodetraining` list manager group.

**securecodetraining** -- [securecodetraining - A List Manager](https://lists.qualcomm.com/ListManager?id=securecodetraining&query=securecodetraining&type=list "https://lists.qualcomm.com/ListManager?id=securecodetraining&query=securecodetraining&type=list")

Also, check if you have been added to the SCR_Revoke/SCRPastDue lists. If you have not completed the trainings on time you will be added to the following blacklists. We do not handle perforce read/write/branch access. It is automated.

- **SCR_Revoke** -- [SCR_Revoke - A List Manager](https://lists.qualcomm.com/ListManager?match=eq&field=default&query=SCR_Revoke "https://lists.qualcomm.com/ListManager?match=eq&field=default&query=SCR_Revoke")
    
- **SCRPastDue** -- [SCRPastDue - A List Manager](https://lists.qualcomm.com/ListManager?match=eq&field=default&query=SCRPastDue "https://lists.qualcomm.com/ListManager?match=eq&field=default&query=SCRPastDue")
    

If you have been added to these lists, please ensure that you have completed the courses mentioned below and have taken the refresher quiz. If yes, please mail [channels.support - A List Manager](https://lists.qualcomm.com/ListManager?id=channels.support&query=channels.support&type=list "https://lists.qualcomm.com/ListManager?id=channels.support&query=channels.support&type=list") keeping us in CC.

- Secure Code 2023 -- Part 1 [Secure Code 2023 - Part 1 - Workday](https://wd12.myworkday.com/qualcomm/learning/course/9051c6c039b010015fe266626a050001?type=9882927d138b100019b928e75843018d&record=3821b77ce545100591f2304d229d0001 "https://wd12.myworkday.com/qualcomm/learning/course/9051c6c039b010015fe266626a050001?type=9882927d138b100019b928e75843018d&record=3821b77ce545100591f2304d229d0001")
    
- Secure Code 2023 -- Part 2 [Secure Code 2023 - Part 2 - Workday](https://wd12.myworkday.com/qualcomm/learning/course/69639a02338c1001658a23161d160000?type=9882927d138b100019b928e75843018d&record=e65eeb562d351000a661a89c7df50001 "https://wd12.myworkday.com/qualcomm/learning/course/69639a02338c1001658a23161d160000?type=9882927d138b100019b928e75843018d&record=e65eeb562d351000a661a89c7df50001")
    

Secure Code refresher quiz (Part 1 & Part 2)

- [Secure Code: Refresher Quiz 1 - Workday](https://wd12.myworkday.com/qualcomm/learning/course/82d1995251fe01986796320b5a00152d?type=9882927d138b100019b928e75843018d "https://wd12.myworkday.com/qualcomm/learning/course/82d1995251fe01986796320b5a00152d?type=9882927d138b100019b928e75843018d")
    
- [Secure Code: Refresher Quiz 2 - Workday](https://wd12.myworkday.com/qualcomm/learning/course/3789d269cd8e0170fb58b8325a00a62f?type=9882927d138b100019b928e75843018d "https://wd12.myworkday.com/qualcomm/learning/course/3789d269cd8e0170fb58b8325a00a62f?type=9882927d138b100019b928e75843018d")
    

For Linux Kernel write access, you need to additionally complete Secure Code: Linux Kernel course as well.

- [Secure Code: Linux Kernel - Workday](https://wd12.myworkday.com/qualcomm/learning/course/6e0a7a0ac16f01a679bc35095e000c2b?record=aeaa800792a6100201355c052c710000&type=9882927d138b100019b928e75843018d "https://wd12.myworkday.com/qualcomm/learning/course/6e0a7a0ac16f01a679bc35095e000c2b?record=aeaa800792a6100201355c052c710000&type=9882927d138b100019b928e75843018d")
    

Please revert back to us if your trainings and refresher quiz is complete but still facing issues. 

  

Lists to check:

  

__

|Group|Link|
|---|---|
|SCR_Revoke|[SCR_Revoke - A List Manager](https://lists.qualcomm.com/ListManager?match=eq&field=default&query=SCR_Revoke "https://lists.qualcomm.com/ListManager?match=eq&field=default&query=SCR_Revoke")|
|SCRPastDue|[SCRPastDue - A List Manager](https://lists.qualcomm.com/ListManager?match=eq&field=default&query=SCRPastDue "https://lists.qualcomm.com/ListManager?match=eq&field=default&query=SCRPastDue")|
|scrassignment|[scrassignment - A List Manager](https://lists.qualcomm.com/ListManager?id=scrassignment&query=scrassignment&type=list "https://lists.qualcomm.com/ListManager?id=scrassignment&query=scrassignment&type=list")|
|SecureCodeExemptions|[SecureCodeExemptions - A List Manager](https://lists.qualcomm.com/ListManager?match=eq&field=default&query=SecureCodeExemptions "https://lists.qualcomm.com/ListManager?match=eq&field=default&query=SecureCodeExemptions")|
|securecodetraining|[https://lists.qualcomm.com/ListManager?id=securecodetraining&query=securecodetraining&type=list](https://lists.qualcomm.com/ListManager?id=securecodetraining&query=securecodetraining&type=list "https://lists.qualcomm.com/ListManager?id=securecodetraining&query=securecodetraining&type=list")|

  

  

Thanks & Regards


## Channels

- This filter will give you list of all channel - user could be added to one of the below channels
	- ![[channels.png]]
