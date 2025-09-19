[Skorpio Advanced Binary Instrumentation Framework](https://github.com/msuiche/OPCDE/blob/master/2018/Emirates/Skorpio%20Advanced%20Binary%20Instrumentation%20Framework%20-%20Nguyen%20Anh%20Quynh/Opcde2018-skorpio.pdf)

## Redirection techniques

In order to generate code, we translate the snippet into machine language code in the memory of the  
mutator process, and then copy it into the array in the application address space. The most difficult part of  
inserting instrumentation is carefully modifying the original code to branch into the newly generated code.  
To do this, we use short sections of code called trampolines. Figure 2 shows the structure of a trampoline  
and its relationship to the instrumentation point. Trampolines provide a way to get from the point where we  
wish to insert the instrumentation code to our newly generated code. To do this, we replace one or more  
instructions at the instrumentation point with a branch to the start of a base trampoline. The base trampo-  
line code then branches to a mini-trampoline. The mini-trampoline saves the appropriate machine state  
(such as the registers and condition codes), and contains the code for a single snippet. At the end of the  
snippet, we place code to restore the machine state and to branch back to the base trampoline. The base  
trampoline then executes the instruction(s) that were displaced from the original code. If the snippet is to  
be inserted after the point executes (i.e., after a function call return), we can also insert a mini-trampoline  
here.  
Multiple snippets can be inserted at a single point, and they are chained together such that the end  
of one snippet branches to the start of the next one, and the final snippet branches back to the trampoline.

![[Pasted image 20230203162758.png]]

## Ref
1. http://www.cs.umd.edu/~hollings/papers/apijournal.pdf
2. https://www.cl.cam.ac.uk/techreports/UCAM-CL-TR-606.pdf
3. https://www.cl.cam.ac.uk/techreports/UCAM-CL-TR-606.pdf
4. https://dl.acm.org/doi/abs/10.1145/1176760.1176793
5. file:///C:/Users/mshelia/Downloads/WCTC_2018_ZhaoValerie_EvaluationofDynamicB.pdf