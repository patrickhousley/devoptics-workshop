# DevOptics Value Streams
A CloudBees DevOptics Value Stream models a complex continuous delivery process and can be assembled from multiple pipelines. A Value Stream shows a series of interconnected gates or steps that deliver value to a customer. Those steps are presented in the "Value Stream view". The Value Stream view allows you to:

* Track changes
* Detect stalled and delayed changes
* Identify failures and blockages
* Find components ready for testing
* View contributing components

### Value Stream Visual Editor
DevOptics visual editor lets you model different phases and gates of your value stream.

* Phases - the milestones or stages used to deliver the software
* Gates - the Jenkins pipeline that is run to move the code change forward (e.g. build, test, deploy, …​)

Once modeled you can instrument the Jenkins Pipelines from the CloudBees Core Pipeline Workshop so DevOptics is able to track tickets and commits flowing through the value stream end to end.

1. Go back to DevOptics in your browser and switch to the **Value Streams** view and click on the **Create value stream** button to open the [**Value Stream Visual Editor**](https://go.cloudbees.com/docs/cloudbees-documentation/devoptics-user-guide/value_streams/#devoptics-visual-editor)<p><img src="img/streams/value_streams_views.png" width=800/>
2. Click on the default title in the upper-left and change the title to be **{your GitHub username} helloworld-api** and hit return <p><img src="img/streams/change_title.png" width=800/>
3. Next click on the **Untitled Gate** below the **Build** phase and then click on the **cog** to configure the gate <p><img src="img/streams/configure_build_gate.png" width=800/>
4. Fill out the gate configuration form - **IMPORTANT:** Replace ***{your GitHub username}*** with your GitHub username/Team Master name and replace ***{GitHub Org name}*** with your GitHub Organization name you are usign for this workshop:
  - **Gate Name**: development branch
  - **Master**: https://workshop.cb-sa.io/teams-{your GitHub username}-do/
  - **Job**: {your GitHub username}-do/{GitHub Org name}/helloworld-api/development
  - **Phase**: Build
  - leave the other fields as is <p><img src="img/streams/build_gate_form.png" width=500/>
  - **Save** the form
5. Click on the **Untitled Gate** below the **Test** phase, then click on the **cog** to configure the gate and fill out the gate configuration form:
  - **Gate Name**: test branch
  - **Master**: https://workshop.cb-sa.io/teams-{your GitHub username}-do/
  - **Job**: {your GitHub username}-do/{GitHub Org name}/helloworld-api/test
  - **Phase**: Test
  - leave the other fields as is 
  - **Save** the form
6. Click on the **Untitled Gate** below the **Release** phase, then click on the **cog** to configure the gate and fill out the gate configuration form:
  - **Gate Name**: master branch
  - **Master**: https://workshop.cb-sa.io/teams-{your GitHub username}-do/
  - **Job**: {your GitHub username}-do/{GitHub Org name}/helloworld-api/master
  - **Phase**: Release
  - Check the **This is a deployment job** checkbox <p><img src="img/streams/release_gate_form.png" width=500/>
  - **Save** the form
7. Click on the **Save** button to save the Value Stream <p><img src="img/streams/save_value_stream.png" width=800/>
8. Done! Your DevOptics Value Stream is ready for review. <p><img src="img/streams/onsave_value_stream.png" width=800/>

### DevOptics Performance Metrics
Now that you have an initial Value Stream, we will commit a number of changes to your fork of the  ***helloworld-api*** repository to simulate [DevOptics Performance metrics](https://go.cloudbees.com/docs/cloudbees-documentation/devoptics-user-guide/value_streams/#_devops_performance_metrics).

1. Navigate to and open the GitHub editor for the `Jenkinsfile` file in **development** branch of your forked **helloworld-api** repository <p><img src="img/streams/hwapi_dev_branch.png" width=800/>
2. Update the Jenkinsfile to fail in the `Test` stage by adding the following `error` step
```
        error 'fake error to force failure in test stage/gate'
``` 
3. Commit the change to your **development** branch with a commit message of: **API-1001 new feature**  <p><img src="img/streams/fake_error_step.png" width=700/>
4. Once the job for your **development** branch completes, switch to your DevOptics Value Streams window and you should see the **development branch** gate updated to show that it has 1 ticket in it and that it completed successfully<p><img src="img/streams/metrics_dev_branch.png" width=800/>
5. Now we will create a [Pull Request](https://help.github.com/en/articles/creating-a-pull-request) between the **development** branch and **test** branch of your forked **helloworld-api** repository. Navigate to your forked **helloworld-api** repository in GitHub - click on the **New pull request** button
6. Change the **base repository** to the **test** branch of your forked **helloworld-api** repository (not the **cloudbees-dwjw** repository), add a comment and then click the **Create pull request** button <p><img src="img/streams/metrics_dev_to_test_pr.png" width=700/>
7. Click the **Merge pull request** button and then click the **Confirm merge** button but **DO NOT delete the development branch**
8. Switch to your DevOptics Value Streams window and you should eventually see the **test branch** gate updated to show that it has 1 ticket in it and that it failed<p><img src="img/streams/metrics_test_branch_fail.png" width=800/>
9. Delete the `error` step from the `Test` stage on the **development** branch of your fork of the  ***helloworld-api*** repository and commit the change to the **development** branch with the following commit message: **API-1001 fix**
10. Create another [Pull Request](https://help.github.com/en/articles/creating-a-pull-request) between the **development** branch and **test** branch of your forked **helloworld-api** repository. Navigate to your forked **helloworld-api** repository in GitHub - click on the **New pull request** button
11. Change the **base repository** to the **test** branch of your forked **helloworld-api** repository (not the **cloudbees-dwjw** repository), add a comment and then click the **Create pull request** button <p><img src="img/streams/metrics_dev_to_test_pr_fix.png" width=700/>
12. Click the **Merge pull request** button and then click the **Confirm merge** button but **DO NOT delete the development branch**
13. Switch to your DevOptics Value Streams window and you should eventually see the **test branch** gate updated to show that it has 1 ticket in it and that it is successful. Note the *Change Failure Rate* and the *Mean Time To Recovery* of the **test gate**<p><img src="img/streams/metrics_test_branch_success.png" width=800/>
14. So now we are ready to merge to the **master** branch and release. Create another [Pull Request](https://help.github.com/en/articles/creating-a-pull-request) between the **test** branch and **master** branch of your forked **helloworld-api** repository. Navigate to your forked **helloworld-api** repository on the **test** branch in GitHub - click on the **New pull request** button<p><img src="img/streams/metrics_test_to_master_pr.png" width=700/>
15. Change the **base repository** to the **master** branch of your forked **helloworld-api** repository (not the **cloudbees-dwjw** repository), add a comment and then click the **Create pull request** button
16. Click the **Merge pull request** button and then click the **Confirm merge** button but **DO NOT delete the test branch**
17. Switch to your DevOptics Value Streams window and you should eventually see the **master branch** gate updated to show that it has 1 ticket in it and that it is successful<p><img src="img/streams/metrics_master_branch_success.png" width=800/>
18. Next, click on the **master branch** gate and you will now have gate metics for *Mean Lead Time*, *Deployment Frequency*, *Mean Queue Time* and *Mean Processing Time*.<p><img src="img/streams/metrics_master_gate_df.png" width=500/>
19. Next, click on the *chevron* icon to switch to view the metrics for the entire value stream as opposed to jsut the **master gate**<p><img src="img/streams/metrics_for_value_stream.png" width=500/>

### Value Stream JSON Editor

In addition to the Visual Editor, DevOptics also provides a JSON editor. The JSON editor is only available to update a Value Stream created by the Visual Editor not create one. The following example will walk you through creating another value stream using the JSON editor that encompasses two separate repositories (services).

1. Go back to DevOptics in your browser and switch to the **Value Streams** view and click on the **Create New** button in the upper right corner <p><img src="img/streams/value_streams_views.png" width=800/> This will open the [**Value Stream Visual Editor**](https://go.cloudbees.com/docs/cloudbees-documentation/devoptics-user-guide/value_streams/#devoptics-visual-editor)
2. Click on the default title in the upper-left and change the title to be **{your GitHub username} Hello App**, hit return and then click the **Save** button in the upper-left to save the Value Stream<p><img src="img/streams/json_change_title.png" width=800/>
3. Next, click on the the 3 vertical dots next to the ***Edit*** button and select **Edit JSON** from the menu <p><img src="img/streams/edit_json.png" width=800/>
4. Delete the **Value Stream JSON** content and replace with the following, replacing all occurences of **{your GitHub username}**, both for the `master` value and the `job` value, in the JSON with your GitHub username and ***{GitHub Org name}*** with the GitHub Organization you are using for this workshop:
```
{
	"phases": [
		{
			"id": "TSlZ0sqDuh",
			"name": "Build",
			"gates": [
				{
					"id": "5eMvajavv",
					"name": "API Dev",
					"master": "https://workshop.cb-sa.io/teams-{your GitHub username}-do/",
					"job": "{your GitHub username}-do/{GitHub Org name}/helloworld-api/development",
					"feeds": "v-4fcgpTY",
					"fanout": []
				},
				{
					"id": "ZF5Ou-7vv",
					"name": "UI Dev ",
					"master": "https://workshop.cb-sa.io/teams-{your GitHub username}-do/",
					"job": "{your GitHub username}-do/{GitHub Org name}/helloworld-nodejs/development",
					"feeds": "release",
					"fanout": []
				}
			]
		},
		{
			"id": "QLggEt9hI",
			"name": "Test",
			"gates": [
				{
					"id": "v-4fcgpTY",
					"name": "API Test",
					"master": "https://workshop.cb-sa.io/teams-{your GitHub username}-do/",
					"job": "{your GitHub username}-do/{GitHub Org name}/helloworld-api/test",
					"feeds": "yiK5MUR5Q",
					"fanout": []
				}
			]
		},
		{
			"id": "ExwDnpexcA",
			"name": "Deploy API",
			"gates": [
				{
					"id": "yiK5MUR5Q",
					"name": "API Master",
					"master": "https://workshop.cb-sa.io/teams-{your GitHub username}-do/",
					"job": "{your GitHub username}-do/{GitHub Org name}/helloworld-api/master",
					"feeds": "release",
					"fanout": [],
					"type": "deployment"
				}
			]
		},
		{
			"id": "8THbC9EXqX",
			"name": "Deploy App",
			"gates": [
				{
					"id": "release",
					"name": "UI Master",
					"master": "https://workshop.cb-sa.io/teams-{your GitHub username}-do/",
					"job": "{your GitHub username}-do/{GitHub Org name}/helloworld-nodejs/master",
					"fanout": [],
					"type": "deployment",
					"feeds": null
				}
			]
		}
	]
}
```
5. Click the **Save changes** button <p><img src="img/streams/save_json.png" width=800/>
6. You will have a new Value Stream <p><img src="img/streams/microservice_vs.png" width=800/>.

### The **helloworld-nodejs** Application

The **helloworld-nodejs** application is a simple NodeJS app that serves a simple index page with a message. 

If you navigate to **helloworld-nodejs** in your Jenkins master, you'll notice that there are two stages, a `Web Tests` stage used to test the NodeJS app using a selenium sandbox which runs on the `development` branch and a `Build and Push Image` stage that runs on `master`. <p><img src="img/streams/hwnodejs_dev_branch.png" width=800/>

The `UI Dev` gate of the value stream represents the `development` branch of the **helloworld-nodejs** app. The `Web Tests` stage tests whether the hosted nodejs app shows a body of `Hello World!`, spelled correctly. The current state of the app has an error (the body shows `Hello Worlld`) that we will fix in the next few steps. <p><img src="img/streams/hwnodejs_failed_test.png" width=800/>

### Fixing the **helloworld-nodejs** Application

1. In DevOptics you should see that the **UI Dev** gate has failed due to the error mentioned above. <p><img src="img/streams/failed_ui_dev.png" width=800/>
2. Open the GitHub editor for the `hello.js` file in the **development** branch of your forked **helloworld-nodejs** repository, on line 13 fix the misspelled ***Worlld***   
```
res.render('index', { title: 'Hello', message: 'Hello World!', 
```
3. Commit the change to the **development** branch with the commit message ***UI-1001 fixed misspelling*** <p><img src="img/streams/fix_ui_misspelling.png" width=800/>
4. Now your job should complete successfully and the **UI Dev** gate should show that it finished successfully - what is your MTTR for the **UI Dev** gate? What is the MTTR for the entire Value Stream?  <p><img src="img/streams/ui_dev_vs_success.png" width=800/>
5. Now that you have fixed your **helloworld-nodejs** application it is time to merge to the **master** branch and deploy. Create a [Pull Request](https://help.github.com/en/articles/creating-a-pull-request) between the **development** branch and **master** branch of your forked **helloworld-nodejs** repository. 
6. Changed the **base repository** to the **master** branch of your forked **helloworld-nodejs** repository (not the **cloudbees-dwjw** repository), add a comment and then click the **Create pull request** button <p><img src="img/streams/ui_dev_to_master_pr.png" width=600/>
7. A job will be created for the pull request and once it has completed successfully your pull request will show that **All checks have passed**. Go ahead and click the **Merge pull request** button and then click the **Confirm merge** button but DO NOT delete the **development** branch <p><img src="img/streams/ui_merge_pr.png" width=600/>
8. In your DevOptics Value Stream you should see the **UI-1001** ticket move from the **UI Dev** gate to the **UI Master** gate. What is your deployment frequency? <p><img src="img/streams/final_microservice_vs.png" width=800/>

The next DevOptics feature that we will look at is [DevOptics Run Insights](./insights.md).

