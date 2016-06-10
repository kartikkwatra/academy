---
title: "Quick SQL Tricks"
redirect_from: 
permalink: /courses/sql-postgis/
---
# Quick SQL Tricks

CartoDB is built on a database called PostgreSQL. The SQL part of that means structured query language, which is a powerful and popular language for analyzing tables of data. This guide provides several SQl queries you can easily use as part of your data analysis workflow. If you are new to SQL, first review lesson one of this series, "SQL and PostGIS in CartoDB Editor".

## Basic Math Queries

A selection of queries to help you with a few basic mathematical operations on your dataset!

### Count Data

Running a count of your data can be helpful to determine if your dataset contains null data, or to make sure your dataset has the correct amount of data.

Example One
{% highlight sql %}
SELECT COUNT(*) 
FROM 
  tablename
{% endhighlight %}

Example Two
{% highlight sql %}
SELECT COUNT(*) 
FROM 
  tablename 
WHERE 
  columnname is null
{% endhighlight %}

### Find the Maximum Value of All Columns

Locate the largest value of your dataset.

{% highlight sql %}
SELECT 
  cartodb_id, 
  the_geom, 
  count, 
  date, 
  latitude, 
  longitude, 
  newcolumn, 
  the_geom_webmercator 
FROM 
  table_name
WHERE 
  count=(SELECT MAX(count) FROM table_name)
{% endhighlight %}

### Normalize Data

Normalize your data, this can be helpful with Torque maps that expect data in the range of 0-255.

{% highlight sql %}
UPDATE 
  tablename 
SET 
  normedcolumn = datacolumn*255/360
{% endhighlight %}

### Round data

Showing numerical data as part of an infowindow? Round your number column for easy viewing.

{% highlight sql %}
SELECT 
  round(count::numeric, 2) 
FROM 
  tablename  
{% endhighlight %}

## Formatting Your Data

## Date and Time

Postgres accepts date and time in many [formats](https://www.postgresql.org/docs/9.4/static/datatype-datetime.html). Postgres also offers many methods for dealing with date and time types. 

### Select by date part

{% highlight sql %}
SELECT * FROM table
WHERE date_part('day', timestamp)=3 --> Selects every 3rd day
AND date_part('month' timestamp)=5 --> Selects May dates
-- 'year', 'hour', 'minute', 'second' also work
{% endhighlight %}

### Convert Year String to Timestamp

If you have a dataset with a year value and no additional time information, the year column can be converted to a postgres timestamp. However, when using a year date for a Torque map, your animation may lack fluidity.

{% highlight sql %}
UPDATE 
  tablename 
SET 
  yearColumn = to_timestamp(yr_compl,'YYYY')
{% endhighlight %}

## Updating Your Dataset

Postgres also offers many convenience methods for updating a dataset including appending data, deleting data, and more!

### Insert Into

If you need to combine datasets, add append rows from one table into another table using INSERT INTO.

{% highlight sql %}
INSERT INTO table2 (column, column, ...)
SELECT (column, column, ...) FROM table1
{% endhighlight %}

### Delete Data

Delete null data from a table, which can affect analysis and visualization.

{% highlight sql %}
DELETE 
FROM 
  table_name
WHERE 
  the_geom_webmercator is null
{% endhighlight %}

## Selecting Data

There are many ways to slice and dice your data in order to select a subset of data.
