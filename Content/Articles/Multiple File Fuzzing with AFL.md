---
up: "[[Writing MoC]]"
tags:
  - type/writing/article
status: abandoned
created-date: 2024-12-10
---


Working on a fuzzing project recently I came across interesting challenge in the situation where we had to fuzz a program which tool multiple input files for processing. Doing the basic internet research there was not much of the documentation was found on the internet on this topic except for this [link](https://github.com/google/fuzzing/blob/master/docs/split-inputs.md#how-to-split-a-fuzzer-generated-input-into-several) but it didn't seem to provide the solution to the problem I was facing. There were couple of solution which came to my mind right of the bat! First been just append two file and separate those file in the harness, Second been fix one file and second file should be the fuzz corpus and third been just modify the fuzzer (AFL) to deal more then one file. Well all these solutions had their pros and cons, and I ended up selecting one of the solution for my project and it work pretty well for me but I don't think other solutions were not bad, they just didn't fit in my project. So thought why not document those solutions with all the pros and cons so you can use these detail to decide which of these technique can help you solve the same problem in your specific use case.

Before we dig into the solution lets briefly understand the how AFL works so that we are on the same page. You have a program which you want to fuzz which takes command-line input as the path of the file you want the target program will process the file and AFL will mean while collect the coverage as the program is executing. You have to provide collection of input files called corpus, which AFL will cycle thought. AFL also modifies the file slightly and see's if it has produced any new code coverage. If it does produce new coverage AFL will create the copy of the modified file and store it in a directory which it will use in future execution. Sometime getting the user data is not that straight forward as putting the file path as command-line option, you may have to write program on top of the target program to send the input to the target section of the program you want to fuzz, its called as Harness. With that been said lets look at different solutions.
- **Solution One :**
	- Create a script that will concatenate multiple file together but that's not enough you program is expecting two files, you need to modify the programs such that it will take one input file and that file will be separate the file in the program(or write a special harness). But if file are simply concatenated together how will I know where does the first file start and next file starts? I would like you to ask this question to all of the solutions which we will discuss.
	- To fix this problem in this solution I will put the offset information at the start of the concatenated file. The offset metadata is the offset within the file where particular file starts, we will fix the size of each offset to 4 bytes so that the harness the parse it. This also gives us the flexibility to concatenate more then 2 file. All the at harness needs to know is that how many files it should be expecting say of example it is expecting 3 files then it needs to parse first 12 bytes and extract out the 3 (4 byte) offsets from that and separate 3 buffer using on that. You could put the offset metadata at the of file it doesn't matter, its just that it have to be at a predictable location so that harness parse it from there and start or end of the file is the only place that I can think of.
	```
	+----------+----------+----------------------+---------------------+
	| Offset 1 | Offset 2 |     File 1  Data     |     File 2 Data     |
	+----------+----------+----------------------+---------------------+
	```
	- The whole reason of doing the offset metadata saga is because multiple file which we are dealing with can be of varying sizes. If those files are of fixed size then you job becomes very simple we just have to separate out those file base on that size alone.
	- **Drawback**
		- The drawback of this approach is the that if section of the file which contains the offset metadata is mutated by AFL the file separation login in the harness will fail and may separate the file incorrectly or crash the program both of these failure are not fruitful for our fuzzing goal.
- **Second solution :**
	- This solution is tries to fix the drawback of the previous solution. In this method instead of storing the offset metadata in within the file we can store the offset metadata in the external file. This external file will have mapping of all the file name used in fuzzing and corresponding offset metadata. When the test case is executed harness will reading the offset metadata of the corresponding file.
	- But there is on problem with this approach when AFL creates new mutation it changes the file name and also when you first lunch AFL it will move your input to different folder and rename all the file do to this your mapping will become useless also when AFL finds a new mutation of the file it will have to add new entry of the offset metadata in the mapping file. To add this new functionality we cannot add this code in harness as it might crash we will have to modify AFL to write back the offset metadata in the mapping file, that will be too much work.
	- But if using the filename as key to get offset metadata and issue we use file hash as the key to search the offset metadata still the problem is that after the file mutation the hash will also change and so the mapping becomes irrelevant for newly mutated file.
	- Instead of using filename as metadata tracking we can use file hash in mapping file this will solve the problem partially we will still have to patch AFL to add mapping of new mutation file. Taking the route of patch AFL should be the last option lets look at another solution.
- **Solution Three :**
	- Modify AFL such that is doesn't mutate the offset metadata in the appended in the input file.
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
- **Solution Seven** : fix one file and 2nd file should be fuzzing corpus 
	- this solution will become unmanageable from more then two file
	- if you have good variety of corpus on both file types
	- and if your files work in pairs.