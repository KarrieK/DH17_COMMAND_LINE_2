# Command line tools #

## Review ##

In our last session we learnt some unix commands and can now move files around our filesystem like a pro - time to start digging into our data. ALl without using a mouse!

Csvkit is the king of csv wrangling libraries. It converts, greps, sorts and outputs large sets of data so you don't need to use Excel or SQL.

Csvkit is flexible, easy to use and powerful. It is not limited to a million rows of data like excel, yet is time efficient so you aren't waiting for queries like SQL. It's a great stepping stone to learning how to program.

##Commands we're going to cover: ##

* in2csv - converts a file into a csv
* csvlook - gives us a preview of our data
* csvcut - cutting too for manipulating csvs
* csvclean - cleans our csv for errors
* csvstat - gives us descriptive stats for the content of our csv
* csvsort - sorts the contents of a csv file
* csvgrep - regex command like a refined search function in our csv
* head - limits data to the top ten rows

## Installation ##

You can use `sudo easy_install csvkit` to install csvkit. Shout if that works if not there are some other options

Install Homebrew. Then do pip install csvkit at the command line. You might need to use `pip3 install csvkit`if you're running python 3 - if this doesn't work we need to bump it up a level. Time to break out the sudo (super user) command. Try `sudo pip install csvkit` 

# Looking at our data

* in2csv
* csvlook
* csvcut

Because you're all fabulous we can skip step one. Our data is already in csv format but if it's not you can convert from xlsx to csv using in2csv

`in2csv sampledata.xlsx > data.csv`

Ok are we in the right folder?

Let's take a look at what we've got

`csvlook TED_CAN_2015.csv`

Your data is probably going to look like a mess. The rows and columns can't all fit in the width of your terminal and so it's going to be pretty dificult to make any sense of what you've got.

We need to refine and figure out the schema of what we're looking at.

csvcut -n data. csv 
The -n means that we should see the names of the columns

This is about the time that we grab a data dictionary to figure out what each column means

##Data formatting

csvclean
csvstat
Let's clean the data quickly to make sure it's formatted correctly.

csvclean IRSdata.csv
Now we get look at some basic stats to look for trends in our data.

csvstat IRSdata.csv
This is another handy way of checking how clean your data is - look for values that are out of place like if there was more than 12 unique values for Month.

Time to start wrangling.

##Data Wrangling

head - limits to the top 10 rows
csvcut - slices away segements of our data
csvgrep - Regular expression allowing us to filter our data
csvsort - sorts the data
Now we start combining our functions to build queries to explore our data.

We use | or a pipe to combine or stack the commands together to build more powerful queries.

Let's have a look at which states have IRS 990 organisations.

csvcut -c STATE IRSdata.csv | csvlook
We want to see what sort of data we have - so let's use head and have a look at the first 10 rows

csvcut -n IRSdata.csv | csvlook | head
Unfortunately that doesn't tell us much so let's look and see if we can get some summary stats on the states.

csvcut -c STATE IRSdata.csv | csvstat | csvlook
Which state has the most amount of organisations? But what does that mean?

Let's take a look to see if there are multiple organisations of the same name in the data set

csvcut -c NAME IRSdata.csv | csvstat | csvlook
Let's combine STATE and INCOME_AMT to see what we get

csvcut -c STATE,INCOME_AMT IRSdata.csv | csvlook
Obviously there are more organisation in Maine than other states. Let's use csvgrep to take a closer look at what's going on in Maine

csvcut -c STATE,INCOME_AMT IRSdata.csv | csvgrep -c STATE -m ME | cvstat | csvlook
What is the total income amount for all 990 organisations in ME? $10,043,698,930 How many organisations have 0 income? What is the maximum income? How many rows? What are the 5 most frequent values?

Digging deeper

Let's build a query to figure out the names of the organisations with the highest incomes

csvcut -c STATE,INCOME_AMT IRSdata.csv | csvgrep -c STATE -m ME | csvsort -c INCOME_AMT | csvlook | head 
Csvkit has sorted the data by INCOME-AMT from lowest to highest - this isn't very convienent so we are going to use the -r flag (reverse) to reverese the sorting order

csvcut -c STATE,INCOME_AMT IRSdata.csv | csvgrep -c STATE -m ME | csvsort -c INCOME_AMT -r | csvlook | head 
Now see if we can figure out which organisation in Maine has an income amount of over $2 billion

csvcut -c STATE,INCOME_AMT,NAME IRSdata.csv | csvgrep -c STATE -m ME |csvsort -c INCOME_AMT -r | csvlook | head 
Who is on top? If you are a local paper then maybe there are some surprising results here

##Finishing up

Today we only touched briefly on csvkit - while used csvkit briefly in a bash shell you can combine it with python or SQL to build powerful queries and keep interrogating your data.

Here are some other custom commands we didn't touch on:

csvsql - Generates SQL statements for a csv or execute those statements directly on a database. In the latter case supports both creating tables and inserting data
csvpy - Loads a CSV file into a csvkit.CSVKitReader object and then drops into a Python shell so the user can inspect the data however they see fit
csvformat - Converts a csv to a custom output format
csvjson - Converts a CSV file into JSON or GeoJSON
##Resources

If you want to dig deep and get to know csvkit better then check out the great documentation and tutorial on readthedocs.

https://csvkit.readthedocs.org/en/0.9.1/tutorial.html
https://source.opennews.org/en-US/articles/eleven-awesome-things-you-can-do-csvkit/
##Contact me

Email: karrie.anne.kehoe@gmail.com Twitter: @karriekehoe
