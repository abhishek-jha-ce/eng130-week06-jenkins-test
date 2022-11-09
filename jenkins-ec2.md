# AWS S3 task

## Proposed solution

![exercise3ec2 drawio](https://user-images.githubusercontent.com/110366380/200810379-741429a8-218a-4e37-933b-6ca43abdffc3.png)

## Create a New EC2 Instance

- Launch a new instance
- Provide `Name` and tags - Example: `eng130-abhishek-jenkins-app`
- AMI - select existing node AMI
- Instance Type - Leave default t2.micro
- Key Pair(login) - select eng130 , the existing .pem file
- Network settings
  - Choose your `VPC`
  - Choose `Public Subnet`
  - Auto-assign public IP - `Enable`
  - Security Groups - Create a new one. Example: `eng130-abhishek-jenkins-sg`
  - Add 4 security rule

| Type       | Protocol | Port Range | Source Type | Source               |
| ---------- | -------- | :--------: | ----------- | -------------------- |
| SSH        | TCP      | 22         | My IP       | My IP                |
| HTTP       | TCP      | 80         | Anywhere    | 0.0.0.0/0            |
| SSH        | TCP      | 22         | Custom      | jenkins-server-ip/32 |
| Custom TCP | TCP      | 3000       | Anywhere    | 0.0.0.0/0
 
- Launch the instance.

### Check if the instance is working

- `SSH` into the instance using the terminal. Example: ssh -i "eng130.pem" ubuntu@355.249.863.201
- Navigate to the `app` folder and type `npm start` to start the app.
- Copy the Pulic IP of the `app` Virtual Machine and check it in the browser to see it's working.
- If it's working delete the `app` folder from the `EC2 Instance`, we are copying it via `jenkins`. The app folder is already there as we are using an image to build it.
```
rm -rf app/ - Delete app folder
```

## Create a New Jenkins job

- Create a new Job.
- Select free style project.
- Next Section - fill out these details

##### General Tab

- Give a suitable description for the job
- Discard Old builds
  - Strategy - Log Rotation  
  - Max # of builds to keep: 3
- GitHub project
  - Provide url of the git hub project `https://github.com/abhishek-jha-ce/eng130-week06-jenkins-test.git/`

##### Office 365 Connector Tab

- Select `Restrict where this project can be run`
  - Label Expression `sparta-ubuntu-node`

##### Source Code Management Tab

- Select `Git`
  - Repositories - Repository URL `git@github.com:abhishek-jha-ce/eng130-week06-jenkins-test.git` the `SSH` link for the same git hub repo.
  - Credentials - Private Key created for jenkins. `eng130_jenkis_abhishek`. If previously used, select it.

  - Branches to build - Branch Specifier - `*/main`

##### Build Triggers Tab

- Leave this tab empty.

##### Build Environment Tab

- Select `Provide Node & npm bin/folder to PATH - use the default values generated.
- SSH Agent - Credentials - Copy and paste the `eng130.pem` file content. This is used to connect to our `EC2 Instances`.

##### Build Tab

- Select `Add build step` - `execute shell` and enter the following commands:

```
# To copy the files from git hub to ec2 instance
rsync -avz -e "ssh -o StrictHostKeyChecking=no" app ubuntu@[public ip]:/home/ubuntu
rsync -avz -e "ssh -o StrictHostKeyChecking=no" environment ubuntu@[public ip]:/home/ubuntu

ssh -A -o "StrictHostKeyChecking=no" ubuntu@[public ip] <<EOF
    cd app
    
    sudo killall -9 node # Kill any existing node process
    
    #bash provsion.sh
    npm install
    nohup node app.js > /dev/null 2>&1 & # To run node in background
EOF
```
- Save this job.

## Make Changes 

- Go the the `index.ejs` file in app and make some changes.
- Push the changes to github.
- We have not automated it yet, so we have to manually build the jenkins job.
- After build is successful, we can refresh the browser and see the changes.

<p align="center">
  <img src="https://user-images.githubusercontent.com/110366380/200844825-7ca51c73-b273-4571-93d7-b88743edb196.png">
</p>
            
## Automate the changes

- In task 2, we merged the `dev` branch to `main` branch.
- Go to Configure, in the `dev-test` job.
- In the `Post-build Actions` tab
  - Click on `Add post-build action` and select `Projects to build`
  - specify the name of this job (3rd job) in there. - `abhishek-ec2-integration`
