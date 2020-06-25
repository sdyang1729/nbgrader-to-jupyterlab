# How to install nbgrader-to-jupyterlab

The sole purpose of this repository is to share my experiences in **install**ing [nbgrader-to-jupyterlab](https://github.com/LibreTexts/nbgrader-to-jupyterlab)'s [lab-common](https://github.com/LibreTexts/nbgrader-to-jupyterlab/tree/lab-common) branch. The original README file for [common](https://github.com/LibreTexts/nbgrader-to-jupyterlab/tree/lab-common) branch is [README_original.md](./README_original.md).

Disclaimer: Please use this code at your own risk. 


## Step 1 
Create a clean conda environment.

```shell
conda create -n nbgrader python==3.8.1 jupyterlab nodejs
conda activate nbgrader 
```

## Step 2
Clone the codes and install it by following the instructions at nbgrader.readthedocs.io, [development install](https://nbgrader.readthedocs.io/en/stable/contributor_guide/installation_developer.html).


```
git clone --single-branch --branch lab-common https://github.com/sdyang1729/nbgrader-to-jupyterlab
cd nbgrader-to-jupyterlab
pip install -r dev-requirements.txt -e .
```

## Step 3
Install all the labextensions by following the instructions at jupyterlab.readthedocs.io for developing [JupyterLab extension](https://jupyterlab.readthedocs.io/en/stable/developer/extension_tutorial.html).

```shell 
cd ./nbgrader/labextensions/assignment_list
jlpm install 
jupyter labextension install . --no-build

cd ../course_list
jlpm install
jupyter labextension install . --no-build

cd ../create_assignment
jlpm install
jupyter labextension install . --no-build

cd ../validate_assignment
jlpm install
jupyter labextension install . --no-build

cd ../../..
```



## Step 4
Install lab-serverextensions.

```shell
jupyter serverextension enable --user nbgrader.labextensions.assignment_list.assignment_list

jupyter serverextension enable --user nbgrader.labextensions.course_list.course_list
```

Remark: 

- One has to change lab\_app.web\_app to lab\_app in the line 19 of the file nbgrader.labextensions.assignment_list.assignment_list.\_\_init\_\_.py. Otherwise, assignment_list labextension does not work as it is supposed to. 

- The following serverextensions are enabled. 
	- nbgrader.labextensions.assignment_list.assignment_list
	- nbgrader.labextensions.course_list.course_list

- validate_asignment lab-serextension seems incomplete. In this install, we use the original. 



## Step 5
Install nb-serverextensions

```shell
jupyter serverextension enable --user nbgrader.server_extensions.formgrader
jupyter serverextension enable --user nbgrader.server_extensions.validate_assignment
jupyter serverextension enable --user nbgrader.labextensions.validate_assignment.validate_assignment
```

Remark:

- I am installing the original formgrader and validate_assignment.  
- This step installs the following two serverextensions. 
	- nbgrader.server\_extensions.formgrader
	- nbgrader.server\_extensions.validate\_assignment

- I modified `assignment_list/src/assignmentlist` a little bit so that we use the original validate_assignment nb-serverextension. 

## Step 6

In the following, python/ is the root directory for a course where all the homeworks are stored.

```
cd /somewhere/in/my/computer/python
jupyter lab --watch
```

Enjoy!


## Remark

- This installation method worked at least for me.
- If formgrader pops up but does not list the homeworks properly, check if your course directoty is well made.  
- If you change a file, then save it and perform the following command to make it work.

  ```shell
  jupyter lab build
  ```
