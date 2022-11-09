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
- If it's working delete the `app` folder from the `EC2 Instance`, we are copying it via `jenkins`.
```
rm -rf/app to delete everything
```


<p align="center">
  <img src="https://user-images.githubusercontent.com/110366380/200844825-7ca51c73-b273-4571-93d7-b88743edb196.png">
</p>
            

