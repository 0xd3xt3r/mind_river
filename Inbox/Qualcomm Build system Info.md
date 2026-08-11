---
up: "[[Knowledge Base MoC]]"
tags:
  - "#type/knowledge"
status: todo
created-date: 2026-07-09
related: 
summary:
---

## Components

1. CRM id - this is the 
2. AU id
3. contents.xml - vendor build
4. build_tree_details.txt - qssi build - path ./LINUX/android/release_notes/build_tree_details.txt
```json
{
"EntityType":"SoftwareProduct",
"EntityName":"Hawi.LA.1.0.r1",
"EventName":"BuildApproved",
"EventArgs":
	{
	"BuildID":"Hawi.LA.1.0.r1-00840-STD.PROD-1.172652.2",
	"Approved By":"NdsPatchAutomation",
	"Approved On":"\\/Date(1783567743239)\\/",
	"Service Type":"Patch Build",
	"Service Type ID":"3",
	"User":"NdsPatchAutomation",
	"IsNoShadowBuild":false
	}
}
 
```
## Rest API

| API                                  | Description                                    |
| ------------------------------------ | ---------------------------------------------- |
| `/api/v1/get_oe_filer_paths/`        | Get OE filer path                              |
| `/api/v1/get_files/`                 | Get bin/artifact paths by SI branch and target |
| `/api/v1/get_manifest_repr/`         | Get manifest project name                      |
| `/api/v1/tag_commit/`                | Check whether a tag exists in a project        |
| `/api/v1/get_filer/`                 | Get AU filer path                              |
| `/api/v1/get_destination_comp_data/` | Get SI component composition                   |
| `/api/v1/get_mapping_images/`        | Get SI and secondary SI/AU info                |
| `/api/v1/get_prj_branch_from_au/`    | Get manifest project and branch from AU tag    |
| 

### Endpoints

1. https://github.qualcomm.com/qpsi/au_automation/blob/main/kw_image_json/LA.Vendor.17.6.0.art.json
2. https://hedwig.qualcomm.com/images/tags