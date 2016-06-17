---
layout: post
title: Scratch post
---

This post is used for mucking about with styling things.  So it will always be full of gibberish.

LISP

{% highlight lisp linenos %}
(format t "Hello, world~%")
{% endhighlight %}

Python

{% highlight python %}
print('Hello, world')
{% endhighlight %}

```python
#!/usr/bin/env python3

# Wastrel is a tool for tracking time consumed by projects/packages.
# It has the secondary purpose of teaching me Python 3.
# Dr Owain Kenway

# Where it is distributed, it is done so under a 4 clause,
# BSD-style license (see LICENSE.txt)

# This method creates a table in the SQLite DB for a project
def createproj(project, dbloc, debug):
   import sqlite3
   if debug:
      print("Creating project", project)

   db = sqlite3.connect(dbloc)
   sql = "CREATE TABLE " + project + "(START INT NOT NULL, END INT NOT NULL, REASON TEXT)"

   if debug:
      print(sql)

   db.execute(sql)
   db.close()

# This method creates a record for an event.  You call the method before
# start work and then enter a reason (optional) and press return at the 
# end.  The start and stop are recorded in the database.
def startevent(project, dbloc, debug):
   import sqlite3
   import time

   if debug:
      print("Starting event in project", project)
   start = int(time.time())
   why = input("Say what you've been doing and press return when you've finished: ")
   stop = int(time.time())
   db = sqlite3.connect(dbloc)

# Note this is not bobby tables proof.  Don't expose this tool to multiple people.
   sql = "INSERT INTO " + project + "(START, END, REASON) VALUES (" + str(start) + "," + str(stop) + ",'" + why +  "')"

   if debug:
      print(sql)
   db.execute(sql) 
   db.commit()
   db.close()

# This method prints a log of all the events in a given project.
def log(project, dbloc, debug):
   import sqlite3
   db = sqlite3.connect(dbloc)
   sql = "SELECT START, END, REASON FROM " + project

   if debug:
      print(sql)
   records = db.execute(sql)
   print("start stop why")
   for row in records:
      print(row[0], row[1],'"' + str(row[2]) + '"')
   db.close()

# This method calculates the total time of all events for a given project
# in hours, minutes and seconds.
def printtotal(project, dbloc, debug):
   import sqlite3
   import datetime
   db = sqlite3.connect(dbloc)
   sql = "SELECT START, END, REASON FROM " + project

   if debug:
      print(sql)
   records = db.execute(sql)
   total = 0
   for row in records:
      total = total + (int(row[1]) - int(row[0]))
   print(str(datetime.timedelta(seconds=total)), "hours:minutes:seconds used by project", project)
   db.close()

# Main function
if __name__ == '__main__':
   import argparse

# Work out DB log on this machine.
   from os.path import expanduser
   home = expanduser("~")
   dbloc = home + "/.wastrel.db"

# Set up an argument parser.
   parser = argparse.ArgumentParser(description='Manage time.')
   parser.add_argument('-n', metavar='project', type=str, help="Create a new project.")   
   parser.add_argument('-e', metavar='project', type=str, help="Start a new event in a particular project.")
   parser.add_argument('-l', metavar='project', type=str, help="Print out log for project.")
   parser.add_argument('-u', metavar='project', type=str, help="Print out total usage for project.")
   parser.add_argument('-d', action='store_true', help="Enable debug output.")

   args = parser.parse_args()

   if args.d:
      print(args)

   if (args.n != None):
      createproj(args.n, dbloc, args.d)
   elif (args.l != None):
      log(args.l, dbloc, args.d)
   elif (args.e != None):
      startevent(args.e, dbloc, args.d)
   elif (args.u != None):
      printtotal(args.u, dbloc, args.d)
   else:
print("Doing nothing, use -h to see arguments.")
```
