# Getting Started
This section describes all procedures of use this benchmarks. The procedures include setting experiment environment, creating bug repository and checking out source codes of specific versions. The step of creating bug repository can be skipped when you use archives that you downloaded from the above table.
All the commands are written base on Ubuntu 16.04 LTS because all the experiments are executed in this environment.
We describe scripts folder briefly and list up each steps of all procedures.

```
## Scripts Directory Structure ##
- repository: Scripts to prepare the resources to execute each technique.
- results: Scripts to collect the execution results of each technique and export to Excel.
- analysis: Scripts to analysis for the result of each technique and features extracted from resources. <br /> 
             We applied Mann-Whitney U test, Pearson correlation and so on.
- commons: Scripts to managing subjects and common functions.
- utils: Personal libraries for experiments.
```

### Clone this repository
* Clone the repository by using the following command. (We cloned into the "Bench" directory.)
> $ git clone https://github.com/exatoa/Bench4BL.git Bench <br />
* If you don't have git, please install git first using following commands.
> $ sudo apt-get update <br />
> $ sudo apt-get install git <br />

    
### Download subjects' archives.
Download all subjects from the Subjects table and save them in the cloned repository path. If you need some of all subjects, you can download some of them.
We saved them into the 'Bench/_archives' directory. To use our scripts, we recommend that each subject stores in the group directory to which it belongs.
After downloaded, unpack all archives by using the unpacking.sh script.
> $ cd Bench <br />
> Bench$ mkdir _archives <br />
> Bench$ cd _archives <br />
> Bench/_archives$ mkdir Apache <br /> 
> Bench/_archives$ cd Apache <br />
> Bench/_archives/Apache$ wget -O CAMEL.tar "https://drive.google.com/uc?export=download&id=0B78iVP5pcTfKdEZZZnJrWmZxWjg" <br />
> ....work recursively.... <br />
> Bench$ mkdir data <br />
> Bench$ ./unpacking.sh _archives data

The last command unpacks all archive files in '_archives' folder into 'data' folder as keeping the original directory structures.


### Install python
* We used python 2.7. (If you have python 2.7 in your computer, please skip this section.)
> // install python <br />
> $ sudo add-apt-repository ppa:fkrull/deadsnakes <br />
> $ sudo apt-get update <br />
> $ sudo apt-get install python2.7 python <br />
> $ sudo apt-get install python-pip <br />

### Install python libraries
> // install python dependencies <br />
> $ pip install numpy scipy matplotlib pytz GitPython bs4 xlswriter nltk <br />

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
    
> Bench/scripts$ python launcher_repoMaker.py <br />
> Bench/scripts$ python launcher_DupRepo.py <br />
    
### Update count information of bug and source codes.
    - The script of Counting.py makes a count information for bug and source code. 
> Bench/scripts$ python Counting.py <br />
    

# Execute Previous Techniques
* To get the result of each technique, you can use scripts/launcher_Tool.py.
* Preparing step
    - You need to set the PATHs and JavaOptions in the launcher_Tool.py file.
    - Open the file, launcher_Tool.py and check the following variables 
    - ProgramPATH: Set the directory path which contains the release files of the IRBL techniques. (ex. u'~/Bench/techniques/releases/')
    - OutputPATH: Set the result path to save output of each technique (ex. u'~/Bench/expresults/')
    - JavaOptions: Set the java command options. (ex. '-Xms512m -Xmx4000m')
    - JavaOptions_Locus: Set the java options for Locus. Because Locus need a large memory, we separated the option. (ex. '-Xms512m -Xmx4000m')
