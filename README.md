### Install python libraries
We have 8 dependencies below:

    bs4 >= 0.0.1
    matplotlib >= 2.0.1
    numpy >= 1.13.3
    scipy >= 0.19.1
    python-dateutil >= 2.6.1
    pytz >= 2017.3
    GitPython >= 2.1.5
    XlsxWriter >= 0.9.8

You can install using following commnad.
> $ pip install numpy scipy matplotlib pytz GitPython bs4 xlswriter python-dateutil<br />

### Update PATH information.
    - In the file scripts/commons/Subject.py, there are variables that stores a resource PATH information as a string.
    - The variables are Subjects.root, Subjects.root_result, and Subjects.root_feature.
    - You should change the variables according to cloned path of this repository.
