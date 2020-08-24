# Program Arguments

## Usage

* You call your program **from your terminal** like: 

```bash
./myprogram arg1 arg2 arg3		# Here ./ means current directory
diff file1 file2
rm file
```

* Why program arguments (not reading from input)?
  * Options are recommended to be passed as program arguments, while data are more recommended to be passed as  standard input or files
  * When typing commands in terminal, there may be autocompletion (press `tab`): lower the risks of typo
  * Single commands can compose larger shell scripts

> **Multiple commands in one line**
>
> ```bash
> g++ test.cpp -o test && ./test	# Run immediately after compilation
> ```

> **Simplest Shell Script** (Linux or OSX)
>
> * Create a file ended with ".sh": `touch test.sh`
> * Edit the shell script:
>
> ```
> #!/bin/bash
> clear									# Clear the screen
> echo "Hello World"		# Greeting :)
> ```
>
> * Chang the permission of this file:
>
> ```bash
> chmod +x ./test.sh
> ```
>
> * Run the script: `./test.sh`

## Parsing Aruguments

* int main(int argc, char *argv[])
  * argc: Argument count (the count of words seperated by whitespace in one command)
  * argv: Argument vector/values

* All arguments passed to `main` in C/C++ are **C-strings**(char *argv[]))
  * Because `string` is a class provided in C++, but not a primitive data type
* You may need to do type conversion before you want to use the arguments, and throw exceptions when arugments are not as expected.

## Advanced Parsing

* Search for `getopt` or `getopt_long`, these are ways of analyzing a bunch of program arguments as **options**.
* Used in VE281. May not be allowed in VE280 (or not necessary)

## Args in CLion

<img src="../img/clion-terminal.png" alt="clion-terminal" style="zoom:50%;" />



## Credit

SU2019 & SU2020 VE280 Teaching Groups.

VE280 Lecture 9