
## Components of the fuzzer

1. Crash Detection
	1. Debuggers
	2. Sanitisers 
		1. Address Sanitiser
		2. Memory Sanitiser
3. Fuzzer feedback 
	1. Coverage Feedback
	2. Edge Coverage feedback
	3. Error code feedback
4. Corpus Minimization
5. Crash Triage
	1. Crash Reproducing
	2. Crash De-duplication
6. Harness
	1. This perhaps the part of the fuzzer where you will put most of the effort and creativity then the rest of the components. The more fancier the target the more creative you have to be.
	2. Because you need to have deeper understanding of the code base. You also need to understand attack surface and the tainted data(data which the attacker controls).
	3. Different type of harnessing
		1. On-Target - Running the target software on target like mobile device, IoT Device or PC Software.
			1. This is often chosen because the target software id too much hardware dependent and you don't want to put too much effort in to emulating stuff. Since the target is the most precise target behaviour its an appropriate choise to save time.
		2. Emulator
		3. Direct on system
		4. Off-Target porting
7. Corpus generation
8. Fuzz Case Mutation strategy
9. Fuzzing Metrics
	1. Execution per second
	2. Code Coverage/Edge coverage
10. Automation
	1. I case you are working with unique target like embedded fuzzing you are using simple debugger and doing random fuzzing.

## Types of fuzzing targets

1. File format fuzzing
2. Grammar Base fuzzing
3. Browser Engine
4. API Base Fuzzing
5. Kernel Fuzzing


## Workflow of fuzzing



