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
Once you have access to the Domino training environment - click the search button in the top left corner of the UI then use the search bar to discover any projects tagged for 'Training'

Select the project called WineQuality

<!-- ![image](readme_images/Search.png) -->

<p align="center">
<img src = readme_images/Search.png width="800">
</p>

Read the readme to learn more about the project's use case, status, etc.

In the top right corner, choose the icon to **fork** the project. Name the project *yourname-Domino-Training*

<!-- ![image](readme_images/Fork.png) -->

<p align="center">
<img src = readme_images/Fork.png width="800">
</p>

In your new project - go into the settings tab

View the default hardware tier and compute environment - ensure they are set to 'Small' and 'Workshop-Environment' respectively:

<!-- ![image](readme_images/ProjectSettings.png) -->

<p align="center">

<img src = readme_images/ProjectSettings.png width="800">
</p>

Go the access and sharing tab - change your project visibility to **Public**

<!-- ![image](readme_images/ProjectVisibility.png) -->

<p align="center">
<img src = readme_images/ProjectVisibility.png width="800">
</p>

Add your instructor or another attendee as a collaborator in your project. 
<!-- ![image](readme_images/AddCollaborator.png) -->

<p align="center">
<img src = readme_images/AddCollaborator.png width="800">
</p>

Change their permissions to results consumer.
<!-- ![image](readme_images/ResultsConsumer.png) -->

<p align="center">
<img src = readme_images/ResultsConsumer.png width="800">
</p>

### Lab 1.2 - Defining Project Goals

Click back into the Overview area of your project. Then navigate to the manage tab.

Click on Add Goals

<!-- ![image](readme_images/AddProjectGoals.png) -->

<p align="center">
<img src = readme_images/AddProjectGoals.png width="800">
</p>

For the goal title type in 'Explore Data' and click save. Once the goal is saved click the drop down on the left to mark the goal as in 'Data Acquisition and Exploration' status.


<!-- ![image](readme_images/Goal1status.png) -->

<p align="center">

<img src = readme_images/Goal1status.png width="800">
</p>

[optional] - Add a comment to the goal and tag a collaborator you've added earlier by typing @ then their username

<!-- ![image](readme_images/Goal1comment.png) -->

<p align="center">

<img src = readme_images/Goal1comment.png width="800">
</p>

### Lab 1.3 - Add Data Source

We will now add data connection defined by an admin to our project to later query in data. To do so - navigate to the Data tab of your projects

<!-- ![image](readme_images/AddDataSource.png) -->

<p align="center">

<img src = readme_images/AddDataSource.png width="800">
</p>

Select the 'domino-winequality-workshop' s3 bucket connection and click add to project

<!-- ![image](readme_images/AddS3.png) -->

<p align="center">
<img src = readme_images/AddS3.png width="800">
</p>


This concludes all labs in section 1 - Prepare Project and Data! 

## Section 2 - Develop Model

### Lab 2.1 - Inspect Compute Environment
Click on the cube icon on the far left sidebar of the UI

Select 'Domino-Workshop-Environment' 

<p align="center">
<img src = readme_images/EnvironmentsPage.png width="800">
</p>

Inspect the dockerfile to understand the python & R packages installed, configurations specified, kernels installed etc. 

Scroll down to Pluggable Workspaces Tools - this is the area in the compute environment where IDEs are made available for end users

Scroll down to the Run Setup Scripts section

Here we have a script that executes upon startup of workspace sessions or job (pre-run script) and a script that executes upon termination of a workspace session or job (post-run script) 

Click on the revisions tab - here you can see all the versions of an environemnt that have existed over time. You can see who built which version and, if you are permissed to do so, you can select 
which revesion becomes the active version

Finally navigate to the projects tab - you should see all projects that are leveraging this compute environment. Click on your project to navigate back to your project. 


Click into the Workspaces tab to prepare for the next lab.

### Lab 2.2 - Exploring Workspaces

In the top right corner click Create New Workspace

<p align="center">
<img src = readme_images/AddWorkspace.png width="800">
</p>

Click the Workspace Environemnt dropdown to browse all avaiable Compute Environments - ennsure that Domino-Workspace-Environment is selected.

Select JupyterLab as the Workspace IDE

Click the Hardware Tier dropdown to browse all available hardware configurations - ensure that Small is selected. 

Click Launch now.

<p align="center">
<img src = readme_images/LaunchWorkspace.png width="800">
</p>

Once the workspace is launched, create a new python notebook by clicking here:

<p align="center">
<img src = readme_images/NewNotebook.png width="800">
</p>

When you have your notebook loaded, click on the Data tab, then onto the data source we added in lab 1 as displayed below -

