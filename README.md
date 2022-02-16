## Domino Hands-On Workshop: Predicting Wine Quality

#### In this workshop you will work through an end-to-end workflow broken into various labs to -

* Read in data from a live source
* Prepare your data in an IDE of your choice, with an option to leverage distributed computing clusters
* Train several models in various frameworks
* Compare model performance across different frameworks and select best performing model
* Deploy model to a containerized endpoint and web-app frontend for consumption
* Leverage collaboration and documentation capabilities throughout to make all work reproducible and sharable!


## Section 1 - Project Set Up

### Lab 1.1 - Forking Existing Projects
Once you have access to the Domino training environment use the search bar to discover any projects tagged for 'Training'

Select the project called WineQuality

![image](readme_images/Search.png)

Read the readme to learn more about the project's use case, status, etc.

In the top right corner, choose the icon to **fork** the project. Name the project *yourname-Domino-Training*

![image](readme_images/Fork.png)

In your new project - go into the settings tab

View the default hardware tier and compute environment - ensure they are set to 'Small' and 'Workshop-Environment' respectively:

![image](readme_images/ProjectSettings.png)

Go the access and sharing tab - change your project visibility to **Public**

![image](readme_images/ProjectVisibility.png)

Add your instructor or another attendee as a collaborator in your project. 
![image](readme_images/AddCollaborator.png)

Change their permissions to results consumer.
![image](readme_images/ResultsConsumer.png)

### Lab 1.2 - Defining Project Goals

Click back into the Overview area of your project. Then navigate to the manage tab.

Click on add some goals

![image](readme_images/AddProjectGoals.png)

For the goal title type in 'Explore Data' and click save. Once the goal is saved click the drop down on the left to mark the goal as in 'Data Acquisition and Exploration' status.


![image](readme_images/Goal1status.png)

[optional] - Add a comment to the goal and tag a collaborator you've added earlier by typing @ then their username

![image](readme_images/Goal1comment.png)

### Lab 1.3 - Add Data Source

We will now add data connection defined by an admin to our project to later query in data. To do so - navigate to the Data tab of your projects

![image](readme_images/AddDataSource.png)

Select the 'domino-winequality-workshop' s3 bucket connection and click add to project

![image](readme_images/AddS3.png)


This concludes all labs in section 1 - Prepare Project and Data! 
