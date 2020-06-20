

# How to install 

- common-lab 에 기초하여 조금씩 수정하면서 내가 만든 dev 버전으로 설치한 것이다. 
- 실제로 해 본 것이다.
- 20200619

## Make a clean conda environment.

 ```shell
 conda create -n nbgrader python==3.8.1 jupyterlab nodejs
 conda activate nbgrader 
 ```

## Install nbgrader by following the instructions at nbgrader.readthedocs.io, development install.

 https://nbgrader.readthedocs.io/en/stable/contributor_guide/installation_developer.html

 ```
 git clone --single-branch --branch lab-common https://github.com/LibreTexts/nbgrader-to-jupyterlab
 cd nbgrader-to-jupyterlab
 
 
 
 pip install -r dev-requirements.txt -e .
 ```

## Install all the labextensions by following the development instructions at jupyterlab.readthedocs.io

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



## Install lab-serverextensions.

 ```shell
 jupyter serverextension enable --user nbgrader.labextensions.assignment_list.assignment_list
 
 jupyter serverextension enable --user nbgrader.labextensions.course_list.course_list
 ```

 Remark: 

A) nbgrader.labextensions.assignment_list.assignment_list.__init__.py 의 line19에서 lab_app.web_app을 lap_app으로 고쳐야 assignment_list labextension이 제대로 작동한다. 

B) 다음이 enable 된다. 

​	(1) nbgrader.labextensions.assignment_list.assignment_list

​	(2) nbgrader.labextensions.course_list.course_list

C) validate_asignment lab-serextension은 아직 미완성인 것으로 판단되어 설치하지 않는다. 원래 것을 그대로 사용한다. 



## Install nb-serverextensions

```shell
jupyter serverextension enable --user nbgrader.server_extensions.formgrader
jupyter serverextension enable --user nbgrader.server_extensions.validate_assignment
```

다음 두 serverextension을 설치한다. 

​	(1) nbgrader.server_extensions.formgrader

​	(2) nbgrader.server_extensions.validate_assignment

Remark:

- create_assignment labextenion does not require a serverextension.

- For formgrader we use the old one. No new work.

- It seems that they have done some works on validate assignment but they don't work. So I let the old validate_assignment come into play. 

  ```shell
  jupyter serverextension enable --user nbgrader.labextensions.validate_assignment.validate_assignment
  ```



## Modify assignment_list/src/assignmentlist 

At the moment, the [validate] button in the assignment_list labextension does not work. We fix it as follows. 

The basic idea is to use the old validate_assignment nb-serverextension. 

- define requestAPI2
- modify requestAPI to requestAPI2; arguments should be changed appropriately.
- rebuild jupyter lab. 

## Enjoy!

 ```
 cd /somewhere/in/my/computer/dg
 jupyter lab --watch
 ```

## Remark

- 잘 된다.

- If you don't cd to dg, then formgrader does not work properly.  

- 한 파일을 고치면 그 파일을 저장한 후

  ```shell
  jupyter lab build
  ```

  그러고 나서 chrome을 refresh한다. 그러면 수정사항이 적용된다. 

  - 음, 내 실험 결과 validate_assignment serverextension from the labextension directory는 완전하지 않다. 거기에 정의된 validate 함수는 하는 일이 아무 것도 없다. 그래서 원래 들어 있던 validate_assignment serverextension from the serverextension directory가 제공하는 API를 이용하도록 path를 약간 조정하였다. 그래서 노트북이 열렸을 때 메뉴에 생성된 "Validate" 버튼이 작동되도록 하였다. 

