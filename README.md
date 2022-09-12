# Project2

# Building a CI/CD Pipeline

**Overview**

The aim of the project is to demonstrate how to build a GitHub repository from zero to hero, and to create a scaffolding that will assist you to perform 
Continuous Integration and Continuous Delivery.

Also, 
-	Creating a Github Actions with a Makefile and requirement.txt file
-	Application code to perform initial lint, test and install cycle
-	Integrating the project with Azure Pipeline to enable Continuous Delivery to Azure Pipeline

**Short demo added [here](https://uptro29158-my.sharepoint.com/:v:/g/personal/maria_miculescu_student_upt_ro/EU8oGQkB61FDnZGdcy8OAwsBe7xwOEKZhrYTFffr7hNuIg?e=3kzGWH).**

**Architectural Diagram**

![](/images/digram%20flow.png)

**Project plan**

To visualize the evolution of the project and be sure that all actions are done in time a [spreadsheet](https://uptro29158-my.sharepoint.com/:x:/g/personal/maria_miculescu_student_upt_ro/Eb_MAS5xkxZCsWX8-zZwyaEBbg9Dmb4btCEexF_CxqgbOw?e=Nd0z1W) with a quarterly and yearly plan was designed, 
for each activity an estimation of difficulty was added. 

To be more easy to catch from a single view, a simply [Trello](https://trello.com/b/D0sgyFv7/project2) board was also included. 

**CI: Set Up Azure Cloud Shell**

I created a Github Repo, a ssh-key (added in the GitHub), I upload the key in personal GitHub account, 
I cloned the project into Azure Cloud Shell, please screenshot below:

![](/images/project%20cloned%20into%20Azure%20Shell.png)

Creating the Makefile with the associated code:

        install:
	  pip install --upgrade pip &&\
		  pip install -r requirements.txt
	
        test:
	  python -m pytest -vv test_hello.py

        lint:
		pylint --disable=R,C hello.py
    
        all: install lint test

Creating requirements.txt
Creating this file is a convenient way to list what packages a project needs. 
Also, for Flask I mentioned exact version of the package.

        Flask==2.0.3
        pandas
        scikit-learn
        pylint
        pytest

Creating the Python Virtual Environment and source into it:

        python3 -m venv ~/.myrepo
        source ~/.myrepo/bin/activate

Creating – hello.py 
        def toyou(x):
                return "hi %s" % x


        def add(x):
                return x + 1


        def subtract(x):
                return x - 1

Creating test_hello_py

        from hello import toyou, add, subtract


        def setup_function(function):
                print("Running Setup: %s" % function.__name__)
                function.x = 10


        def teardown_function(function):
                print("Running Teardown: %s" % function.__name__)
                del function.x


        ### Run to see failed test
        #def test_hello_add():
        #    assert add(test_hello_add.x) == 12

        def test_hello_subtract():
            assert subtract(test_hello_subtract.x) == 9

        Run local test using make all

![](/images/run%20local%20test.png)

Run python app.py and ./make_prediction.sh

![](/images/run%20python%20app.py.png)

![](/images/run%20make_prediction.sh.png)

**CI:Configure GitHub Actions**

Enable Github Actions and replace yml code with below:

    name: Python application test with Github Actions

    on: [push]
                jobs:
    build:

    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v2
    - name: Set up Python 3.5
      uses: actions/setup-python@v1
      with:
        python-version: 3.5
    - name: Install dependencies
      run: |
        make install
    - name: Lint with pylint
      run: |
        make lint
    - name: Test with pytest
      run: |
        make test
        
I verified that remote test pass in GitHub Actions:

![](/images/passing%20GitHub%20Actions.png)

GitHub actions badge

![Python application test with Github Actions](https://github.com/MariaMurarescu/Project2/actions/workflows/main.yml/badge.svg)

  
**Continuous Delivery on Azure**

Setting up Azure Pipelines to deploy Flask starter code to Azure app Service  
Deploying to Azure App Services

![](/images/deploying%20to%20azure%20app%20service.png)

Checking if the app is up and running by opening the URL:
https://mariaproject.azurewebsites.net

Edit file make_predict_azure_app.sh and replace <yourname>  with your webapp name
Test the remote webapp
        
![](/images/test%20remote%20webapp.png)
  
Logs of your running webapp via Azure Cloud Shell
  
![](/images/log%20for%20the%20app.png)
        
 **Setting up CI/CD using Azure**
 
Go to Azure DepOps Organizations, create new project called Project2
Set up a new service connection via Azure Resource Manager and Pipeline
Select Organizations Settings > New Service Connection > Azure Resource Manager
Next, select Pipeline and create a new one
Create GithHub integreation
Configure python to Linux Webapp on Azure
Set up CD workflow
  
![](/images/build%26deploy.png)  

![](/images/deployment%20job.png) 
   
From now, every change made to your code will trigger the CI/CD pipeline and update your webapp.
        
Final step
From Azure CLI do Git pull and push that everything will be update.
	
How to improve the project in the future

For the application I would add more fetaures to offer a frendly interface.

        

















