# Steps to convert .py to .exe in Python 3.6

Install Python 3.6.

Install cx_Freeze, (open your command prompt and type pip install cx_Freeze.

Install idna, (open your command prompt and type pip install idna.

Write a .py program named myfirstprog.py.

Create a new python file named setup.py on the current directory of your script.

In the setup.py, code below and save it.

With shift pressed right click on the same directory, so you are able to open a command prompt window.

In the prompt, type python setup.py build

If your script is error free, then there will be no problem on creating application.

Check the newly created folder build. It has another folder in it. Within that folder you can find your application. Run it. Make yourself happy.

### setup.py:
```
from cx_Freeze import setup, Executable

base = None    

executables = [Executable("myfirstprog.py", base=base)]

packages = ["idna"]
options = {
    'build_exe': {    
        'packages':packages,
    },    
}

setup(
    name = "<any name>",
    options = options,
    version = "<any number>",
    description = '<any description>',
    executables = executables
)
```
### EDIT:

* be sure that instead of myfirstprog.py you should put your .pyextension file name as created in step 4;
* you should include each imported package in your .py into packages list (ex: packages = ["idna", "os","sys"])
* if your .py scriot imported using from then you should include the base backage into the pachage list. (e.g ```from multiprocessing import Queue```) you have to include ```multiprocessing```.
* any name, any number, any description in setup.py file should not remain the same, you should change it accordingly (ex:name = "<first_ever>", version = "0.11", description = '' )
* the imported packages must be installed before you start step 8.
* once the .exe gets created and if you get dll issue up on running the executable then, you have to copy the dll into the current directory where you are building the exe.

### Example:
```
import sys
import os
from cx_Freeze import setup, Executable

includes = []
# include_files = [r"C:\Users\saktikanta\AppData\Local\Programs\Python\Python36\DLLs\tcl86t.dll",
#                  r"C:\Users\saktikanta\AppData\Local\Programs\Python\Python36\DLLs\tk86t.dll"]

os.environ['TCL_LIBRARY'] = "C:\\Users\\saktikanta\\AppData\\Local\\Programs\\Python\\Python36\\tcl\\tcl8.6"
os.environ['TK_LIBRARY'] = "C:\\Users\\saktikanta\\AppData\\Local\\Programs\\Python\\Python36\\tcl\\tk8.6"
base = 'Win32GUI' if sys.platform == 'win32' else None

base = None

executables = [Executable("sampleApp1.py", base=base)]

packages = ["idna"]
options = {
    'build_exe': {
        'packages':packages,
        "include_files": ["tcl86t.dll", "tk86t.dll"]
    },
}

setup(
    name = "Demo Desktop App",
    options = options,
    version = "0.1",
    description = 'Initial version of desktop app using python',
    executables = executables
)
```
