---
tags:
  - qpsi/project
  - codec
status: done
---

# Codec Fuzzing

## Projects

1. Project already fuzzed
	- mm-parser
	- alac
	- dsd
	- flac
	- mp3
1. Project Not Fuzzed
	- adec-qcelp13
	- adec-evrc
	- adec-ape


## CR Raised

1. First Batch
	1. https://orbit/CR/3210532
	2. https://orbit/CR/3210505
	3. https://orbit/CR/3175183
	4. https://orbit/CR/3175182
	5. https://orbit/CR/3175179
	6. https://orbit/CR/3174852
	7. https://orbit/CR/3171366
	8. https://orbit/CR/3157125
2. Second Batch
	1. https://orbit/CR/3233301
	2. https://orbit/CR/3233298
	3. https://orbit/CR/3233295
	4. https://orbit/CR/3233294
	5. https://orbit/CR/3228641
	6. https://orbit/CR/3228603
	7. https://orbit/CR/3224278


## Notes
- [LA Source](https://opengrok.qualcomm.com/source/xref/LA.UM.10.9.1/vendor/qcom/proprietary/mm-audio-noship/omx/)
- [Git link](ssh://review-android.quicinc.com:29418/platform/vendor/qcom-proprietary/mm-audio-noship)
- [mm-audio-noship Scott's effort](https://github.qualcomm.com/sbauer/mm-audio-noship/tree/master/adec-dsd/sw/dec/dsd2pcm/test)
- [AVS.ADSP.2.9 Src](http://hyd-lnxbld008:8080/xref/AVS.ADSP.2.9/main/aud/algorithms/audencdec/ape/dec/ApeDecoderLib/tst/)

## Cmd's

1. afl-gcc -fsanitize=address -ggdb -O0 memscpy.c main_qpsi.c dsd2pcm_header.c ../../dsd2pcm/src/*.c -I ../../dsd2pcm/inc/ -I ../../CSIM/common/basic_op/ -o fuzzer
1. clang -O0  test/omx_ape_dec_test.c  ../../adec-ape/src/*.c -I ../../adec-ape/inc/ -I ../../CSIM/common/basic_op/ -o fuzzer

## Existing CR's

CVE-2016-2477 ANDROID-27251096 
CVE-2016-2478 ANDROID-27475409 
CVE-2016-2479 ANDROID-27532282
CVE-2016-2480 ANDROID-27532721 
CVE-2016-2481 ANDROID-27532497 
CVE-2016-2482 ANDROID-27661749 
CVE-2016-2483 ANDROID-27662502
CVE-2016-3747 ANDROID-27903498
CVE-2016-3746 ANDROID-27890802


https://orbit/CR/998722
https://orbit/CR/1010088
https://orbit/CR/1000235
https://orbit/CR/1063982

## Existing Research
1. [Fuzzing Android: a recipe for uncovering vulnerabilities inside system components in Android](https://www.blackhat.com/docs/eu-15/materials/eu-15-Blanda-Fuzzing-Android-A-Recipe-For-Uncovering-Vulnerabilities-Inside-System-Components-In-Android-wp.pdf)
1. [FANS: Fuzzing Android Native System Services via Automated Interface Analysis](https://www.usenix.org/system/files/sec20fall_liu_prepub.pdf)
2. [Fuzzing Android OMX](https://hitcon.org/2016/CMT/slide/day2-r2-c-1.pdf)
	1. Found bugs in QC code base with similar pattern in Audio sub-system.
	2. [Open OMX](https://elinux.org/images/e/e0/The_OpenMAX_Integration_Layer_standard.pdf)

## Bugs Reports

### Bug 1

input_buffer_size can cause buffer overflow
https://opengrok.qualcomm.com/source/xref/LA.UM.10.9.1/vendor/qcom/proprietary/mm-audio/omx/adec-amr/qdsp6/src/omx_amr_adec.cpp#3049

https://opengrok.qualcomm.com/source/xref/LA.UM.10.9.1/vendor/qcom/proprietary/mm-audio/omx/adec-amr/qdsp6/src/omx_amr_adec.cpp#3067

https://opengrok.qualcomm.com/source/xref/LA.UM.10.9.1/vendor/qcom/proprietary/mm-audio/omx/adec-amr/qdsp6/src/omx_amr_adec.cpp#3076

OMX codec : Integer overflow for calculating buffer size leading to buffer under allocation.

omx_common_adec::allocate_input_buffer

### Bug 2

https://opengrok.qualcomm.com/source/xref/LA.UM.10.9.1/vendor/qcom/proprietary/mm-audio-noship/omx/adec-common/sw/src/omx_common_adec.cpp


**Project** : platform/vendor/qcom-proprietary/mm-audio

**Files**
- ~~omx/adec-common/sw/src/omx_common_adec.cpp
- ~~omx/adec-evrc/sw/src/omx_evrc_adec.cpp
- ~~omx/adec-qcelp13/sw/src/omx_Qcelp13_adec.cpp~~
- ~~omx/aenc-mpegh-sw/sw/src/omx_mpegh_aenc_base.cpp
- omx/aenc-qcelp13/qdsp5/src/omx_qcelp13_aenc.cpp
- 7k/adec-omxwma/src/omx_wma_adec.cpp
- 7k/adec-omxmp3/src/omx_mp3_adec.cpp
- omx/adec-wma/qdsp5/src/omx_wma_adec.cpp
----
Affected File : vendor/qcom/proprietary/mm-audio/omx/adec-amr/qdsp6/src/omx_amr_adec.cpp

### Bug 3
```C
OMX_ERRORTYPE  COmxAmrDec::allocate_input_buffer(__unused OMX_IN OMX_HANDLETYPE hComp,
        OMX_INOUT OMX_BUFFERHEADERTYPE** bufferHdr,
        __unused OMX_IN OMX_U32          port,
        OMX_IN OMX_PTR                   appData,
        OMX_IN OMX_U32                   bytes)

{
    OMX_U32 nBufSize = bytes;
[...]
            if(pcm_feedback)
            {
                /*Integer Overflow possible here */
                nMapBufSize = nBufSize + (unsigned int)sizeof(META_IN);
            }
            else
            {
                nMapBufSize = nBufSize;
            }
            mmap_data =(struct mmap_info *)alloc_ion_buffer(nMapBufSize);
[...]
            bufHdr->nAllocLen         = nBufSize;
            bufHdr->pAppPrivate       = appData;
            bufHdr->nInputPortIndex   = OMX_CORE_INPUT_PORT_INDEX;
            bufHdr->nOffset           = 0;
            m_input_buf_hdrs.insert(bufHdr, (OMX_BUFFERHEADERTYPE*)mmap_data);
            m_inp_current_buf_count++;
```
  
  

### Bug 4
```C

OMX_ERRORTYPE  COmxAmrDec::allocate_output_buffer(__unused OMX_IN OMX_HANDLETYPE hComp,
        OMX_INOUT OMX_BUFFERHEADERTYPE** bufferHdr,
        __unused OMX_IN OMX_U32               port,
        OMX_IN OMX_PTR                     appData,
        OMX_IN OMX_U32                       bytes)

{

[...]
           /* possible integer overflow */
            nMapBufSize = nBufSize + (unsigned int)sizeof(META_OUT);
            mmap_data =(struct mmap_info *)alloc_ion_buffer(nMapBufSize);
[...]
            bufHdr->nAllocLen         = nBufSize;
            bufHdr->pAppPrivate       = appData;
            bufHdr->nOutputPortIndex  = OMX_CORE_OUTPUT_PORT_INDEX;
            bufHdr->nOffset           = 0;
            bufHdr->pOutputPortPrivate = (void *)ion_fd;
            m_output_buf_hdrs.insert(bufHdr, (OMX_BUFFERHEADERTYPE*)mmap_data);
            m_out_current_buf_count++;
        }

```
  

### Bug 5
```C
void* COmxAmrDec::alloc_ion_buffer(unsigned int bufsize)

{
    struct mmap_info *ion_data = NULL;
    unsigned int heap_id_mask = 0;
    int rc = 0;
    DEBUG_PRINT("%s %p\n",__FUNCTION__,this);
    ion_data = (struct mmap_info*) calloc(sizeof(struct mmap_info), 1);
    if(!ion_data)
    {
        DEBUG_PRINT_ERROR("\n alloc_ion_buffer: ion_data allocation failed\n");
        return NULL;
    }
    /* bufsize is control by user possible integer overflow*/
    ion_data->map_buf_size = (bufsize + 4095) & (~4095);
```
### Bug 6

```C

OMX_ERRORTYPE  COmxAmrDec::use_input_buffer(
        __unused OMX_IN OMX_HANDLETYPE   hComp,
        OMX_INOUT OMX_BUFFERHEADERTYPE** bufferHdr,
        __unused OMX_IN OMX_U32          port,
        OMX_IN OMX_PTR                   appData,
        OMX_IN OMX_U32                   bytes,
        OMX_IN OMX_U8*                   buffer)
{
    OMX_U32 nBufSize = bytes;

[...]
        if (bufHdr != NULL && loc_bufHdr != NULL)
        {
            if(pcm_feedback)
            {
                /* possbile buffer overflow */
                nMapBufSize = nBufSize + (unsigned int)sizeof(META_IN);
            }
            else
            {
                nMapBufSize = nBufSize;
            }
            mmap_data =(struct mmap_info *)alloc_ion_buffer(nMapBufSize);
  
```
### Bug 7

  
```C
OMX_ERRORTYPE  COmxAmrDec::use_output_buffer(
        __unused OMX_IN OMX_HANDLETYPE   hComp,
        OMX_INOUT OMX_BUFFERHEADERTYPE** bufferHdr,
        __unused OMX_IN OMX_U32          port,
        OMX_IN OMX_PTR                   appData,
        OMX_IN OMX_U32                   bytes,
        OMX_IN OMX_U8*                   buffer)
{
    OMX_U32 nBufSize = bytes;
[...]
            nMapBufSize = nBufSize + (unsigned int)sizeof(META_OUT);
            mmap_data =(struct mmap_info *)alloc_ion_buffer(nMapBufSize);
            bufHdr->nAllocLen         = nBufSize;
            bufHdr->pAppPrivate       = appData;
[...]
```

#### Second Batch Bugs

1. adec-aac/qdsp6/inc/omx_aac_adec.h
2. adec-alac/qdsp6/inc/omx_alac_adec.h
3. **adec-amr/qdsp6/inc/omx_amr_adec.h**
4. adec-amrwbplus/qdsp6/inc/omx_amrwbplus_adec.h
5. adec-ape/qdsp6/inc/omx_ape_adec.h
6. adec-g711/qdsp6/inc/omx_g711_adec.h
7. adec-wma/qdsp6/inc/omx_wma_adec.h


1. https://orbit/CR/3233301 - AlacDec
2. https://orbit/CR/3233298 - Amrwbplus [D1]
3. https://orbit/CR/3233295 - WmaDec
4. https://orbit/CR/3233294 - G711
5. https://orbit/CR/3228612 - Amrwbplus [D1]
6. https://orbit/CR/3224278 - AacDec
7. https://orbit/CR/3228641 - ApeDec
8. https://orbit/CR/3228603 - AmrDec

 
