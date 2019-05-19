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

The ability to add custom tags and identify different elements being monitored in a way that is meaningful to you and your team / organisation - opens up a whole world of additional options over more traditional monitoring tools.

It means you are the master of how your infrastructure is identified - rather than being dictated to from a small set of auto-assigned names.   This in turn means that when there is a problem - the returned information and alerts about it - can be sorted, filtered and grouped in ways that demonstrate much more context for those trying to understand the situation.

![Host map details](https://i.imgur.com/ne1ISxO.png) 

Next the exercise requests an installation of a database - in my case I chose MySql - and the corresponding DataDog integration.

![mySql installation](https://i.imgur.com/CXuV7hq.png) 

Once again - DataDog makes the installation of the MySQL integration very straightforward - providing detailed instructions and specific commands to be run in order to setup each potential option and metric.

![mySql installation](https://i.imgur.com/xEfMdBn.png) 

After creating a new database (datadogdb) and a new user to be given access to it (datadog) - one needs to set the required permissions on the main db - as well as on the performance_schema - in order to extract metrics from it and pull them into DataDog.

![mySql permissions](https://i.imgur.com/lhKVDBj.png)

Once completed - these steps enable the included out of the box MySQL reports and dashboards within DataDog.  The huge advantage with these views and reports being included by DataDog - is the amount of time saved by not having to create your own.   Anyone who has run some of the other traditional monitoring tools will know the amount of time that used to need investing in even the basic views.  Thankfully the below required no manual time to produce or configure.

![mySql overview](https://i.imgur.com/B6TQsms.png)

#### Create a custom Agent check that submits a metric named my_metric with a random value between 0 and 1000

Despite the extensive number of out of the box integrations - there may well be metrics that need to be collected from unique systems and require a custom check to be built.  This is catered for via Custom Agent Checks - https://docs.datadoghq.com/developers/write_agent_check/ - via the creation of a check file and a configuration file - which the DataDog agent can use together to gather the required data from the system.

I created a check within /etc/datadog-agent/checks.d/ - called mycheck.py

```
from checks import AgentCheck
from random import randint

class my_metricCheck(AgentCheck):
    def check(self, instance):
        self.gauge('my_metric', randint(0, 1000))
```

and a corresponding configuration file in /etc/datadog-agent/conf.d/ - called (a matching) mycheck.yaml

```
init_config:

instances:
    [{}]
```

Which together - after restarting the datadog-agent service - began collecting a metric named my_metric with a random integer value - as shown below in the datadog metric explorer:

![my_metric check](https://i.imgur.com/GoSPhYL.png)

#### Change your check's collection interval so that it only submits the metric once every 45 seconds.

With the configuration file empty - the check just created - is using the default min_collection_interval of 15 seconds.

By altering the mycheck.yaml file and adding the following - we can change the collection interval to 45 seconds:

```
init_config:

instances:
    [{
        min_collection_interval: 45
     }]
```


#### Bonus Question Can you change the collection interval without modifying the Python check file you created?

It is possible to alter the value for min_collection_interval within the configuration file of any check, whether created by DD or by yourself as a custom check - from within the *.yaml* as we did for the mycheck.yaml file above.  The benefit of always keeping configuration like this within a configuration file - rather than in the check file - is that when time comes to verify or change configuration values - they are all in one place rather than scattered amongst the check code itself.

Visualising Data:
-------------------------------------


