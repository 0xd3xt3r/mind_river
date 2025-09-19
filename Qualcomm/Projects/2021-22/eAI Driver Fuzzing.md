---
tags:
  - qpsi/project
  - fuzzing
status: done
---
# embedded Artificial Intelligence (eAI) Driver Fuzzing

**Lead** : [[Jyotsna]]
**People Involved** : [[Jeevan Bhooi]], [[Xiaoming Zhou]], Bob Pabla
**Project** : Part of [[QDI Driver Fuzzing]] campaign
**Src : ** [Github repo](https://github.qualcomm.com/eai/EAI)

## Future works
 1. Github security research on Qualcomm NPU driver [link](https://securitylab.github.com/research/qualcomm_npu/)
 2. Samsung NPU [link](https://googleprojectzero.blogspot.com/2020/12/an-ios-hacker-tries-android.html)
 3. Research on NPU Android Attack surface
 4. [Bug Reports Query](https://orbit/query/54839)
 5. [Offical Docs share by the team](https://confluence.qualcomm.com/confluence/display/eaisw/Sync%2C+Build%2C+Debug%2C+and+Submit+eAI+code)

## Notes
 
- Tool to convert onnx file to eai file `runtime/tools/model_builder/eai_builder`
- model test file - `runtime\test_app\test_scripts\test_files\models_algo\enpu_v2\fixed`
- `CMAKE_C_COMPLITER=clang CMAKE_CXX_COMPILER=clang++ ./runtime/build.sh --enpu_ver 2 --build_fixed32 --debug --clean_build`
- `./runtime/testapp/run_all_test.sh -fixed32`
- `./build-fixed32/test_app/eaitestapp -fuzz_file ./fuzz_input/input_1.bin`
 - To add code coverage feature - `CC=clang CXX=clang++ LDFLAGS='-fprofile-instr-generate -fcoverage-mapping -fsanitize=address' CFLAGS='-fprofile-instr-generate -fcoverage-mapping -fsanitize=address' ./runtime/build.sh --enpu_ver 2 --build_fixed32 --clean_build`
 - create crash reports `/pkg/modem_sw/afltriage/afltriage -i output/fuzzer_*/crashes/id* -o reports ./build-fixed32/test_app/eaitestapp -fuzz_file @@`
 - `export ASAN_SYMBOLIZER_PATH=../../tools/clang-tools/src/llvm-project/build/bin/llvm-symbolizer`
- The program take found input file.
	- the model file which is the machine learning model
	- the input file - this is the input vector on which the ML processing is done
	- the output file - this is where the output of the ML is written to
	- the golden file - this is the file against which the result of the output is compared with

## [[Fuzzing]] Challenges

- The biggest challenge in fuzzing this driver is that it takes multiple input file for processing and standard fuzzers like AFL, etc only take one file as input.
- There was not much of the documentation was found on the internet on this topic except for this [link](https://github.com/google/fuzzing/blob/master/docs/split-inputs.md#how-to-split-a-fuzzer-generated-input-into-several).
- Couple of Solution to this problems are :
	- **Solution One :** Concatenate those file together and put file offset metadata in the start or end of the file and separate those file in the harness.
		- Since the file is of variable size offset metadata is important to separate the files in the harness.
		- the drawback of this approach is the that if the bytes containing the offset byte is mutated it can cause problem while separating the file in the harness.
	- **Second solution :** is to store the offset metadata in the other file and read the offset metadata base on file name but the problem with this approach are
		- if the file name changes then we can't find the offset
			- but if we use the file hash to search the offset metadata still the problem is the after the file mutation the hash will also change and so the mapping becomes irrelevant for newly mutated file.
		- and other problem is after that when the new mutation is created the offset metadata has to be transfer in the metadata record which will be become and problem in itself
	- **Solution Three :** modify AFL such that is doesn't mutate the offset metadata in the appended in the input file.
		- This way we don't have to maintain any record externally and it is carried with the new mutation file.
		- this approach might fail if AFL adds new bytes in mutable section of the file this way we won't be able to parse the mutable buffer as whole as there is new byte.
		- this approach will also fail as when you adjust the offset in AFL source and when it discovers the new mutation it will dump the input with the adjusted offset, which is equivalent to removing the offset metadata. This behavior can be fixed by **solution five** is much better option. 
	- **Solution Four :** modify the AFL to take multiple input folder and do mutation on each file
		- this may or may not work entirely because size one input file is related to other, basically they are in pairs we might generate lots of invalid input and the pair information will not be carried with the mutation.
	- **Solution Five :** put delimiter between two appended files and in the harness scan for delimiter and separate the files.
		- if the any validation fails just exit the program.
		- this solution better then rest of the one because if the new byte is inserted then the inserted byte will only cause problem in one file and not disrupt the structure of other file.
		- slight short coming of this method is search for delimiter can be incur some performance cost. This can be fixed by adding offset metadata at the start and verifying the file delimiter at that offset.
	- **Solution Six :** modify AFL to do mutation more then one input folders.
		- this is problematic because AFL is build around the assumption that there is only on input folder and it have to way to know which change produce the new coverage. If it doesn't know that then any change in any of the file will lead to new copy of all files.
		- this the worst solution of all.

## Fuzzing Results
### Nov - 2021
- 55,000 crashes and around 2000 unique crashes 
- Category of Bugs found
	1. Heap overflow (Read/Write)
	2. SIGSEGV
		1. null pointer de-reference
	3. SIGFPE - floating pointer exception
		1. divide by zero
	4. Negative sized params - Integer overflow
	5. Allocating very high size memory
	6. ASAN : attempting free on address which was not malloc()-ed
	7. ASAN : memcpy-param-overlap
	8. Use after free

### April - 2022

| Software | Version | Function Coverage | Line Coverage | Region Coverage | Branch Coverage |
|---|---|---|---|---|---|
| Before Fuzzing | - | 20.87% (311/1490) | 14.13% (6293/44536) | 14.32% (4752/33175) | 10.62% (2101/19786 |

- 2nd Attempt to find bugs as in previous software is completely rewritten

## Meeting Notes

### Meeting 1
People : Jeevna, Nilo and me
- eAI driver converts [onnx](https://onnx.ai/) model file to qualcomm's prosperity format. which is parsed by the driver
- eai_builder binary can convert onnx file to eai format
	- ![[Pasted image 20210903195555.png]]

### Meeting 2
	
**Date** : 16-09-2021
**Discussion**

- embedded ml library
- buffer lock prevents from unmapping of memory area, its still vulnerable to read/write from user  PD.
- buffer lock 2 API's does validation if the buffer belongs to the calling process

### Meeting 3

**Date** : 28-10-2021

- Create CR for the eAI driver so that when we have a security incident then we don't need to pay the researcher
	- Don't make them security CR
		- we have agreement from top level that they are aware of the security issue.
		- Security CE will need more work.
- Attach the input file to the security CR even if the harness is non-standard, will be useful for future use.

### Meeting 4

**Date** : 12-01-2022
**People** : Xiaoming Zhou

- eAI driver has been moved from Root PD to User PD not because of the security reason but for performance reason in low powered devices.
- Now the eAI is a library in Audio PD.
- This change is done in waipio itself.
- They won't be fixing the issue as their priority right now is Kailua development.
- But Xiaoming suggested to still raise the CR in-case in future their priority changes to this issues.

### Meeting 5

Date : 05-04-2022

- Code base is different from Kailua and forward.
	- re-run the fuzzing on this code base to get fresh issues.
- Fuzz test was run on Waipio the code base is valid
- CR's we have raise in the pass were on Waipio.
- For Kailua
	- eAI is in the user PD.
	- eNPU driver is in root PD.
	- eNPU driver is different is always in the root PD.
	- model parsing changed
- latest version is 2.8 and more changes coming

### Meeting 6

Date : 18-04-2022

- epnu driver in root pd
- eAI user land application and 
- waipio is previous generation


### Meeting 7

- Model Signature check
- 


## Ref
1. [How to get eAI code](https://confluence.qualcomm.com/confluence/display/eaisw/How+to+get+eAI+code)