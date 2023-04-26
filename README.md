# THE PROJECT
This repository or directory shows solutions to ALX team project that is provided by Chiamaka Anachusi and Tosin Iyiola.

# GENERAL REQUIREMENT FOR THE PROJECT
°Allowed editors includes includes vi,vim,emacs
°All files will be compiled on Ubuntu 20.04 LTS using gcc, using the options -Wall -Werror -Wextra -pedantic -std=gnu897
°All files should end with a new line
°A README.md file, at the root of the folder of the project is mandatory. Also, write a README with the description of the project
°The use of the Betty style. It will be checked using betty-style.pl and betty-doc.pl
°Shell should not have any memory leaks
°No more than 5 functions per file
°All header files should be include guarded

# DESCRIPTION
A shell is a command-line interface (CLI) that allows users to interact with unix-like operating system (OS) by entering commands, executing programs, and managing files and directories. It is a basic version of a shell that provides a limited set of features and functions compared to more advanced shells like bash, zsh, and ksh.
The unix shell includes the following functionality:
1.Command execution: it allows users to execute system commands, such as ls, cd, and echo, as well as execute programs and scripts stored in the file system.
2.Command history: it maintains a record of the commands that have been executed, which can be accessed using the up and down arrow keys.
3.Command line editing: it provides basic line editing features, such as the ability to delete and recall previous commands, insert and delete text, and move the cursor left and right.
3.Variable substitution: it allows users to set and use environment variables to store and retrieve values, such as the current working directory (pwd) or the value of the PATH variable.
4.Wildcard expansion: it supports wildcard characters, such as * and ?, which can be used to match multiple files in a directory.
5.Pipe and redirection: it supports the use of pipes (|) to connect the output of one command to the input of another command, and redirection (> and <) to redirect input and output from and to files.

# INVOCATION
Usage: shell [filename]
To invoke shell, compile all .c files in the repository and run the resulting executable:
 gcc * .c -o shell
./shell

Shell can be invoked both interactively and non-interactively. If shell is invoked with standard input not connected to a terminal, it reads and executes received commands in order.

Example:

$ echo "echo 'hello'" | ./shell
'hello'
$
If shell is invoked with standard input connected to a terminal (determined by isatty(3)), an interactive shell is opened. When executing interactively, shell displays the prompt $  when it is ready to read a command.

Example:

$./shell
$
Alternatively, if command line arguments are supplied upon invocation, shell treats the first argument as a file from which to read commands. The supplied file should contain one command per line. Shell runs each of the commands contained in the file in order before exiting.

Example:

$ cat test
echo 'hello'
$ ./shell test
'hello'
$

# ENVIRONMENT
Upon invocation, shell receives and copies the environment of the parent process in which it was executed. This environment is an array of name-value strings describing variables in the format NAME=VALUE. A few key environmental variables are:

# HOME
The home directory of the current user and the default directory argument for the cd builtin command.

$ echo "echo $HOME" | ./shell
/home/projects

# PWD
The current working directory as set by the cd command.

$ echo "echo $PWD" | ./shell
/home/projects/alx/simple_shell

# OLDPWD
The previous working directory as set by the cd command.

$ echo "echo $OLDPWD" | ./shell
/home/projects/alx/printf

# PATH
A colon-separated list of directories in which the shell looks for commands. A null directory name in the path (represented by any of two adjacent colons, an initial colon, or a trailing colon) indicates the current directory.

$ echo "echo $PATH" | ./shell
/home/projects/.cargo/bin:/home/projects/.local/bin:/home/projects/.rbenv/plugins/ruby-build/bin:/home/projects/.rbenv/shims:/home/projects/....

# COMMAND EXECUTION
After receiving a command, shell tokenizes it into words using " " as a delimiter. The first word is considered the command and all remaining words are considered arguments to that command. shell then proceeds with the following actions:
If the first character of the command is neither a slash (\) nor dot (.), the shell searches for it in the list of shell builtins. If there exists a builtin by that name, the builtin is invoked.
If the first character of the command is none of a slash (\), dot (.), nor builtin, shell searches each element of the PATH environmental variable for a directory containing an executable file by that name.
If the first character of the command is a slash (\) or dot (.) or either of the above searches was successful, the shell executes the named program with any remaining given arguments in a separate execution environment.

# EXIT STATUS
shell returns the exit status of the last command executed, with zero indicating success and non-zero indicating failure.
If a command is not found, the return status is 127; if a command is found but is not executable, the return status is 126.
All builtins return zero on success and one or two on incorrect usage (indicated by a corresponding error message).

