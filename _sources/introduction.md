# Introduction

## Programs and Programming

- Programming teaches you how to make computers do what you want them to do. 
- A program is a set of detailed step-by-step instructions to a computer. It is written in terms of a few basic operations that its reader already understands. Using these few basic operations, you can "teach" a computer new operations by defining them in terms of the basic operations.
  - Example: Computers understand the addition and division operator, which are the basic mathematical operators. You can teach the 
  computer to calculate the average by using the addition and division operator by instruction the computer to add all the numbers in a sequence and divide by the size of the sequence. 
  - You can then use the new average operation and combine with other operations to create more operations. It’s a lot like creating life by putting atoms together to make proteins and then combining proteins to build cells, combining cells to make organs, and combining organs to make a creature.
  Defining new operations and combining them to do useful things is the heart and soul of programming.

## What is a Programming Language?

- You can express directions to the nearest bus station in many different languages such as English, Spanish, or Hindi. 
- Similar to natural languages, there are many programming languages. But they all are instructions that a machine
can understand. Programming languages can also look different. For example `3 + 4` in Python means add three to four. This same
instruction in Schema is express as `(+ 3 4)`.  They are both express the same idea-- they just look different. 
- Every programming language has a way to write mathematical expressions,
repeat a list of instructions a number of times, choose which of two instructions
to do based on the current information you have, and much more.

## What does a programming language do?

- It takes a high-level human readable language expression such as `c = a + b` and translates to machine language that the computer can execute.
- Compilers convert programs written in a high-level language into the machine language of some computer.
- Interpreters simulate a computer that understands a high-level language.
- The source program is not translated into machine language all at once.
- An interpreter analyzes and executes the source code instruction by instruction (line-by-line).
- Compiling vs. Interpreting
- Once program is compiled, it can be executed over and over without the source code or compiler. If it is interpreted, the source code and interpreter are needed each time the program runs
- Compiled programs generally run faster since the translation of the source code happens only once.
- Interpreted languages are part of a more flexible programming environment since they can be developed and run interactively
- Interpreted programs are more portable, meaning the executable code produced from a compiler for a Pentium won’t run on a Mac, without recompiling. If a suitable interpreter already exists, the interpreted code can be run with no modifications.

## Ambiguity in Language 

- Natural Language can have ambiguity. 
- English example: "I saw a man on a hill with a telescope."
  - There’s a man on a hill, and I’m watching him with my telescope.
  - There’s a man on a hill, who I’m seeing, and he has a telescope.
  - There’s a man, and he’s on a hill that also has a telescope on it.
  - I’m on a hill, and I saw a man using a telescope.
  - There’s a man on a hill, and I’m sawing him with a telescope.
  - source: https://www.quora.com/What-are-some-examples-of-ambiguous-sentences


:::{warning}
<strong>Bad code can be ambiguous due to human error. To the machine it is not ambiguous.</strong> When this happens, the program might behave unexpectedly. This unexpected behavior is called a bug!
:::
