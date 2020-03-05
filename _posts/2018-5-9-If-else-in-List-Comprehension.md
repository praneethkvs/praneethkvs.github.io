---
layout: post
title: If Else in List Comprehension, Python
---

In a python List Comprehension, the usual style of using an if statement is as below:

```python
["Even" for el in range(1,7) if el%2 == 0]
```
which goves you the output:  
['Even', 'Even', 'Even']

But if we want to add an else clause to the above list the most intuitive way is to just add the else clause at the end as below:
```python
["Even" for el in range(1,7) if el%2 == 0 else "Odd"]
```
If you try to run the above code python throws an error, so we need to use a conditional expression which is not part of the list
comprehension syntax.

The correct way to do this is as follows:
```python
["Even" if el%2 == 0 else "Odd" for el in range(1,7)]
```
which gives the output:  
['Odd', 'Even', 'Odd', 'Even', 'Odd', 'Even']

Multiple if else conditionals can be used in a list comprehension. But the ternary form of the conditionals does not have an
elif construct but it can be simulated using the else condition as follows:
```python
["Even" if el%2==0 else "divby3" if el%3==0 else "divby5" if el%5==0 else "None" for el in range(1,15)]
```
This is exactly like saying:
```python
for v in l:
    if v == 1 :
        print 'yes'
    else:
        if v == 2:
            print 'no'
        else:
            print 'idle'
```

which gives the output:  
['None', 'Even', 'divby3', 'Even', 'divby5', 'Even', 'None', 'Even', 'divby3', 'Even', 'None', 'Even', 'None', 'Even']


We can also frame more than one for loop in the form of a list comprehension.
Let's take the the following piece of code which has two for loops.  

```python

arr = [10,8,7,14,5]

for i in range(0,len(arr)):
        for j in range(0,i):
            if arr[j] < arr[i]:
                print(arr[i]-arr[j])
```
  
The same piece of code in list comprehension is as follows:
```python
[arr[i]-arr[j] for i in range(0,len(arr)) for j in range(0,i) if (arr[j] < arr[i])]
```  
