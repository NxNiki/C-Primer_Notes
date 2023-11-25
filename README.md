# Cpp Primer_Notes
notes of book C++ Primer

## compiling a C++ program:
By default, the command to run the GNU compiler is g++:

`g++ -o prog1 prog1.cc`

The -o prog1 is an argument to the compiler and names the file in which to put the executable file. This command generates an executable file named prog1 or prog1.exe, depending on the operating system. Compilers usually include options to generate warnings about problematic constructs. It is usually a good idea to use these options. Our preference is to use -Wall with the GNU compiler, and to use /W4 with the Microsoft compilers.

## section 1.4
When we enter input to a program from the keyboard, different operating systems use different conventions to allow us to indicate end-of-file. On Windows systems we enter an end-of-file by typing a control-z—hold down the Ctrl key and press z— followed by hitting either the Enter or Return key. On UNIX systems, including on Mac OS X machines, end-of-file is usually control-d.

## section 1.5.1
Every class defines a type. The type name is the same as the name of the class.
the class author determines all the operations that can be used on objects of the class type.

Most operating systems support ﬁle redirection, which lets us associate a
named ﬁle with the standard input and the standard output:
`$ addItems <infile >outfile`
Assuming $ is the system prompt and our addition program has been compiled into
an executable ﬁle named addItems.exe (or addItems on UNIX systems), this com-
mand will read transactions from a ﬁle named infile and write its output to a ﬁle
named outfile in the current directory.




