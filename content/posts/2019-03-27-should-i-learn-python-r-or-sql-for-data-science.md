---
title: Should I learn Python, R or SQL for data science?
author: Steve Haines
type: post
date: 2019-03-27T11:00:24+00:00
weight: 2
url: /2019/03/27/should-i-learn-python-r-or-sql-for-data-science/
featured_image: https://storage.googleapis.com/wordpress-sqlsteve/2019/03/a061e0cf-image.png
categories:
  - Discussion
tags:
  - Beginner
  - Data Science
  - Learning
  - Python
  - R
  - SQL

---
The question &#8216;Should I learn Python, R or SQL for data science?&#8217; is a common one amongst new starters in the field of data science. I have often seen blogs explaining for instance how: _&#8220;Python is better as it will allow you to do other things as well as data science&#8221;_ or  _&#8220;R is better at doing statistical analysis&#8221;_ and so on.

### Mind the knowledge gap?

While it&#8217;s perfectly valid to hold an opinion on these things as an experienced data developer, I have often thought that for beginners, statements like these can be unhelpful. I think this is because they can promote the idea that one of these languages is inherently more valuable than another.

Furthermore, I worry that people might hear that learning Python for example is _the only way_ to get into data science. They might then spend a lot of time learning Python and not solving common data challenges such as preparing and cleansing data, selecting models and applying tests.

### 80/20 rule strikes again

These common tasks fall under the umbrella of an area of data science called &#8216;[data wrangling&#8217;][1]. It has often been argued that data scientists spend up to 80% of their time on these tasks. This is exactly the opposite of what most people expect data scientists to be doing. (e.g. wearing lab coats, using scientific calculators etc.)<figure class="wp-block-pullquote is-style-solid-color">

>  
> Data scientists, according to interviews and expert estimates, spend from 50 percent to 80 percent of their time mired in this more mundane labor of collecting and preparing unruly digital data, before it can be explored for useful nuggets.
> 
> <cite><br />Steve Lohr, New York Times <br />Aug. 17, 2014<br /></cite></figure> 

It&#8217;s my belief that all of these languages are equally good at &#8216;data wrangling for beginners&#8217; and that the answer to the question which language should I learn for data science is &#8216;any or ideally all of them&#8217;. I would argue that if you are just starting off on your data science journey, any of these languages would be a reliable toolkit for tackling the challenges you need to overcome on the way.

The important thing to do as a beginner, is to get some data from a real world scenario and try to draw an inference from the data that allows you to learn something about the subject matter. The skills that you learn by doing this are entirely transferrable to any programming language.

### Background on these three languages

It&#8217;s worth giving some background on these three languages as there are some significant differences in design principles and architecture. SQL is a [declaratively written][2] query language which you use to communicate with the SQL engine. The language syntax is built around the idea of querying the database engine about the data in an English-like syntax. It is designed for working with relational data and is unlikely to perform well in other scenarios. SQL has been around since the late 70s in it&#8217;s various forms.

[Python][3] and [R][4] by comparison, are both interpreted multi-model languages which support for example functional programming as well as imperative and object-orientated programming (and others). These languages, both created in the early 90s, can be used for a variety of tasks and are by no means as data bound as SQL will always be. R was specifically created to be a re-imagining of the statistical computation language called [S][5] which is much older. Python was created by a single [guy][6] who wanted to create a clean and simple yet powerful language that allowed developers to get started quickly.  


### Four common data wrangling tasks.﻿

I&#8217;m going to try to avoid comparing &#8216;features and benefits&#8217; of each language as there are many hundreds of articles around the internet that provide this already. Instead, I will aim to demonstrate their core similarities by doing the same data wrangling task that you would do as part of a normal data analysis job in each of the different languages. 

The tasks we will use to test my theory are as follows:

  1. Data Load
  2. Looking at top 10 and bottom 10 values to see the type of values or the range of a column
  3. Summarising and aggregating data
  4. Visualising data

The data I am using as part of this experiment is meteorological outdoor temperature readings taken from the roof of the Create Centre in Bristol on an hourly basis. The source for this data is [here][7].

### 1. Data Load

