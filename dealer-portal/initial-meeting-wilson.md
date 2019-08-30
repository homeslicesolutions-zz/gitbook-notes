# Initial Meeting with Wilson

## Code-Walkthrough 
### Environments
* Sandbox
	* Client: `http://dealer.sandbox.fair.engineering/portal`
	* API: `https://api.sandbox-secure.fair.engineering`
* Staging
	* Client:  `https://dealer.stage.fair.engineering/portal`
	* API: `https://api.stage-secure.fair.engineering`
* Prod
	* Client: `https://dealer.prod.fair.engineering/portal`
	* API: `https://api.prod-secure.fair.engineering`

### Login
* You can either create one using the UI and wait for some one to give you permissions.
* OR you can use Wilson’s
	*  `wilsonf@fair.com` with password: `password`

### Main bugs:
* Web: Orders: All the table lists don’t have pagination hooked up
* Web: Super User: List needs have better UX for filtering
* Dev Env: Webpack: Hot reload is not working
* Dev Env: NPM: Fix Vulnerable dependencies

### Nice to haves:
* Fix "Blueprint" feature
* Documents => Reorganizing Documents data structure
* Serialize to Camel Casing in all data structure
* Fix Hot Reloading 
* GRPC-Web

### SF Suggestions:
* Add JS Doc
* Switch from Mocha to Jest for Snapshot Test and Coverage Report

## Deployment 
### Understanding branch nomenclature 
* `master`  
	* Trunk that represent Production 
* `dev`  
	* Ongoing newest code. Usually newer than /Master/ 
* `feature/[JIRA#]-*` or `bugfix/[JIRA#]-*`  
	* Represents local working branches 
* `release/[version]` -  
	* Computer-generated branch that is reserved as the workflow branch to be released and eventually merged to  
 
### Merge Recommendations 
* Rebasing is favored over "squash and merge" 
* Recommended steps to merge. In your local branch… 
	* Squash locally 
	* Rebase against `dev`  
	* Resolve the conflicts 
	* Re-push back to Origin 
	* Release to Sandbox 
   
### Releasing to Sandbox/Staging 
* Two ways of getting into sandbox/staging  
	* Merging to `dev` 
	* OR Comment in PR 
		 * Make a PR 
		 * Add a comment `/release sandbox` (or `stage`) 
 
### Workflow 
1. Create a branch with `feature/[JIRA#]` based off of `dev` 
2. Work on the feature, commit to Origin 
3. Create a PR 
4. Assign to `fair web` group and ping “Slack” maybe in a dev channel 
5. Release to sandbox 
	1. Comment in PR `/release sandbox` 
6. When ready for QA.
7. Go to Google Doc “Release Notes”
8. Find the current week’s release
9. Find the sheet correlating to the App and fill in info i.e. Jira and stuff
10. NOTE: Must go in by 11AM
11. Publish to staging from local by command: 
	1. `git flow release start [version]` 
	2. Creates release branch locally
	3. Manually push to origin for it to auto push to staging
12. Wait for Slack in the “release-notes” channel for QA giving the green light
13. If QA finds bugs, checkout the “release” branch, pull and make PRs against that release branch.
14. Release to Prod: Code Pipeline 
	1. Login to AWS-Stage 
	2. Go Pipeline-pipeline in the menu 
	3. Find your app i.e. `dealer-portal-v2` 
	4. You should see 3-steps and make sure the first 2 are done. 
	5. Re-check the commit (the link should be there) 
	 1. Click on “review” and add some baloney comment and click OK
15. Double check in Prod 
	1. Go to the webpage to see if it’s up 
	2. Check the Pods if they’re up and running 
		1.  `fair login` 
		2. 16. 16. `kubectl get pods | grep dealer-portal` 
16. Post-Release Reconciliation 
	1.  `git flow release finish [version]`
		1. Merges to local Master and Dev 
		2. Adds commit 
		3. Adds tag
	2. Manually Push local `dev` to origin
		1. `git push origin dev && git push --tags`
	3. Manually Push local master to origin
		 1. `git push origin master && git push --tags`