# SIGNAL
While running in interactive mode, shell ignores the keyboard input Ctrl+c. Alternatively, an input of end-of-file (Ctrl+d) will exit the program.
User hits Ctrl+d in the third line.
       $ ./shell
       $ ^C
       $ ^C
       $
# VARIABLE REPLACEMENT
shell interprets the $ character for variable replacement.

# $ENV_VARIABLE
ENV_VARIABLE is substituted with its value.

Example:
$ echo "echo $PWD" | ./shell
/home/projects/ALX/simple_shell

# $?
? is substitued with the return value of the last program executed.

Example:

$ echo "echo $?" | ./shell
0

# $$
The second $ is substitued with the current process ID.

Example:

$ echo "echo $$" | ./shell
6494

#COMMENTS
shell ignores all words and characters preceeded by a # character on a line.

Example:

$ echo "echo 'hello' #this will be ignored!" | ./shell
'hello'

# OPERATORS
shell specially interprets the following operator characters:

# ;- COMMAND SEPARATOR
Commands separated by a ; are executed sequentially.

Example:

$ echo "echo 'hello' ; echo 'world'" | ./shellex
'hello'
'world'

# && - AND logical operator
command1 && command2: command2 is executed if, and only if, command1 returns an exit status of zero.

Example:
$ echo "error! && echo 'hello'" | ./shell
./shell: 1: error!: not found
$ echo "echo 'all good' && echo 'hello'" | ./shell
'all good'
'hello'

# || - OR logical operator
command1 || command2: command2 is executed if, and only if, command1 returns a non-zero exit status.

Example:
$ echo "error! || echo 'but still runs'" | ./shell
./shell: 1: error!: not found
'but still runs'
The operators && and || have equal precedence, followed by ;.

# SHELL BUILTIN COMMANDS

# cd
°Usage: cd [DIRECTORY]
°Changes the current directory of the process to DIRECTORY.
°If no argument is given, the command is interpreted as cd $HOME.
°If the argument - is given, the command is interpreted as cd $OLDPWD and the pathname of the new working directory is printed to standad output.
°If the argument, -- is given, the command is interpreted as cd $OLDPWD but the pathname of the new working directory is not printed.
°The environment variables PWD and OLDPWD are updated after a change of directory.
Example:

$ ./shell
$ pwd
/home/projects/ALX/simple_shell
$ cd ../
$ pwd
/home/projects/ALX
$ cd -
$ pwd
/home/projects/ALX/simple_shell

# alias
°Usage: alias [NAME[='VALUE'] ...]
°Handles aliases.
°alias: Prints a list of all aliases, one per line, in the form NAME='VALUE'.
°alias NAME [NAME2 ...]: Prints the aliases NAME, NAME2, etc. one per line, in the form NAME='VALUE'.
°alias NAME='VALUE' [...]: Defines an alias for each NAME whose VALUE is given. If name is already an alias, its value is replaced with VALUE.

# exit
°Usage: exit [STATUS]
°Exits the shell.
°The STATUS argument is the integer used to exit the shell.
°If no argument is given, the command is interpreted as exit 0
Example:

        $ ./shell
        $ exit

# env
°Usage: env
°Prints the current environment.
Example:

         $ ./shell
         $ env
         NVM_DIR=/home/ptojects/.nvm
         ..

# setenv
°Usage: setenv [VARIABLE] [VALUE]
°Initializes a new environment variable, or modifies an existing one.
°Upon failure, prints a message to stderr.
Example:

        $ ./shell
        $ setenv NAME katty
        $ echo $NAME
        katty

# unsetenv
°Usage: unsetenv [VARIABLE]
°Removes an environmental variable.
°Upon failure, prints a message to stderr.
Example:

      $ ./shell
      $ setenv NAME katty
      $ unsetenv NAME
      $ echo $NAM
      $

© AUTHORS
1. Chiamaka Anachusi - [Linkedin](https://www.linkedin.com/in/chiamaka-anachusi-b-sc-pgde-aca-82a90baa) | [Twitter](https://twitter.com/AnachusiChiamak) | [Facebook](https://facebook.com/Amakasim) 

2. Tosin Iyiola - [github](https://github.com/Tosinval) 

# REFERENCE AND ACKNOWLEDGEMENT
This README borrows from the [Linux man pages](https://linux.die.net/man/1/sh)(https://linux.die.net/man/1/dash) 
This project was written as part of the curriculum of the ALX-SE programme by Holberton School. Holberton School is a campus-based full-stack software engineering program that prepares students for careers in the tech industry using project-based peer learning. For more information about ALX, visit [Alx site](https://www.alxafrica.com) 

