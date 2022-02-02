# Going from Azure Pipelines to Github Actions

Github Actions has the same basic structure as Azure Pipelines and shares a lot of other similarities with each other. Usually you find the differences when it comes to syntax and the so called _Actions_ which are equal to built-in tasks in Azure Pipelines. 

To get started with Github Actions you need a ``` .github/workflows ``` directory in your repository on Github and inside the folder _workflows_  you can add a ``` .yml ``` file which will be your Github Actions file. A shortcut to create it is to click on the _Actions_ bar in your Github repository and _set up a workflow yourself_ :

![Actions](Images/git_actions.png)

To decide when Github Actions will trigger a workflow you need to define it by using _on_ (same as _trigger_ on Azure Pipelines), for example here where it is triggered when making a push or pull request to the master branch:

```yml
on:
  push:
    branches: main
  pull_request:
    branches: main  
```

Inside _jobs_ (same as _stages_  on Pipelines) you define what you want Github Actions to do. First you name the job and then decide which type of machine to use by using _runs-on_. 

Inside _steps_ you define all the tasks that you want to be executed as parts of the job. For example here can we see a job called _build_ which runs on a linux machine. First it uses the action _actions/checkout@v2_ so your workflow can access the repository. Then with _actions/setup-dotnet@v1_ it sets up a.NET CLI environment. After that we are able to write commands by using _run_:

```yml
jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v2
    - name: Setup .NET
      uses: actions/setup-dotnet@v1
      with: |
        dotnet-version: 5.0.x
    - name: Restore dependencies
      run: dotnet restore
    - name: Build
      run: dotnet build --no restore
    - name: Test
      run: dotnet test --no-build --verbosity normal
```

Variables in Github Actions are defined by using _env_ (Environment variable). Github Actions also contains predefined variables the same way as Azure Pipelines does but with different names. When using variables the praxis is to use dollarsign with double curly brackets, for example if you want to print the variable _first_name_: 

```yml
run: echo ${{ env.first_name }}
env:
  first_name: John
```
