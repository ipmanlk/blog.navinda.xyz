+++
title = "Bash Parameters"
description = ""

[taxonomies]
tags = ["bash", "shell", "linux"]
categories = ["scripting"]
+++

_This is simply a guide with important information from the Bash manual. If you want more in-depth explanations, please refer to Bash man pages._

A parameter is an entity that stores values. There are three types of parameters in Bash.

- Variables (User-defined/Shell)
- Positional parameters.
- Special parameters.

## Variables
### User defined variables
These are variables created by users. These are usually named in lowercase and underscore is used to separate words.

```bash
# This will create a variable named user_defined_var with the value 10
user_defined_var=10
```
If we want to print the value of a variable, we can use the **parameter expansion**.
```bash
# We are referencing a variable here
echo "Value is: ${user_defined_var}"
```
The above script will output ``Value is 10``.


### Shell variables
These are commonly known as environmental variables. There are two types of shell variables in Bash.
- Bourne Shell Variables (10 in total)
- Bash Shell Variables (Around 95 in total)

Bash is based on the original Bourne shell, which is much older. Shell variables that are available in both Bourne shell and Bash shell are known as Bourne shell variables. Shell variables that were introduced much later in Bash, are known as Bash shell variables.

Shell variables have special meanings. Bash populates some of these variables with useful information and others can be used with commands to achieve specific outcomes. 

Ex,
- ``$USER``: contains the username of the currently logged in user
- ``$HOSTNAME``: contains the computer (host) name
- ``$PS1``: contains the prompt of the shell

All of the shell variables have uppercase names. This is the reason for defining user variables in lowercase letters, to avoid any confusion. 

### Positional parameters
These are parameters set by the shell when we pass command line arguments to shell scripts. Each argument needs to be separated by a space.

```bash
$ my_script.sh 1 2 3 4
```
In the above my_script.sh file, we can access arguments 1,2,3,4 using positional parameters ``$1``, ``$2``, ``$3``, and ``$4`` respectively.

It's important to remember that we can't set these positional parameters manually. For example, the following script will fail.

```bash
$ 1="test"
-bash: 1=test: command not found
```
This is shell protecting how positional parameters work. These numeric parameter names are created by the shell based on the given arguments automatically.

Also, there is no limit to how many positional arguments that can be created. However, **if there are more than nine positional parameters, advanced parameter expansion (``${}``) must to be used**.

```bash
echo "$10" # This will output an invalid value. Only $1 will be considered
echo ${10} # This will output the correct value
```

### Special parameters
Parameters that Bash gives special meaning to are known as special parameters. These are very much like shell variables. But unlike them, **we can't set the values of special parameters manually**.

The value of a special parameter is calculated for us by the shell based on the script we are currently running. These variables provide very useful information. 

Following is a list of special parameters and information they provide. I'm using ``$`` in front of each parameter name because it's more clear. But remember that ``$`` simply means expansion. Without it, you get the parameter name, not the value.

#### ``$#`` 
This provides the number of positional parameters given to a script.
```bash
$ ./test.sh 1 2 # if we expand #, output will be 2
```

#### ``$0``
When used inside a script, this provides the full path of that script (script name).

#### ``$@``
This provides all positional parameters available to a script as individual words and every word is subjected to word splitting. When wrapped inside double quotes (``"$@"``), each word will be wrapped in double quotes and won't be subject to word splitting.

Imagine we are passing full names of individuals to a script, if we use ``$@`` without double quotes, the output will be something like the following.

```bash
$ ./test.sh "max payne" "carl thompson" 
max payne carl thompson # 4 words due to word splitting in positional parameters.
```
If we use double quotes (``"$@"``), things will change.
```bash
$ ./test.sh "max payne" "carl thompson" 
max payne carl thompson # 'max payne' 'carl thompson' will be the result here. No world splitting in positional parameters.
```

#### ``$*``
When unquoted, this is the same as unquoted ``$@``. But when quoted, this will put the first character of the IFS variable between positional parameters and provide as a single word.

```bash
$ ./test.sh 1 2 3 # quoted result
1,2,3
```