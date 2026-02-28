---
up: "[[Qualcomm Project MoC]]"
tags:
  - "#qpsi/project"
  - "#klockwork"
  - "#type/project"
aliases:
  - Klockwork Developer Review Training
status: in-progress
created-date: 2022-01-01
summary: DR Reviewer Training
---

## Task


## Klockwork Status 

![[Pasted image 20241209184944.png]]
## Docs

[Developer Review (DR) Training](https://qualcomm-my.sharepoint.com/:p:/r/personal/apayapur_qti_qualcomm_com/_layouts/15/guestaccess.aspx?e=88e5mu&share=Ed1lj1mX3QZMjZiPKx_Ym3kBKyH-jqlUuHhd2ktrvT3n6w)

[Master sheet](https://qualcomm-my.sharepoint.com/:x:/p/apayapur_qti/ES7fxBe4DvlGn0nosvCTwTABVZ1BpYUJGyg_oiR9AjPRvw?email=mshelia%40qti.qualcomm.com&e=W1iUE8&CID=47DCF15A-DE7B-457F-87CC-0FFC8FAB794E&wdLOR=cF8E3C967-ED74-4F6D-A158-F2786632AC14)

## Notes

- in case someone says its NON-QC code do git blame and check
- [SA Reviewer List Link](https://lists.qualcomm.com/ListManager?action=view&query=SA_Reviewers&field=default&match=eq)

## Documentation

1. [In-build Checkers](https://docs.roguewave.com/en/klocwork/current/candccheckerreference)
1. [Custom Checkers](https://docs.roguewave.com/en/klocwork/2021/cckastsyntaxreference)
1. [Tuning](https://help.klocwork.com/current/en-us/concepts/tuningccanalysis.htm)

## Resources

1. [How to setup and run Klockwork (QPSI Internal Wiki)](https://confluence.qualcomm.com/confluence/display/QPSIRoadMapTeam/Klocwork+Setup)
2. [Klockwork in-built checkers docs](https://docs.roguewave.com/en/klocwork/current/candccheckerreference)
3. [Developer Review (DR) Training](https://qualcomm-my.sharepoint.com/:p:/r/personal/apayapur_qti_qualcomm_com/_layouts/15/guestaccess.aspx?e=88e5mu&share=Ed1lj1mX3QZMjZiPKx_Ym3kBKyH-jqlUuHhd2ktrvT3n6w)
4. [Command Reference](https://docs.roguewave.com/en/klocwork/current/commandreference)
5. [KAST Syntax Reference](https://docs.roguewave.com/en/klocwork/2021/cckastsyntaxreference#concept455)
6. [KAST Examples](https://docs.roguewave.com/en/klocwork/current/cckastexamples#concept447)


## Email Template

### Start test email

Hi [Candidate’s Name],

Thank you for your interest in the Developer Review Training. Your willingness to assist with the security of your module is crucial to ensuring the overall security of our product line.

As the next step, you have to take a small test. This test is basically a list of Klockwork issues for which you have to provide your analysis. I have attached an Excel file to this email containing a list of these issues. Your task is to review these issues, classify them as instructed in the "DR Review Training" and put the status in the "status" column, and provide your written analysis in the comment section.

Here are some tips to write good analysis:

1. **Function Trace and Variables:** Describe the function trace and identify the variables responsible for the vulnerability.
2. **Variable Values:** Explain the values of the variables that can violate expected behavior and lead to a vulnerability.
3. **Stack Trace Analysis:** Analyze the stack trace provided by Klockwork to understand the issue. This is extremely useful to understand the issue.

Please download the Excel sheet, create an offline copy, and fill in the necessary details. Please do not leave any comment on the Klockwork issue page.

We often receive questions about the relevance of Klockwork issues to your usual domain. The Developer Reviewer Program is not a reflection of a candidate’s domain expertise, and it does not require domain-specific knowledge.

I urge you to take this test seriously. Any security lapses can severely damage our reputation and lead to significant costs associated with fixing the bug later in the development process. The later we fix a bug, the more burdensome the internal and external processes become, which no one wants to be part of.

Thank you for your cooperation.



## Candidates Added

| Candidate Name          | Date       | Team              |
| ----------------------- | ---------- | ----------------- |
| Neema Thomas            | 2025-01-01 | DSP sys arch team |
| Zehui Gong              | 2024-20-03 | Android           |
| Manishkumar Arjun Bante | 2024-11-01 |                   |
| Siva                    | 2025-04-14 |                   | 

## Logs

```dataview
TASK
WHERE (icontains(text, this.file.name) AND icontains(text, "#log/sprint")) AND 
		!(icontains(text, "#task") OR icontains(text, "#question"))
	AND !completed
GROUP BY file.name as Filename
SORT file.mtime DESC
LIMIT 20
```
## Archived Task

- [x] #task Neema Thomas sheet review #kw ⏬ ➕ 2024-12-06 📅 2024-12-12 ✅ 2025-01-03 🔒 [[2025-01-10]] 🕸️ Task
