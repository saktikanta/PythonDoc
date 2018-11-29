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
# Convertion of .nmon file(Linux) into .csv file
```
import os
import logging as log
import datetime

tstamp = {}
sysInfoA = []
sysInfoB = []
detailsData = {}
currDir = os.path.dirname(os.path.realpath(__file__))
fname="C:\\saktikanta\\test1.nmon"
outdir= currDir + "\\data\\"
os.mkdir(outdir)
overwrite=False
debug=False

fo = open(fname, 'r')

for l in fo.readlines():
    l = l.strip()
    lar = l.split(',')
    # print(lar)
    if "ZZZZ" in lar[0]:
        # print("before update timestamp",tstamp)
        # print("before update line is",lar)
        tstamp[lar[1]] = lar[3]+" "+lar[2]
        # print(tstamp)
    elif "AAA" in lar[0]:
        sysInfoA.append(lar)
    elif "BBB" in lar[0]:
        sysInfoB.append(lar)
    elif "TOP" in lar[0] and len(lar) > 3:
                    # top lines are the only ones that do not have the timestamp
                    # as the second column, therefore we rearrange for parsing.
                    # kind of a hack, but so is the rest of this parsing
                pid = lar[1]
                lar[1] = lar[2]
                lar[2] = pid
    else:
        if lar[0] in detailsData.keys():
            getTable = detailsData[lar[0]]
            for n, col in enumerate(getTable):
                if n == 0 and lar[n + 1] in tstamp.keys():
                    col.append(tstamp[lar[n + 1]])
                elif n == 0 and lar[n + 1] not in tstamp.keys():
                    print(lar[n], lar[n + 1],",2nd column is not time a timestamp")
                    break
                else:
                    col.append(lar[n + 1])
        else:
            heads = []
            for h in lar[1:]:
                # make it an array
                tmp = []
                tmp.append(h)
                heads.append(tmp)
            detailsData[lar[0]] = heads
fo.close()
print(detailsData.keys())

for stat in detailsData.keys():
    outFile = open(os.path.join(outdir, stat + ".csv"), "w+")
    line = ""
    if len(detailsData[stat]) > 0:
        # Iterate over each row
        for n in range(len(detailsData[stat][0])):
            line = ""
            # Iterate over each column
            for col in detailsData[stat]:
                if line == "":
                    # expecting first column to be date times
                    if n == 0:
                        # skip headings
                        line += "DateTime datetime default current_timestamp,"
                    else:
                        tstamp = datetime.datetime.strptime(
                            col[n], "%d-%b-%Y %H:%M:%S")
                        line += tstamp.strftime("%Y-%m-%d %H:%M:%S")
                else:
                    line += "," + col[n]
            outFile.write(line + "\n")

```
