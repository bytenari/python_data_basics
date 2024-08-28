## Start with IDE

### Visual Studio Code
  pros: familiar, easy to handle with other languages, easy to update on Github
  
  cons: requires installing libraries such as panda, numpy 
### Jupyter Notebook
  pros: built-in libraries, specialised for python related to data
  
  cons: no support for other languages, not connected to Github

*I decided to use both because I don't know which IDE would be used in workplaces.*


## Command-lines
```
@bytenari:/home/project$ python3.11 --version

@bytenari:/home/project$ sudo apt install python3.11

@bytenari:/home/project$ python3.11 -m pip install numpy
```

**ExampleCode1.py**

```
import numpy as np

a = np.array([1, 2])
b = np.array([3, 4])
c = a + b
print(c)
```

**To get the result of ExampleCode1,**

@bytenari:/home/project$ python3.11 ExampleCode1.py