Below you can see how each of these languages would typically handle the task of loading data into a structure that allows for easier analysis later on. In Python and R, we load into Data Frames ([Explanation of Data Frames here][8]) which are essentially the same kind of two-dimensional data structure with columns and rows that are called tables in SQL except data frames in both of these languages are held in-memory instead of on disk. While it is possible to hold a table in-memory in SQL as well, in SQL Server this requires a lot more effort.

SQL requires us to create a physical relational table on disk then insert the data from the CSV starting on Row 2.

<pre class="brush: sql; title: ; notranslate" title="">CREATE TABLE temps ([Date Time] datetime, Temperature decimal(5,2))
BULK INSERT temps FROM 'C:\Users\Steve\Downloads\meteorological-data-create.csv'
WITH (FIRSTROW = 2, FORMAT='CSV');
</pre>

Python has many libraries available to manage data, here we are using the Pandas library to automatically read the CSV with default settings into a new dataframe called temps.

<pre class="brush: python; title: ; notranslate" title="">import pandas as pd
temps = pd.read_csv("meteorological-data-create.csv")
temps
</pre>

In R we are using a built-in library to read the CSV into a new dataframe using some simple settings.

<pre class="brush: r; title: ; notranslate" title="">temps &lt;- read.csv(file="meteorological-data-create.csv" header=TRUE, sep=",") 
temps </pre>

### 2. Profiling and bottom 10 rows

As you can see, loading data in all of these languages is relatively easy and all of the languages have similarities in the syntax they use to do this. This is great for us as it means that the skills learnt in one language are almost entirely transferrable to another of these languages. Now we would like to profile our data and see the top 10 and bottom 10 temperatures available in the dataset.

A key difference between SQL and the others is that SQL allows us to re-use that physical table we created earlier even if we have shut down / restarted the server. This is because it&#8217;s stored on a physical disk. We do just that here and select the top 10 by specifying an order in the &#8216;order by&#8217;.

