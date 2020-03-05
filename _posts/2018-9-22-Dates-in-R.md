---
layout: post
title: Dealing with Dates in R
---

To convert a character object into a date format, use the `as.POSIXlt()` function.
This takes two arguments, the character object and a format specifying the date format as represented in the character object.

So if we have something like 
```R
char <- "9/21/2018 17:22"
date <- as.POSIXlt(char,format="%m/%d/%Y %H:%M")
```

And, to extract dates from the date object we can use the format function.
```R
day <- format(date,"%d")
hour <- format(date,"%H")
```
Note: If you are going to work with the dplyr package, you might get an error saying 
`Column Date is of unsupported class POSIXlt/POSIXt`, just use `as.POSIXct()` instead and it will work fine.
   

![Codes]({{ site.baseurl }}/images/posixltcodes.PNG "POSIXlt Codes")
 
