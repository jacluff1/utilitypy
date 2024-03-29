# utilitypy
utilitypy is a collection of tools to allow printing to terminal, writing files, saving files, and loading files

install
=======
just clone the repo wherever you want it. python version >= 3.6 is required. if using [djakToolbox](https://github.com/jacluff1/djakToolbox) or using a source environment set using [envSet.sh](https://github.com/jacluff1/BASH/blob/master/envSet.sh), you should be fine. 

config.py
=======
a dictionary of default values to be used by modules

util.py
=======
defines two classes, basic and util

1. util
* loadme(self, filename, **kwargs)  
mandatory sring argument, 'filename', with logic inplace to load different kinds of files.  
  * '.csv' are loaded as panda DatFrames  
  * '.npy' are loaded as numpy arrays  
  * '.pkl' are read and loaded using pickl.load()  
  * '.txt' are read line-by-line into a list.  
kwargs are fed to the controller and into any methods that allow them. For instance, np.load(**kwargs) or pd.read_csv(**kwargs)  
example:  
```python
util.loadme('A.npy', skiprows=1)
util.loadme('B.txt')
util.loadme('DF.csv', index_col='a')
```

* printme(self, *args, **kwargs)  
takes any number of arguments and constructs a list of lines that can be either returned or printed to terminal. each object is read separately to allow pretty printing for multiple types of objects. all objects are recursively fed intil it can extract a single string or value to print. The only exception is a tuple.  
if a tuple is supplied as an argument, it assumes you are feeding logic.  
  * length 1: no header, no message, obj to print
  * length 2: no header, message to print the above the line where obj starts, obj to print
  * length 3: header bool, message/header to print above the line where obj starts, obj to print  
kwargs:  
  * tabs: the number of tabs to indent object at
  * output: if true, will return constructed list; otherwise prints to terminal  

example:
```python
util.printme(
    (True, "test header", "beginning test sequence"),
    ("pretty print dictionary:", {'a': [1,2,3], 'b': [5,6,7]}),
    ("test list:", [1,2,3,4]),
    tabs = 1
)
```
```txt
>>>>
    ===============================================================================
    test header
    ===============================================================================
    
    beginning test sequence
    pretty print dictionary:
        a:
            1,
            2,
            3
        b:
            5,
            6,
            7
    test list:
        1,
        2,
        3,
        4
```
* saveme(self, filename, obj, **kwargs)  
the corollary to loadme
* writeme(self, filename, *args, **kwargs)  
gets a list of lines to write using lines = util.printme(*args, output=True, **kwargs), then writes the lines to the filename.
2. basic: repackages a dictionary as a class

writeme.py
==========
functions to write basig functional python file template or class definition template. this module is also executable

1. write_template(*args):  
* *args are an arbitrary number of filenames. a file will be written for each filename provided. NOTE: do not include .py in filename  
2. write_template_class(*args):  
* *args are an arbitrary number of filenames. a class template will be written for each filename provided. NOTE: do not include .py in filename

examples:
```python
writeme.write_template('test1', 'test2')
writeme.write_template_class('class1', 'class2', 'class3')
```
```bash
python -B writeme.py -p "path/to/directory" -f "test1 test2" -c "class1 class2 class3"
```
in command line, run
```bash
python writeme.py -h
```
for more information on command-line options
