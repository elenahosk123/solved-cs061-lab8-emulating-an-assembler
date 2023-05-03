Download Link: https://assignmentchef.com/product/solved-cs061-lab8-emulating-an-assembler
<br>
The purpose of this lab is to do some more advanced subroutine development, specifically nested subroutine calls.

You will construct and print a table of Assembly Language instructions with their Machine Language opcodes.

And you will build a search function for that table which emulates one basic element of an assembler or compiler.

<ul>

 <li>Our Objectives for This Week</li>

</ul>

<ol>

 <li>Exercise 01 ~ Subroutine with helper: Prints out LC3 instructions &amp; op-codes</li>

 <li>Exercise 02 ~ Subroutine with helper: Instruction parser</li>

</ol>

<h1>Exercise 01</h1>

Write the following subroutine:

;———————————————————————————————–

; Subroutine: SUB_PRINT_OPCODE_TABLE

; Parameters: None

; Postcondition: The subroutine has printed out a list of every LC3 instruction

;                                  and corresponding opcode in the following format:

;                  ADD = 0001 ;      AND = 0101

;                                  BR = 0000

;                                  …

; Return Value: None

;———————————————————————————————–

<strong>Specifications: </strong>

<ul>

 <li>The data for the subroutine consists of two ​<em><u>remote</u></em>​ “parallel” arrays:</li>

</ul>

○ An array of numbers (​not strings​), each one representing an LC3 opcode (i.e. #1, #5, etc.)

○ An array of strings, each one representing the corresponding LC3 Assembly Language (AL) instruction, in the same order as the opcodes.

(i.e. “ADD”, “AND”, etc.)

○ When invoked, the subroutine simply prints the tables as described.

<strong>Hints: </strong>

<ul>

 <li>The Op-code table from the text is provided at the end of this document.</li>

 <li>The two arrays will be stored remotely, with the remote addresses provided as local data <em><u>to the</u></em><u>​</u><em> <u>subroutine</u></em><u>​</u> ​<em>(this is so that a different subroutine can also access the same arrays) </em></li>

 <li>Store the array of opcodes as a list of .FILL pseudo-ops</li>

 <li>Store the array of AL instructions as a list of .STRINGZ pseudo ops</li>

</ul>

○ Terminate this array of strings with a .FILL #-1

<ul>

 <li>To iterate through the two arrays in parallel, keep a pointer to each array</li>

</ul>

○ Iterate through the ​<u>opcode</u>​ list one memory location at a time

○ You ​<em>could</em>​ print out each AL instruction using PUTS (Trap x22) – but we only have a pointer to the start of the entire aray of strings, i.e. the address of the first instruction!!

We don’t know the start address of the rest of them!

So you are going to use the starting address of the whole array, and iterate through it character by character, printing each with OUT (Trap x21), stopping at the #0 ​<em>(i.e. essentially make your own PUTS subroutine!)</em>

At that point, you will print the ” = “, print the opcode and a newline, and then increment the two array pointers and start over.

○ Print the opcode with the helper subroutine described below, passing it in R2

○ Quit when the instruction array pointer points to the value xFFFF = #-1 ​<em>(use BRn) </em>




You will need a helper subroutine to print the op-codes, a version of your Assignment 3: this subroutine will take a register parameter, skip the 12 MSBs, and print out just the 4 LSBs, as ascii 1s and 0s  ​<em>(</em><u>​<em>no</em></u><em> <u>terminating newline</u></em>​<em> – that will be up to the parent subroutine)</em>​.

So when passed e.g. the value #12 in R2, the sub will print out “1100” ;———————————————————————————————–

; Subroutine: SUB_PRINT_OPCODE

; Parameters: R2 containing a 4-bit op-code in the 4 LSBs of the register

; Postcondition: The subroutine has printed out just the 4 bits as 4 ascii 1s and 0s

;                               The output is NOT newline terminated.

; Return Value: None

;———————————————————————————————–

<strong>Test Harness: </strong>

Write a test harness that calls the SUB_PRINT_OPCODE_TABLE <em>(</em>​ <em>the SUB_PRINT_OPCODE will be called from inside that subroutine – this is the first time you will be using a nested subroutine call! Be very careful about backing up &amp; restoring ONLY the necessary registers). </em>




<strong><u>Fair Warning:</u></strong>

If you use .STRINGZ to simply store “ADD = 0001” (or any similar cheating hack-job) etc and print it out that way, you will not only get no credit for the lab, you will also receive a heavy sigh and will be walked away from in tired dismissal by the TA.

<h1>Exercise 02</h1>

Build a second pair of subroutines (same master/helper structure) that allow a user to repeatedly type in instruction names (example: “ADD”, “JSR”, “BR”) and be told whether the instruction is valid (whether the instruction exists).

;———————————————————————————————–

; Subroutine: SUB_FIND_OPCODE

; Parameters: None

; Postcondition: The subroutine has invoked the SUB_GET_STRING subroutine and stored a string

;                              as local data; it has searched the AL instruction list for that string, and reported

;               either the instruction/opcode pair, OR “Invalid instruction” ; Return Value: None

;———————————————————————————————–

;———————————————————————————————–

; Subroutine: SUB_GET_STRING

; Parameters: R2 – the address to which the null-terminated string will be stored.

; Postcondition: The subroutine has prompted the user to enter a short string, terminated

;                               by [ENTER]. That string has been stored as a null-terminated character array

;                               at the address in R2

; Return Value: None (the address in R2 does not need to be preserved)

;———————————————————————————————–

<strong>Specifications: </strong>

<ul>

 <li>The FIND subroutine invokes the GET_STRING sub, which prompts the user to type an [ENTER]-terminated string, which is stored as local data in the FIND sub (make sure to allocate enough memory locally for this string)</li>

 <li>The input string is compared with the array of LC3 instructions.</li>

</ul>

As in the previous subroutine, the two arrays are accessed via locally-stored addresses.

<ul>

 <li>If the input string matches one of the instructions, then that line from the opcode table is printed out. Otherwise “Invalid instruction” is printed.<strong>            </strong></li>

</ul>

<strong>Examples: </strong>

<ul>

 <li>The user types “JSRR[ENTER]”

  <ul>

   <li>The subroutine prints “JSRR = 0100”</li>

  </ul></li>

 <li>The user types “AMD[ENTER]”

  <ul>

   <li>The subroutine prints “Invalid instruction”</li>

  </ul></li>

</ul>

<strong>Test Harness: </strong>

Just add a call to SUB_FIND_OPCODE to your harness for exercise 01