<p align="center">
<img src = readme_images/DataTab.png width="800">
</p>

Copy the provided code snippet into your notebook and run the cell

<p align="center">
<img src = readme_images/S3CodeSnippet.png width="800">
</p>

After thats run copy the code below into a following cell - 

```python
from io import StringIO
import pandas as pd

s=str(objects[0].get(),'utf-8')
data = StringIO(s) 

df=pd.read_csv(data)
df.head()
```

Now cell by cell, enter the following code and run the cells to visualize and prepare the data!

```python
import seaborn as sns
import matplotlib.pyplot as plt
df['is_red'] = df.type.apply(lambda x : int(x=='red'))
fig = plt.figure(figsize=(10,10))
sns.heatmap(df.corr(), annot = True, fmt='.1g')
```

```python
corr_values = df.corr().sort_values(by = 'quality')['quality'].drop('quality',axis=0)
important_feats=corr_values[abs(corr_values)>0.08]
print(important_feats)
sns.set_theme(style="darkgrid")
plt.figure(figsize=(16,5))
plt.title('Feature Importance for Wine Quality')
plt.ylabel('Pearson Correlation')
sns.barplot(important_feats.keys(), important_feats.values, palette='seismic_r')
```
```python
for i in list(important_feats.keys())+['quality']:
    plt.figure(figsize=(8,5))
    plt.title('Histogram of {}'.format(i))
    sns.histplot(df[i], kde=True)
```

Finallly write your data to a domino dataset by running

```python
df.to_csv('/domino/datasets/local/WineQuality/WineData.csv', index = False)
```

Rename your notebook 'EDA_code.ipynb' by right clicking on the file name as shown below then click the Save icon.

<p align="center">
<img src = readme_images/RenameAndSaveNotebook.png width="800">
</p>

### Lab 2.3 - Syncing Files

Now that we've finished working on our notebook and written data back to our project, we want to sync our latest work. To do so click on the File Changes tab in the top left corner of your screen - 

<p align="center">
<img src = readme_images/SyncProject.png width="800">
</p>

Enter an informative but brief commit message such as "Completed EDA notebook" and click to Sync All Changes. 

Click the Domino logo in the top left to return to your Domino project, then navigate to the Files tab of your project.

Notice that the latest commit will reflect the commit message you just logged and you can see 'EDA_code.ipynb' in your file directory.

<p align="center">
<img src = readme_images/DFS.png width="800">
</p>

Click on your notebook to view it. On the top of your screen click 'Link to Goal' in the dropdown, select the goal you created in Lab 1.2

<p align="center">
<img src = readme_images/LinkToGoal.png width="800">
</p>

Now navigate to Overview, then to the manage tab and see your linked notebook.

Click the ellipses on the goal to mark the goal as complete

<p align="center">
<img src = readme_images/MarkGoalComplete.png width="800">
</p>


### Lab 2.4 - Run and Track Experiments

Now it's time to train our models! 

We are taking a three pronged approach and building a model in sklearn (python), xgboost (R), and an auto-ml ensemble model (h2o).

First, navigate back to your JupyterLab workspace tab. In your file browser go into the scripts folder and inspect 'multitrain.py'

<p align="center">
<img src = readme_images/MultiTrain.png width="800">
</p>

Check out the code in the script and comments describing the purpose of each line of code.

You can also check out any of the training scripts that multitrain.py will call.

Open a terminal by clicking the blue plus icon then the terminal icon as shown below - 

<p align="center">
<img src = readme_images/Newterminal.png width="800">
</p>

Type in the following command and press enter

```shell
python scripts/multitrain.py
```

You should see the following output

<p align="center">
<img src = readme_images/MultiTrainKickOff.png width="800">
</p>

Now switch into your other browser tab to return to your domino project. Navigate to the Jobs page.

Watch as three job runs have appeared, you may see them in starting, running or completed state.

<p align="center">
<img src = readme_images/Jobs.png width="800">
</p>

Click into the sklearn.py job run.

In the details tab of the job run note that the compute environment and hardware tier are tracked to document not only who ran the experiment and when, but what versions of the code, software, and hardware were executed.

<p align="center">
<img src = readme_images/sklearnRunDetails.png width="800">
</p>


Click into the results tab of the job. Scroll down to view the visualizations and other outputs of the job.

<p align="center">
<img src = readme_images/sklearnResults.png width="800">
</p>


We've now trained 3 models and it is time to select which model we'd like to deploy.

Inspect the table and graph to understand the R^2 value and Mean Squared Error (MSE) for each model. From our results it looks like the sklearn model is the best candidate to deploy.

In the next section of labs we will deploy the model we trained here!

