---
tags:
  - vulnerability-research
status: in-progress
created-date: 26-03-2021
---

# Bug Report

- source https://www.metabaseq.com/imagemagick-zero-days/
- This vulnerability is very similar to CVE-2022-44268: ARBITRARY REMOTE LEAK
- this project is also part of oss-fuzz and you can find the details to build the application in oss-fuzz directory
	- https://imagemagick.org/script/install-source.php
- the bug is in coders/msl.c
	- msl is a Magick scripting language to process the image on command line.

```C
// msl.c
static void MSLStartElement(void *context,const xmlChar *tag,
  const xmlChar **attributes)
{
// skip the code
  new_profile=FileToStringInfo(filename,~0UL,exception);
  if (new_profile != (StringInfo *) NULL) {
	  (void) ProfileImage(msl_info->image[n],name,
		GetStringInfoDatum(new_profile),(size_t)
		GetStringInfoLength(new_profile),exception);
	  new_profile=DestroyStringInfo(new_profile);
	}
  continue;
}

// skip the code
```

# Existing Vulnerabilities

1. https://community.f5.com/t5/technical-articles/imagetragick-imagemagick-remote-code-execution-vulnerability/ta-p/276262
2. https://securityboulevard.com/2023/02/yet-more-imagemagick-vulnerabilities/
3. https://imagetragick.com/