* The script executes 6 techniques for all subjects.
* The script basically works for the multiple versions of bug repository and each of the related source codes.
* Options
    - -w <work name>: \[necessary\] With this option, users can set the ID for each experiment, and each ID is also used as a directory name to store the execution results of each Technique. Additionally, if the name starts with "Old", this script works for the previous data, otherwise works for the new data.
    - -g <group name>: A specific group. With this option, the script works for the subjects in the specified group. 
    - -p <subject name>: A specific subject. To use this option, you should specify the group name. 
    - -t <technique name>: A specific technique. With this option, the script makes results of specified technique.
    - -v <version name>: A specific version. With this option, the script works for the specified version of source code.
    - -s: Single version mode, With this option, the script works for the only latest source code.
    - -m: With this option, the bug repositories created by combining the text of duplicate bug report pairs are used instead of the normal one.


* Examples
> Bench/scripts$ python launcher_Tool.py -w NewData <br />
> Bench/scripts$ python launcher_Tool.py -w NewDataSingle -s <br />
> Bench/scripts$ python launcher_Tool.py -w NewData_Locus -t Locus <br />
> Bench/scripts$ python launcher_Tool.py -w NewData_CAMLE -g Apache -p CAMEL <br />

### Install Java
* All previous techniques are executed in Java Runtime Environment.
* If you have java in your computer, please skip this section.
> // install java <br />
> $ sudo apt-get install python-software-properties <br />
> $ sudo add-apt-repository ppa:webupd8team/java <br />
> $ sudo apt-get update <br />
> $ sudo apt-get install oracle-java8-installer <br />
>  <br />

### Install indri
- To execute BLUiR and AmaLgam, you need to install indri.
- Since there are compile problems, we chose indri-5.6 version.
- In the installing process, please memorize the path in the first line in the "make install" log. <br />
(In my case, /usr/local/bin.  This is the installed path of indri)
- And then, Change Settings.txt file.
- Commands to install indri
> // Install g++ and make for indri <br />
> $ sudo add-apt-repository ppa:ubuntu-toolchain-r/test <br />
> $ sudo apt-get update <br />
> $ sudo apt-get install g++ <br />
> $ sudo apt-get install make <br />
> $ sudo apt-get install --reinstall zlibc zlib1g zlib1g-dev <br />
> <br />
> // download and install indri (If you faced an error in the compiling, please try with another version.)<br />
> $ wget https://downloads.sourceforge.net/project/lemur/lemur/indri-5.6/indri-5.6.tar.gz <br />
> $ tar -xzf indri-5.6.tar.gz <br />
> $ cd indri-5.6 <br />
> $ ./configure <br />
> $ make <br />
> $ make install <br />
>    /usr/bin/install -c -m 755 -d /usr/local/bin <br />
>    /usr/bin/install -c -m 755 -d /usr/local/include <br />
>    /usr/bin/install -c -m 755 -d /usr/local/include/indri <br />
>    ... <br />
>    ... <br />
>    /usr/bin/install -c -m 644 Makefile.app /usr/local/share/indri <br />
>  <br />
> // changeSettings.txt file <br />
> $ cd ~/irblsensitivity/techniques/releases &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;// We assume you cloned our repository to  <br />
> $ vi Settings.txt <br />
> &nbsp; &nbsp; indripath=/usr/local/bin/ &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<-- edit this value as a the first log of "make install" <br />
>

# Previous Techniques Load on Eclipse
We changed previous techniques on Eclipse. But we didn't include eclipse environment files (.metadata folder, .project and .classpath file) in each previous techniques folders.
 
 So, If you want to load these techniques on Eclipse, please follow next sequence.
 
 - Open Eclipse
 - Make a 'techniques' folder into workplace of Eclipse. Then .metadata folder will be created in 'techniques' folder.
 - On the 'Package Explorer' panel, Open context menu by clicking right mouse button.
 - Select 'Import', Then a pop-up windows will be placed.
 - Except BLUiR project,  choose 'General > Projects from Folder or Archive' item and click 'Next' button.
 - Designate project folder in 'techniques' and click 'Finish' button.
 - Then, the project will be loaded and be shown in the Package Explorer.
 - BLUiR is made as Maven project. So, You should import with 'Maven > Existing Maven Project'. And then, just choose project folder. You don't need to change any other options.
 - Especially BLIA project, need to add library JUnit.