<pre class="brush: sql; title: ; notranslate" title="">SELECT TOP 10 * FROM temps ORDER BY [Date Time] ASC 
SELECT TOP 10 * FROM temps ORDER BY [Date Time] DESC 
</pre>
![](https://storage.googleapis.com/wordpress-sqlsteve/2019/03/c856f7bb-image.png)


Python has some useful functions called &#8216;nlargest&#8217; and &#8216;nsmallest&#8217; in the pandas library, we use these here to return the top and bottom 10 rows.

<pre class="brush: python; title: ; notranslate" title="">temps.nlargest(10,'Temperature')
temps.nsmallest(10,'Temperature')
</pre>
![](https://storage.googleapis.com/wordpress-sqlsteve/2019/03/94bba37f-image.png)


R uses a similar process, where we sort the data frame and only fetch the 10 rows from the head of the frame. This returns an array:

<pre class="brush: r; title: ; notranslate" title="">head(sort(df1$Temperature,decreasing=TRUE), n = 10)
head(sort(df1$Temperature,decreasing=FALSE), n = 10)
</pre>
![](https://storage.googleapis.com/wordpress-sqlsteve/2019/03/6e2a304d-image.png)


It&#8217;s interesting to note here that our first data quality issue has arisin, but it&#8217;s only visible through the SQL query. We have discovered that there are NULL values in our dataset. This is visible through SQL as it treats NULL values as if they were &#8216;small values&#8217; rather than &#8216;non-values&#8217; as Python and R treat them. It&#8217;s an interesting difference between these languages and highlights why profiling and understanding data is such a valuable skill.

### 3. Summarising and aggregating data

Now we have some understanding of the range of the data, we might want to zoom in a bit closer and see the month minimum and maximum temperatures for each month across the whole dataset.

In SQL we would achieve this with some aggregate functions (MIN,MAX) and a &#8216;Group By&#8217; to aggregate the data. We group all the dates in a single month into a single row for each month by computing a new date using the helpful function DATEFROMPARTS which creates a new day from integers for year, month and day, we simply add 1 as the day then add this new field to the Group By as so:  


<pre class="brush: sql; title: ; notranslate" title="">SELECT	
        DATEFROMPARTS(YEAR([Date Time]),MONTH([Date Time]),'01') AS [Month],
	MIN(Temperature) AS [MinTemp], 
	MAX(Temperature) AS [MaxTemp] 
FROM temps 
GROUP BY DATEFROMPARTS(YEAR([Date Time]),MONTH([Date Time]),'01')
ORDER BY DATEFROMPARTS(YEAR([Date Time]),MONTH([Date Time]),'01')
</pre>
![](https://storage.googleapis.com/wordpress-sqlsteve/2019/03/682bf98b-image.png)

In Python we follow virtually the same procedure, although in Python we have got this far without declaring that the &#8216;Date Time&#8217; field is the DateTime datatype where in SQL we have to do this before we start. We then add a new column called &#8216;Month&#8217; to the dataframe which is the first day of each month to aggregate the monthly values. Finally we use &#8216;groupby&#8217; to apply the &#8216;min&#8217; and the &#8216;max&#8217; to this dataframe.

<pre class="brush: python; title: ; notranslate" title="">temps["Date Time"] = pd.to_datetime(temps["Date Time"])
temps['Month'] = temps["Date Time"].apply( lambda temps : pd.datetime(year=temps.year, month=temps.month, day=1))	
temps.set_index(temps["Month"],inplace=True)
temps.groupby('Month').agg({'Temperature':['min','max']})
</pre>
![](https://storage.googleapis.com/wordpress-sqlsteve/2019/03/54918d24-image.png)


In R, we again do a very similar process. We use the excellent &#8216;zoo&#8217; library to turn a datetime into a date set on the first day of each month as we did previously. We then use the built-in aggregate feature to group summarise the &#8216;Temperature&#8217; column by the new month field we just created; applying the &#8216;min&#8217; and &#8216;max&#8217; functions on this field during these aggregations. This produces a dataframe which has too many columns, so we then select only the columns that we need using their numerical index (1,2 and 4) then re-alias them as &#8216;month&#8217;, &#8216;min&#8217; and &#8216;max&#8217;.

<pre class="brush: r; title: ; notranslate" title="">library(zoo) df1$month &lt;- as.Date(as.yearmon(df1$Date.Time)) 
SummarisedTemps &lt;- data.frame(aggregate(Temperature ~ df1$month, data=df1, FUN=min),aggregate(Temperature ~ df1$month, data=df1, FUN=max)) 
SummarisedTemps &lt;- SummarisedTemps[,c(1,2,4)] 
colnames(SummarisedTemps) &lt;- c("month", "min", "max") 
</pre>
![](https://storage.googleapis.com/wordpress-sqlsteve/2019/03/dd368628-image.png)


### 4. Visualising Data

Until recently, it would have been relatively difficult to continue the comparison at this point as SQL (At least in the Microsoft ecosystem) has never typically had a built in visualising tool, even for simple data profiling. Recently however, Microsoft have released [Azure Data Studio][9] for Linux, OSX and Windows based on their popular VS Code platform.

It was relatively easy to click a button on the right hand side of the screen and select a bar graph for my data set using the very same query as I used above. This graph was created without any code, but it was a bit fiddly with a few clicks to set up, the result is below.

![](https://storage.googleapis.com/wordpress-sqlsteve/2019/03/ea101cf9-image-1024x326.png)[10]

Alternatively if you don&#8217;t have Azure Data Studio you could use the venerable Microsoft Excel by simply copying the data out of the query results and using Excel&#8217;s chart feature. In this case I decided to use Excel as I wasn&#8217;t able to get Azure Data Studio to make a line plot in the way that I wanted it to.

![](https://storage.googleapis.com/wordpress-sqlsteve/2019/03/42736596-image-1024x662.png)[11]



With Python we do need some code and the help of the MatPlotLib library. With that imported, we set the size of the plot to 40x20inches. We then run the same aggregatation we created earlier and I just tag .plot().show() on the end. MatPlotLib helpfully (or perhaps unhelpfully?) does the rest for you, including working out what kind of data to show, labelling the axes and deciding what each axis scale should be. See below for code and results:

<pre class="brush: python; title: ; notranslate" title="">import matplotlib.pyplot as plt
plt.rcParams['figure.figsize'] = [40, 20]
temps.groupby('Month').agg({'Temperature':['min','max']}).plot().show()
</pre><figure class="wp-block-image is-resized">
[<img src="https://storage.googleapis.com/wordpress-sqlsteve/2019/03/c355ccc9-temps-1024x513.png" alt="Plot a line chart with two series in Python." class="wp-image-126" width="1024" height="513" srcset="https://storage.googleapis.com/wordpress-sqlsteve/2019/03/c355ccc9-temps-1024x513.png 1024w, https://storage.googleapis.com/wordpress-sqlsteve/2019/03/c355ccc9-temps-300x150.png 300w, https://storage.googleapis.com/wordpress-sqlsteve/2019/03/c355ccc9-temps-768x385.png 768w, https://storage.googleapis.com/wordpress-sqlsteve/2019/03/c355ccc9-temps-1568x785.png 1568w" sizes="(max-width: 1024px) 100vw, 1024px" />][12]</figure> 

In R we have to take slightly more control of the plotting process, we set the size of the plot to a larger size like we did in Python, we then plot the min line and assign the labels for the plot (xlab,ylab for each axis and main for the title). To add an additional line to the plot we use lines() for each additional line. The type property informs plot() that we would like a line plot.

<pre class="brush: r; title: ; notranslate" title="">options(repr.plot.width=16, repr.plot.height=9)
plot(SummarisedTemps$month,SummarisedTemps$min, type = "l", col = "red", xlab = "Month", ylab = "Temperature",main = "Temperature chart", ylim=c(-7,35) )
lines(SummarisedTemps$month,SummarisedTemps$max, type = "l", col = "blue")
</pre><figure class="wp-block-image is-resized">
[<img src="https://storage.googleapis.com/wordpress-sqlsteve/2019/03/3f0556d2-tempsr-1024x576.png" alt="Plot a line chart with two series in Python." class="wp-image-129" width="1024" height="576" srcset="https://storage.googleapis.com/wordpress-sqlsteve/2019/03/3f0556d2-tempsr-1024x576.png 1024w, https://storage.googleapis.com/wordpress-sqlsteve/2019/03/3f0556d2-tempsr-300x169.png 300w, https://storage.googleapis.com/wordpress-sqlsteve/2019/03/3f0556d2-tempsr-768x432.png 768w, https://storage.googleapis.com/wordpress-sqlsteve/2019/03/3f0556d2-tempsr.png 1536w" sizes="(max-width: 1024px) 100vw, 1024px" />][13]</figure> 

### To SUM() up

Well that&#8217;s it for now! What I hope to have demonstrated is that all three of these languages in the data space are more than capable of achieving exactly the same results with actually less syntactic difference between them than you might expect. 

I hope to have also demonstrated that while these languages might seem complex from the outside, the recurring problems that data scientists are spending a lot of their time on day-to-day are actually totally common sense kind of things that anyone would be able to spot. 

All of these languages in my opinion have a relatively shallow learning curve to start with which I think makes them all pretty accessible and an ideal learning ground for those who would like to get into data science.

If you are interested in following along with the blog inside your own Jupyter notebook you can get free compute at [Azure Notebooks][14] and you will find all of the code used in this blog at my GitHub page [here][15].

[1]: https://en.wikipedia.org/wiki/Data_wrangling
[2]: https://en.wikipedia.org/wiki/Declarative_programming
[3]: https://en.wikipedia.org/wiki/Python_(programming_language)
[4]: https://en.wikipedia.org/wiki/R_(programming_language)
[5]: https://en.wikipedia.org/wiki/S_(programming_language)
[6]: https://en.wikipedia.org/wiki/Guido_van_Rossum
[7]: https://opendata.bristol.gov.uk
[8]: http://www.r-tutor.com/r-introduction/data-frame
[9]: https://docs.microsoft.com/en-us/sql/azure-data-studio/what-is?view=sql-server-2017
[10]: https://storage.googleapis.com/wordpress-sqlsteve/2019/03/ea101cf9-image.png
[11]: https://storage.googleapis.com/wordpress-sqlsteve/2019/03/42736596-image.png
[12]: https://storage.googleapis.com/wordpress-sqlsteve/2019/03/c355ccc9-temps.png
[13]: https://storage.googleapis.com/wordpress-sqlsteve/2019/03/3f0556d2-tempsr.png
[14]: https://notebooks.azure.com/
[15]: https://github.com/stev2k/SQLSteve