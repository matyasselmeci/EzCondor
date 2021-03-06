# EzCondor
## Intro
- convenient utilities
- workflow verification
## Content
### 1. Modules
#### ExtractMacros.py
 a class for extracting macros and their values in a submit file  

```python
class ExtractMacros:
	def __init__(self,filename):
        	self.filename = filename
	def handle_process(self,line, queue): 
	#a fuction to handle the queue of input or out put  = in.$(Process)
	def extract_rm(self,line,regulation,queue):
	# match target regulation, add all its value in a list and return	
	def extract(self, *args):
       '''
       main function for extracting the content of target macros
       1. accept more than one macros
       2. the result is a dict which its keys are macros and values are the content of macros.
        :return: a dict include target macros
        '''
```
#### ModifyNode.py :  
class ModifyNode： a class for modifying single node in dag
```python
#This class is used for changing a node's status by modifying its description file.
class ModifyNode:
    def __init__(self,nodename):
        self.filename = nodename

    def detect_status(self,macro,status):
        # check whether a macro's value matches the giving status.
    def change_to_noop(self):
        # change a Dag node to noop
    def reverse_noop(self):
        # reverse a noop nodes to  a Dag node to normal status.
```
#### Utilities.py :  
class Utilities： a class for warp basic submit and monitor functions of HTCondor, and add more available methods like submit job/DAG via classAD.
```python
class Utilities:
 
    def __init__(self,subfile = None):
        self.subfile = subfile
        self.subad = None
        if subfile != None:
            self.jobname = subfile.split(".")[0]
    def submit_subfile(self,subfile = None):
        '''
        submit a job via *.sub file
        :param 
        :return: stdout
        '''
    def submit_ondemand(self,jobname, **kwargs):
        '''
        This function will form a ClassAd and Submit job vid input argument, you must provide necessary argument for ClassAd
        :param
        :return: None
        '''

    def submit_file2AD(self,subfile = None):
	#convert a submit file to ClassAd form and then submit
    def submit_DAG2AD(self, dagfile):
	#convert a DAG submit file to ClassAd form and then submit
    def monitor_job(self):
       #monitor job finishing time
```

### 2. Tools
#### dag2dataflow.py: 
A tool which reads a DAG description file(*.dag) and outputs a dataflow result.

```python
# dag file example:
# File name: diamond.dag
#    JOB  A  test.py
#    JOB  B  test.py
#    PARENT A CHILD B C
#    PARENT B C CHILD D
# a dataflow model:
#   [{input: ..., transform: name.  output: ....}...]

robin@RobinLaptop:~/LocalProject/HTCondorPython$ ./step0418.py t.py
test.py has no marco: input
test.py has no marco: input
[{'Input': ['8788.abc', 'uim.data', 'mathematica.sh'], 'T': 'A', 'Output': ['t.py', 'test.py']}, {'Input': ['8788.abc', 'uim.data', 'mathematica.sh'], 'T': 'B', 'Output': ['t.py', 'test.py']}]
```

#### dag_makefile.py: 
a common script used as a PRE script before every node in DAG, which can make sure all nodes in a workflow can be executed only if its input and output meet some Rules. 

-  Rule:
```python
if a OP node:
    if all input_sandbox files exist:
        if all output_sandbox files exist:
            if the all output files are created lately than all input files:
                turn into NOOP
            else:
                no change # new input
        else:
            no change  # lack output 
    else：
        turn into NOOP
if a NOOP node:
    if all input—sandbox files exist:
        if all output files exist:
            if all output files created lately than all input files:
                no change  #no new input
            else:
                turn into OP
        else:
            turn into OP
    else:
        no change   # lack input
```
