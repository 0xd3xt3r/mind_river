---
tags:
  - qpsi/project
  - codec
status: done
---
# EVS vocodec Fuzzing

## Code coverage information

| Software | Version | Function Coverage | Line Coverage | Region Coverage | Branch Coverage |
|---|---|---|---|---|---|
| Before Fuzzing | 12.12 | 80.07% (1294/1616) | 84.02% (74682/88891) | 83.95% (45762/54508) | 77.94% (20705/26564) |
| After Fuzzing | 12.12 |   80.32% (1298/1616) | 85.17% (75709/88891) | 84.97% (46313/54508) | 79.86% (21213/26564) |
| Before fuzzing | 12.15 |   78.61% (1294/1646) | 83.45% (74788/89621) | 83.37% (45814/54952) | 77.16% (20731/26866)|
| After fuzzing | 12.15 |   78.68% (1295/1646) | 83.73% (75042/89621) | 83.61% (45943/54952) | 77.57% (20840/26866) |

## CR's
1. https://orbit/CR/3171326
2. https://orbit/CR/3171366
3. https://orbit/CR/3132962
4. ~~https://orbit/CR/3132948  [DISCARDED] - this entry point is not used~~

## Notes
- [Release Notes Folder](https://qualcomm.sharepoint.com/teams/AudioSystems-Codecs/Shared%20Documents/Forms/AllItems.aspx?csf=1&web=1&e=tigQB7&OR=Teams%2DHL&CT=1649670019319&params=eyJBcHBOYW1lIjoiVGVhbXMtRGVza3RvcCIsIkFwcFZlcnNpb24iOiIyNy8yMjAzMDcwMTYxMCJ9&cid=f351d02a%2De271%2D4d39%2Daf4e%2D3f2ec1c017b3&RootFolder=%2Fteams%2FAudioSystems%2DCodecs%2FShared%20Documents%2FGeneral%2Fvocoder%2FEVS%2F12%2E15&FolderCTID=0x012000CC946856FC079447AFFB1247524A467B) as report of changes for new version from 3GPP
- [Release Notes Link](https://qualcomm.sharepoint.com/:w:/r/teams/AudioSystems-Codecs/_layouts/15/Doc.aspx?sourcedoc=%7B9D5399B8-B25E-4699-AA9B-584045A7D2FD%7D&file=S4-20xxxx_CR26442-0039_Corrections_Rel12.docx&action=default&mobileredirect=true) the document which has all the changes
- System Release of REL 16 version of the code Location: **//depot/AUDIO_SYSTEM/VocoderSims/evs/rel/12.15/1.0.0/** on P4 server: qctp401:1666

## Build Commands

```bash
LLVM_PROFILE_FILE="foo.profraw" ./EVS_dec 8 ../../dataset/testvec/testv/bitstreams/nb/stv8c_dtx_9600_8kHz.b10.COD xx.dd
/local/mnt/workspace/tools/clang-12/bin/llvm-profdata merge foo.profraw -o foo.profdata
/local/mnt/workspace/tools/clang-12/bin/llvm-cov show ./EVS_dec -instr-profile=foo.profdata
```


## Bugs

- Copying of uninitialized memory to st->lsp_prevfrm in case of bitrate/bandwidth switching, causing potential reading of uninitialized memory
- Avoid usage of negative length remaining_len used in copy operation in the function FEC_synchro_exc ()  possibly causing reading out of bounds on certain implementations. The changes are located in FEC_synchro_exc_fx() and don’t affect bit-exactness on the test vectors. [https://orbit/CR/3132962]
- Prevent out of bound shift operations in AVQ index decoding, leading to possible reading of uninitialized memory. The changes are located in re8_decode_base_index_fx (). This affects bit-exactness on the test vectors (decoder).
- ~~Avoid endless loop when decoding VoIP input files with less than two frames by adding corrections to the logic. The changes are located in decodeVoip() and don’t affect bit-exactness on the test vectors.~~ (This bug was discarded as it didn't have the entry point)
## Command Ref

make CFLAGS='-fprofile-arcs -ftest-coverage -I lib_com -I basic_op -I basic_math -I lib_enc -I lib_dec' LDFLAGS=' -lgcov --coverage' EVS_dec

## Ref
1. https://itectec.com/spec/5-file-formats/
2. [3GPP Ref](http://www.voiceage.com/EVS.html)
3. [Docs for 12.15](https://qualcomm.sharepoint.com/teams/AudioSystems-Codecs/Shared%20Documents/Forms/AllItems.aspx?csf=1&web=1&e=tigQB7&OR=Teams%2DHL&CT=1644433787508&sourceId=&params=%7B%22AppName%22%3A%22Teams%2DDesktop%22%2C%22AppVersion%22%3A%2227%2F22010300408%22%7D&cid=ddfb13b4%2D2698%2D487f%2D8d22%2Dcbc174cead5b&RootFolder=%2Fteams%2FAudioSystems%2DCodecs%2FShared%20Documents%2FGeneral%2Fvocoder%2FEVS%2F12%2E15&FolderCTID=0x012000CC946856FC079447AFFB1247524A467B)
4. [Bug Reports](https://qualcomm.sharepoint.com/:w:/r/teams/AudioSystems-Codecs/_layouts/15/Doc.aspx?sourcedoc=%7B9D5399B8-B25E-4699-AA9B-584045A7D2FD%7D&file=S4-20xxxx_CR26442-0039_Corrections_Rel12.docx&action=default&mobileredirect=true)
