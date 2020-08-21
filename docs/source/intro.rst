************
Introduction
************

Making intro to data handling in python nice and easy!

The ``datesy`` package provides easy handling (read/write) of standard file types, the conversion between the file types
as well as basic data inspection functionalities.

It is designed for an easy start into the python world and data handling in it without having to think of unnecessary basics.


Main Usage
##########

Datesy, making DATa handling EaSY, is mostly helpful if you are looking for:

1. loading/dumping data to a standard file format like `json`, `csv`, `xml`, `xls(x)`
2. inspecting complex data like searching for a path in a dictionary
3. mapping strings and their properties


Motivation
##########

History
***********

The idea to this package came during the work as a consultant with a customer where lot's of files needed
to be read, transformed, inspected etc. and no adequate tools besides searching & filtering with Excel of files partly in the range of GBs were around.

With starting to share the python insights and the code fragments, the only logical next step was do create a really reusable code fragment - a python package.

From this `datesy <https://github.com/janluak/datesy>`_ and ``fil_io`` derived.

Future Development
*********************

With simplifying file IO, adding some more file types or


Limitations
###########

This package is designed to be used by anybody who is new to python. Therefore functions are explicitly held limited to their magic and described accordingly.
There are few things really the big shit rather than simply helping with small tasks which you could have written yourself in a few lines of code but didn't want to think about.
For deep data analysis other packages are far more powerful and maybe helpful to you. Think of ``datesy`` more of the little butler taking care of some basic tasks for you.

This package is compatible to `PyPy <https://pypy.org>`_'s version 3.6.
