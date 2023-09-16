## PURPOSE
The purpose of this deployment was to deploy an automated elastic deployment. This application will be deployed automatically to AWWS Elastic Beanstalk directly after the build and test stages.
## SYSTEM DIAGRAM 
![Deployment3Diagram drawio](https://github.com/Sameen-k/Deployment3/assets/128739962/c0589c4c-7c57-4636-bc30-11ae3e5cc387)
## STEPS
1. Download application files from Kura's GitHub Repo

2. Create your own GitHub Repo and include the Kura application files previously downnloaded (Make sure the files look the same, meaning when copying and pasting make sure the files that are in folders remain in their respective folders)

3. Login to Jenkins and begin to setup new branched pipeline. (GitHub Token will be needed. see next step)

4. Create a personal access token from GitHub (Make sure to save that token in notes in case you need it again. This will be the only time you'll be able to see it)

5. Run the pipeline on jenkins

9. Followed instructions for the install AWS EB CLI onto the ec2 instance

6. Return to your GitHub repo and within the jenkins file, add the deploy stage:
   
``stage ('Deploy') { steps { sh '/var/lib/jenkins/.local/bin/eb deploy' } }``
![Screen Shot 2023-09-15 at 11 49 02 PM](https://github.com/Sameen-k/Deployment3/assets/128739962/7cb6c09c-d551-4841-b08b-6d23fb6bb924)

8. Return to Jenkins and re-test the application. Now observe the additional stage in jenkins

<img width="621" alt="Screen Shot 2023-09-15 at 4 08 44 PM" src="https://github.com/Sameen-k/Deployment3/assets/128739962/5e46edc8-9993-45a5-a97f-0c39b0f29453">

9. Now  return to GitHub to configure a webhook, for the payload URL, type the IP of your ec2 instance along with the port jenkins is operating on (8080) then /github-webhook/
Example: http://32.237.129.42:8080/github-webhook/
*make sure to include the backslash at the end of the url otherwise the webhook won't be successfully be generated*

10. After creating the webhook, return back to the repository on GitHub, make a small change to the HTML files of the application files in the repository. I chose to alter the text "Website" to "WEBSITE"
 
12. If you observe jenkins, you'll see it re-ran automatically and redeployed as well. This means that the webhook has successfully connected to jenkins and Jenkins is now able to note any push changes made in the repository and it'll automatically trigger a new jenkins test/build

These logs on AWS reflect this. 

<img width="876" alt="Environment health has transitioned from Info to Ok" src="https://github.com/Sameen-k/Deployment3/assets/128739962/0516d964-568b-4cfd-b928-febffee11afb">
 
Expected changes in the application observed:
<img width="1274" alt="Screen Shot 2023-09-15 at 3 34 12 PM" src="https://github.com/Sameen-k/Deployment3/assets/128739962/981754e5-b838-4dae-a6a7-b29cc1ece1a8">
<img width="1283" alt="Screen Shot 2023-09-15 at 3 34 05 PM" src="https://github.com/Sameen-k/Deployment3/assets/128739962/2a19bf97-2e22-4765-b032-73d899df1686">

## ISSUES
One issue occured during the AWS EB CLI install onto the ec2 instance. To set up AWS EB CLI, after setting up the jenkins user on the ec2 terminal, the next step is to run the following command 

``pip install awsebcli --upgrade --user``

<img width="842" alt="lelcome to Ubuntu 22 04 2 LTS (GNULinux 5 19 0-1025-aws x86 64)" src="https://github.com/Sameen-k/Deployment3/assets/128739962/47fa3ef2-5145-49e5-b953-9ddaf5317e74">

However this command did not run and the terminal responce was to try to run the following command to install pip:

``sudo apt install pip``

After running the above command, the pip installation still failed and the reason was because the command was being run within the jenkins user. To get out of the jenkins user, the terminal page was just refreshed and then pip install command was run 

``sudo apt install pip``

After this command, the original command was also run which solved the issue entirely 

``pip install awsebcli --upgrade --user``

## OPTIMIZATION
A way to improve this deployment would be to automate the jenkins set up through a simple script as well automating the general set up of the ec2 by creating a script to automatically downlioad the necessary version of python as well as the unzip application and pip, etc.
