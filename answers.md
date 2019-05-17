Introduction
------------
As part of the hiring process for a role with DataDog - as a Technical Account Manager - I was asked to complete this Solutions Engineer technical exercise.
Hopefully what follows will give you a quick glimpse into my approach to technical problems - as well as my presentation and documentation style.


Prerequisites - Setup the environment
-------------------------------------

The technical exercise instructions recommend starting with a blank canvas - and using a fresh host and OS - which is a sensible approach and provides a level playing field for comparison between candidate submissions.

I chose to fire up a new Vagrant VM on my development Macbook Pro - and use the bento box image to start an Ubuntu 16.04 OS - via the command:    

```
vagrant init bento/ubuntu-16.04

```

![Vagrant initiation](https://i.imgur.com/ORNli1f.png) 


The next step is to sign up for a new DataDog account - https://www.datadoghq.com/ - specifying the "DataDog Recruiting Candidate" organisation - so I could start collecting the metrics needed for this exercise.

Thankfully the agent installation is super simple - and when opening a new account - the first screens presented are those to guide you through the process for your specific situation and even provide the exact commands you need to run including the required API Key.

![Agent install screens](https://i.imgur.com/7IwQxAa.png) 

Collecting Metrics
-------------------------------------
 
Here we have the Host Map screen within my new DataDog account - showing the details of my vagrant box - as well as some custom tags set within the datadog.yaml file - using the syntax:

```
tags:
    - env:macbook_pro_15
    - role:mysql_database
```


![Host map details](https://i.imgur.com/ne1ISxO.png) 

Next the exercise requests an installation of a database - in my case I chose MySql - and the corresponding DataDog integration.

![mySql installation](https://i.imgur.com/CXuV7hq.png) 

Here once again - the DataDog site makes the installation of the MySQL integration very straightforward - providing detailed instructions and specific commands to be run in order to setup each potential option and metric.

![mySql installation](https://i.imgur.com/xEfMdBn.png) 



