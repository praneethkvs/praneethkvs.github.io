---
layout: post
title: to.csv() in Pandas formats string as Date.
---

While Using the to.csv() function to write a Pandas DataFrame to a csv file, if your dataframe has ratios (1/4 shows up as Jan-4th), these are automatically parsed 
as a date when you open up the csv in excel and hence the original interpretation is lost, even though the underlying data is in the correct
format. Excel automatically parses it as a date and displays it to us.

To work around this, just save the csv file as a .txt file and then open it from Excel which will launch the Import Data Wizard, select
the delimited option and which will give us an option to choose the data type for each of the columns, changing the type for the column in question to text will retain it as it is without parsing it as a date.
