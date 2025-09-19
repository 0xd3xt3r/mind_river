# CmD Line Tools to deal with binary-data

# Radare2

## XXD
### Skip a few lines with xxd?

Suppose you don't want hex dump of the complete file. Instead, you want the tool to start converting from a specific line. Then this can be achieved using the -s command.

For example, if you want xxd to produce hex dump from line 3 onwards, then here's how you tell xxd to do this:

`xxd -s 0x30 test.txt`

Here's the output produced:
```
00000030: 6961 7c59 0a0a 3034 7c43 6869 6e61 7c4e  ia|Y..04|China|N  
00000040: 0a30 357c 5275 7373 6961 7c59 0a30 367c  .05|Russia|Y.06|  
00000050: 4a61 7061 6e7c 590a 0a30 377c 5369 6e67  Japan|Y..07|Sing  
00000060: 7061 6f72 657c 590a 3038 7c53 6f75 7468  paore|Y.08|South  
00000070: 204b 6f72 6561 7c4e 0a30 397c 4669 6e61   Korea|N.09|Fina  
00000080: 6c61 6e64 7c59 0a31 307c 4972 656c 616e  land|Y.10|Irelan  
00000090: 647c 590a                                d|Y.
```

### Limit xxd output to a particular length?

In the previous section, we discussed how to make xxd start converting from a particular point. But there's also a way to limitize its conversion to a particular point. This can be done using the -l command-line option.

For example, to make sure xxd creates dump for only the first three lines of test.txt, use it in the following way:

`xxd -l 0x30 test.txt`

Here's the output it produced:

```
00000000: 4e6f 2e7c 436f 756e 7472 797c 5965 732f  No.|Country|Yes/  
00000010: 4e6f 0a30 317c 496e 6469 617c 590a 3032  No.01|India|Y.02  
00000020: 7c55 537c 590a 3033 7c41 7573 7472 616c  |US|Y.03|Austral
```

### Set column length?

If you want xxd to produce fewer or more columns in the output, use the -c option and specify the number of columns there. Here's an example command using this option:

`xxd -c 5 test.txt`

And here's the output:

```
00000000: 4e6f 2e7c 43  No.|C  
00000005: 6f75 6e74 72  ountr  
0000000a: 797c 5965 73  y|Yes  
0000000f: 2f4e 6f0a 30  /No.0  
00000014: 317c 496e 64  1|Ind  
00000019: 6961 7c59 0a  ia|Y.  
0000001e: 3032 7c55 53  02|US  
00000023: 7c59 0a30 33  |Y.03  
00000028: 7c41 7573 74  |Aust  
0000002d: 7261 6c69 61  ralia  
00000032: 7c59 0a0a 30  |Y..0  
00000037: 347c 4368 69  4|Chi  
0000003c: 6e61 7c4e 0a  na|N.  
00000041: 3035 7c52 75  05|Ru  
00000046: 7373 6961 7c  ssia|  
0000004b: 590a 3036 7c  Y.06|  
00000050: 4a61 7061 6e  Japan  
00000055: 7c59 0a0a 30  |Y..0  
0000005a: 377c 5369 6e  7|Sin  
0000005f: 6770 616f 72  gpaor  
00000064: 657c 590a 30  e|Y.0  
00000069: 387c 536f 75  8|Sou  
0000006e: 7468 204b 6f  th Ko  
00000073: 7265 617c 4e  rea|N  
00000078: 0a30 397c 46  .09|F  
```


### Make xxd produce binary dump

The -b command-line option makes xxd produce dump in binary digits. Here's what the man page says about this tool:

```
-b | -bits  
Switch to bits (binary digits) dump, rather than hexdump.  This option writes octets as eight digits "1"s and  "0"s  instead  
of  a  normal  hexadecimal  dump. Each line is preceded by a line number in hexadecimal and followed by an ascii (or ebcdic)  
representation. The command line switches -r, -p, -i do not work with this mode.
```

`xxd -b text.bin`

![](https://www.howtoforge.com/images/command-tutorial/xxd-b-option.png?ezimgfmt=rs:0x0/rscb4/ng:webp/ngcb4)
