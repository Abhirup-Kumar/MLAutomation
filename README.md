# MLAutomation
Project integrating Machine Learning with DevOps.

This is project comprising the integration of Machine Learning with DevOps. In Machine Learning, I used KNN (K Nearest Neighbor) algorithm to solve a clustering problem and then integrated it with DevOps tools to automate the process of testing and increasing the accuracy if possible.

# In Machine Learning:

KNN is machine learning algorithm which is used when we have clustering related problem to solve. Like creating groups and then categorizing the datapoints in these groups according to some parameters or some requirements from the client. 
KNN doesn’t use the classic method of training the model, using optimizers, running epochs and all. It creates a map of clusters formed by grouping the datapoints and have predicted column having values max to no. of clusters which indicates in which group does this entry belong to. If prediction is asked it will place the features in formed clusters to see to which group does these values belong to and will predict according to it.
KNN uses the approach of feature learning, in a way learning the placements of features in clusters so when new features are given in form of prediction, it could predict the group it should belong to.

# In DevOps

## Jenkins
Jenkins is a self-contained, open source automation server which can be used to automate all sorts of tasks related to building, testing, and delivering or deploying software. Jenkins can be installed through native system packages, Docker, or even run standalone by any machine with a Java Runtime Environment (JRE) installed.

## Git
Git is a free and open source distributed version control system designed to handle everything from small to very large projects with speed and efficiency. By far, the most widely used modern version control system in the world today is Git. Having a distributed architecture, Git is an example of a DVCS (hence Distributed Version Control System). Rather than have only one single place for the full version history of the software as is common in once-popular version control systems like CVS or Subversion (also known as SVN), in Git, every developer's working copy of the code is also a repository that can contain the full history of all changes.

## Docker
Docker is a tool designed to make it easier to create, deploy, and run applications by using containers. Containers allow a developer to package up an application with all of the parts it needs, such as libraries and other dependencies, and deploy it as one package. By doing so, thanks to the container, the developer can rest assured that the application will run on any other Linux machine regardless of any customized settings that machine might have that could differ from the machine used for writing and testing the code.


# Project Explanation

In Machine Learning section, the dataset used was according to age of people which category spend how much in shopping. Clusters were made to allot them in different groups. Developer picked a value 3 for number of clusters to see the code working and behaving as expected and giving the accuracy of around 80% during prediction against testing data.
We created various jobs to deal with infrastructure in step by step manner.

## Job1 (GitHub)
![Result](Screenshots/ScreenShots/CaptureFINAL9.PNG)

This job is fairly easy than the other one. This job requires setup of Jenkins and Git in your system. I created a hook in my local Git in post-commit file where once any file is committed by the developer it will be automatically get pushed to its GitHub repository. Then in its second part, I had my Jenkins configured using Poll SCM which kept looking to my repository for any changes made and once it detected any changes it will download them to its Jenkins workspace and then I instructed it to copy the repository downloaded to a particular location.

## Job2 ( ContainerUp ) 
This job gets complex compared to the previous one. In this take was to take run the DockerFile containing all the details to launch out own container image containing especially that program which programmer just sent and setting up the whole infrastructure just for that single program. Here first we are checking that no image is already running under the same name which might cause a conflict so not only for the image, but also for the container and the volume we first checked if any with same name is present, if yes then remove it and launch a new one. A new completely empty one which is made only to host the files of this new project given by developer. Name of the image is production:latest and container is mlops and the volume is mlvol.

## Job3 ( Build_IncAccuracy )
This is the last section of this pipeline and the most important one. Here the container formed in previous job is turned on and a script is injected inside it and then it is run. The result of training the model is examined. We see if the code is running successfully otherwise it would have given some error and must have failed. It then checks the accuracy of the obtained model. 
Well yes, the most part is achieved by a script running inside it because I am not using any docker plugin in Jenkins but doing it with what we already have and this we have a major problem which is container is another O.S. and Jenkins is running in another O.S. so I execute the commands in container the same way we usually do in the O.S. where we have Jenkins installed.
It was performed with the exec command in docker where we sent multiple commands separated with semi-colon without getting inside it.
At last the Notification system which includes email mechanism. I also implemented email system which sends email to developer in three scenarios. First, before starting this job which indicates that we have successfully setup the environment and now we are proceeding with evaluating the model. Second, in case of any failure because this would majorly signify that evaluation failed marking failure in running of new code formed after the improvement changes. Third, it is the final one which says that all job has been successfully tuned and now we have achieved more than 90 percent accuracy and is ready in the allotted container.
 
# DockerFile
Created DockerFile to launch our own image which whole environment suited for the program committed by developer. CentOS is the Operating System is the one I chose for the model training; you can also use ubuntu which has wide support in Machine Learning community. Then I installed python3 in which the program is coded along with it, the libraries required by any KNN program. Then a new directory which will specially contain our developer’s codes and made it a working directory that is the default location where we will when we launch this container. Then copied all scripts and necessary files required by the script. Finally launch python interpreter to execute the code and save the results in a file so we can inspect the results to see any scope or requirement of improvement in the code. At last it provides us with a bash shell to execute any commands inside the running container.

