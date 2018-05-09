
You can install using following commnad.
> $ pip install numpy scipy matplotlib pytz GitPython bs4 xlswriter python-dateutil<br />

### Update PATH information.
    - In the file scripts/commons/Subject.py, there are variables that stores a resource PATH information as a string.
    - The variables are Subjects.root, Subjects.root_result, and Subjects.root_feature.
    - You should change the variables according to cloned path of this repository.

### Inflate the source codes.
    - We used multiple versions of source code for the experiment. 
    - The script, launcher_GitInflator.py clones a git repositories and inflates it into the multiple versions which are used in the experiment.
    - Since the provided archives have only a git repository, you need to inflate also.
    - The version information that needs to inflate exists in the Python script and provided archives.
    - The information for the inflation are in the provided scripts and archives. See a file versions.txt in any subject's data directory.
> Bench$ cd scripts <br />
> Bench/scripts$ python launcher_GitInflator.py <br />
    
### Build bug repositories
    - We need to build a repository for the bug reports with pre-crawled bug reports.
    - We are already providing the result of this works in provided subject's archives.
    
