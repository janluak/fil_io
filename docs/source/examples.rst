Examples
========

The examples are split in two parts: the selection of files and the standard interface for reading/writing files.

Selecting Files
###############

The file selection provides multiple ways for retrieving the desired files.
All selecting functions contain three possibility to match:

+ file_ending: matching 100% of the part after the last ``.``
+ pattern: standard pattern for finding fixed strings with wildcards (like ``SomeFixedName_*.csv`` with * representing all kinds of string)
+ regex: a standard regular_expression (`regex <https://www.tutorialspoint.com/python/python_reg_expressions.htm>`_) matching the filename


The easiest way to get the latest file matching containing the name ``DataSource1`` in the beginning and which is a ``.csv`` file:

.. code-block:: python

    from fil_io import select

    # Explicit way
    file_name = select.get_newest_file_from_directory(
                    directory="path/to/directory",
                    pattern="DataSource1*",
                    file_ending="csv"
                    )

    # Shortened way
    file_name = select.get_newest_file_from_directory(
                    directory="path/to/directory",
                    pattern="DataSource1*.csv"
                    )

File reading / writing
######################

The library provides a standardized way of interacting with files.
For every file-type in the `file_IO` subpackage, there exist load- & write-functions following the same pattern.
Only exception is the `xls` module due to the characteristics of sheets.

`Examples are given mostly with csv module, switch the` ``csv`` `to whatever submodule/-package you need.`

All-in-one/doing-all-the-magic loading functions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
The most easy way to load data is with the `load-type` function.
It is a shortcut for the specific ways of loading data in each file-type specific module:

.. code-block:: python

    from fil_io import csv

    data = csv.load(path="path/to/file.csv")
    # data is list of lists representing the csv file

    data = csv.load(path="directory/of/multiple/files")
    # data is dictionary representing all csv files with {file_name: file_content}


The most easy way to write data is with the `write-type` function.
It is again a shortcut to file-type specific modules:

.. code-block:: python

    # data is written to the csv file
    from fil_io import csv
    csv.write(file_name="path/to/file.csv", data=data_to_write)

    # data is written to the json file
    from fil_io import json
    json.write(file_name="path/to/file.json", data=data_to_write)


File-type specific modules: advanced reading/writing
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
For every file-type exist more specific functions for reading & writing the data.
The presented examples from above are redirecting to the most general functions in the packages.

If using a IDE, the implemented functions will be shown to you after importing the file-specific module directly with typing ``csv.`` and hitting `tab`.
If in interactive mode, simply run ``csv.__all__``.

Reading
^^^^^^^
The reading of the files is fairly simple

.. code-block:: python

    from fil_io import csv

    # load single csv file
    data = csv.load_single(file_name="path/to/file.csv")
    # data is representing the csv file


    # load specific list of csv files
    data = csv.load_these(file_name_list=["path/to/file1.csv", "path/to/file2.csv"])
    # data is representing both csv files; {file_name: file_content}


    # load all csv files from a directory
    data = csv.load_all(directory="/path/to/directory")
    # data is representing all csv files of this directory; {file_name: file_content}



    # doing all of the above depending if `path` is file, list_ofs or directory
    data = csv.load(path="path/to/any")
    # depending if single file or multiple files either dictionary representing json file or {file_name: json_value}


Writing
^^^^^^^
For writing, the `fil_io` package provides sometimes some more options for making life easier.
The concept this package is designed, is to work most likely with data in form of a dictionary.
Therefore, often shortcuts are provided.

Let's have a look to row-based file-type `csv` (`comma separated values`):
You can provide either row-based data (in python this would be a list of lists),
or you can provide a dictionary instead and let `fil_io` take care of the conversion. This little magic is part of the `fil_io.convert` module, more details below.

.. code-block:: python

    from fil_io import csv

    # lets start with row-based data
    example_rows = [
                    ["Header1", "Header2", "Header3"],
                    ["Value11", "Value12", "Value13"],
                    ["Value21", "Value22", "Value23"]
                   ]
    csv.write_from_rows(file_name="path/to/csv.csv", rows=example_rows)

    # The result in the file:
    # Header1,Header2,Header3
    # Value11,Value12,Value13
    # Value21,Value22,Value23


    # in difference with data in form of a dictionary
    example_dict = {
                     "Header1": {
                       "Value11": {
                         "Header2": "Value12",
                         "Header3": "Value13"
                       },
                       "Value21": {
                         "Header2": "Value22",
                         "Header3": "Value23"
                       }
                     }
                   }
    csv.write_from_dict(file_name="path/to/csv.csv", data=example_dict)

    # The result in the file is the same:
    # Header1,Header2,Header3
    # Value11,Value12,Value13
    # Value21,Value22,Value23

    # additionally the data can be provided without the naming of the main_key
    #  (in this case "Header1")
    example_dict2 = {
                     "Value11": {
                       "Header2": "Value12",
                       "Header3": "Value13"
                    },
                    "Value21": {
                       "Header2": "Value22",
                       "Header3": "Value23"
                     }
                   }

    csv.write_from_dict(
        file_name="path/to/csv.csv",
        data=example_dict,
        main_key_name="Header1",
        main_key_position=0
    )

    # The result in the file is still the same:
    # Header1,Header2,Header3
    # Value11,Value12,Value13
    # Value21,Value22,Value23

Again, there is a function combining both writing methods, available also with a shortcut stated
in the very beginning of the examples: ``csv.write``


xls/xlsx Files
^^^^^^^^^^^^^^

The Microsoft Excel file interaction works slightly different since sheets are a feature not available to
standard file formats like `json`, `csv` or `xml`.
The standard output format is `Pandas DataFrame <https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.html>`_.

Yet, interaction is still fairly simple:

.. code-block:: python


    from fil_io import xls

    data_frame = xls.load_single_sheet(file_name="path/to/file.xls")     # .xlsx works with the same function
    # returns a pandas.data_frame from first sheet

    # you can specify a sheet_name
    data_frame = xls.load_single_sheet(file_name="path/to/file.xls", sheet="Sheet_Name")
    # returns a pandas.data_frame from sheet with provided name


    # of course multiple sheets can be loaded
    data = xls.load_these_sheets(file_name="path/to/file.xls", sheets=["Sheet_Name1", "Sheet_Name2"])
    # just like the other loading functions, the sheet_name is the key in a dictionary containing the data_frame as value
    # {"Sheet_Name": DataFrame}

    # loading all sheets
    data = xls.load_all_sheets(file_name="path/to/file.xls")
    # {"Sheet_Name": DataFrame}


    # reading multiple files is possible as well
    data = xls.load_theses(file_name_list=["path/to/file1.xls", "path/to/file2.xls"])
    # {file_name: {sheet_name: DataFrame}}
