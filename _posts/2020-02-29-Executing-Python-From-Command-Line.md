---
layout: post
title: Parsing Arguments from command line to Python Scripts.
---

If you want to pass arguments to a python script at runtime through the command line, instead of pre-defining them inside the script, we can do this using the library argparse, which parses the commands input from the command line and passes them on to functions in your script.

<b> Parsing Arguments.</b>

Let's create a python script `add.py` which has a simple function that adds two numbers.

```python
def my_add(a,b):
    return(a+b)


if __name__=="__main__":
    print(my_add(2,3))
```    

If you run this script from your shell `python add.py`, you should see 5, since you passed 2 and 3 as arguments to the function. Now if you want to change these values you would have to go into the script and change them manually. Now let's use argparse so that these arguments can be passed from the commandline at runtime.

```python
import argparse

parser = argparse.ArgumentParser('Input Numbers to Add')
parser.add_argument('a',type=int,help='Input First Number')
parser.add_argument('b',type=int,help='Input Second Number')

args= parser.parse_args()


def my_add(a,b):
    return(a+b)


if __name__=="__main__":
    print(my_add(args.a,args.b))

```
Now you can run this from the command line and pass in arguments to the `my_add` function as follows `python add.py 10 8`. If you try to run it without passing the arguments, it should throw an error, since these arguments are positional and the script cannot run without the arguments.

<b>Optional Arguments.</b>

If you run `python add.py --help` , it will display information that we entered in the help of add_argument(). The Leading -- indicate that it is an optional argument i.e. running the script without specfying this argument does not throw an error. Let's add some optional arguments.

```python
import argparse

parser = argparse.ArgumentParser('Input Numbers to Add')
parser.add_argument('a',type=int,help='Input First Number')
parser.add_argument('b',type=int,help='Input Second Number')
parser.add_argument('--opt_arg',type=str,help='Optional Argument')

args= parser.parse_args()

def my_add(a,b):
    return(a+b)

if __name__=="__main__":
    print(my_add(args.a,args.b))
    print(args.opt_arg)
```
To pass in the optional argument you need to specify the name of the argument,   
`python add.py 20 5 --opt_arg Optional_Argument`

This should print out the Optional Argument along with the sum of the numbers.

<b>Arguments as a list.</b>

You can also pass in a list of arguments by using `nargs` as follows:
```python
import argparse

parser = argparse.ArgumentParser('Input Numbers to Add')
parser.add_argument('nums',type=int,help='Input List of Numbers to be summed',nargs="*")


args= parser.parse_args()

print(args)

def my_add(nums):
    res = 0
    for num in nums:
        res += num
    return(res)


if __name__=="__main__":
    print(my_add(args.nums))
    
    if args.opt_arg is not None:
        print(args.opt_arg)
```
Now you can run `python add.py 1 2 3 4`

<b> Parsing Unknown Arguments.</b>

If you pass arguments to add.py that are not defined in the argument parser, it throws an error, Sometimes a script may only parse a few of the command-line arguments, passing the remaining arguments on to another script or program. In these cases, the parse_known_args() method can be useful. It works much like parse_args() except that it does not produce an error when extra arguments are present. Instead, it returns a two item tuple containing the populated namespace of known arguments that are defined in the argument parser and the list of remaining argument strings that are unkown.

```python
import argparse

parser = argparse.ArgumentParser('Input Numbers to Add')
parser.add_argument('nums',type=int,help='Input List of Numbers to be summed',nargs="*")

args,unkown_args= parser.parse_known_args()

print(args)
print(unkown_args)

def my_add(nums):
    res = 0
    for num in nums:
        res += num
    return(res)


if __name__=="__main__":
    print(my_add(args.nums))
```
Running `python add.py 1 2 3 4 -opt Uknown_arg` will run without any errors although the only argument defined is the list nums. 
