# eng130-week06-jenkins-test

## Steps

### Step 1: Create a new `github repository`.

### Step 2: Create a new `SSH` key pair.

Use the following command to create a new SSH key.

```
ssh-keygen -t rsa -b 4096 -C "<youremail>@gmail.com"

// Provide a filename following the proper naming convention

Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/abhis/.ssh/id_rsa): /c/Users/abhis/.ssh/eng130_jenkins_abhishek
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /c/Users/abhis/.ssh/eng130_jenkins_abhishek
Your public key has been saved in /c/Users/abhis/.ssh/eng130_jenkins_abhishek.pub
The key fingerprint is:
**************
The key's randomart image is:
*******************
******************
```

- We now have 2 new keys generated:
  - Public key `eng130_jenkins_abhishek.pub`. 
  - Private/Identification key `eng130_jenkins_abhishek`.

Note: Make sure to follow the naming convention, otherwise it would overwrite the existing `ssh key` used to connect to github.

### Step 3: Copy the `public key` to the github repo.

- Use `cat` command to view the public key.

```
$ cat eng130_jenkins_abhishek.pub
```

- Copy the displayed public key, make sure not to copy the whitespaces.

- Navigate to the github repository and click on `settings`. Then on the left pane, select `Deploy keys`.

- Click on `Add deploy key`.

- Give a suitable title for e.g. `jenkins-key`.

- Paste the copied key in the `Key` section. Save it.

<p align="center">
  <img src="https://user-images.githubusercontent.com/110366380/200579153-09540d8f-a683-4a46-a252-9b2c67e4d347.png">
</p>

<p align="center">
  <img src="https://user-images.githubusercontent.com/110366380/200578770-5a848850-5021-4462-a186-35b0c76447e7.png">
</p>


### Step 4: Create a New Job in Jenkins

#### Step 4.1: Create a New Job
- Select a name for the Job.
- Select freestyle project and click OK.

<p align="center">
  <img src="https://user-images.githubusercontent.com/110366380/200581650-78e7bffd-935a-4e50-ae37-044f716a7324.png">
</p>

- In the next page, Provide a Description for the Job.
- Select `Discard old builds` and enter 3 for `Max # of builds to keep`.
- Copy the `HTTPS` link from `Code -> Clone-> HTTPS` from github repository and paste it into `Project url`

<p align="center">
  <img src="https://user-images.githubusercontent.com/110366380/200582632-2a324f2b-700a-4558-8aa5-092b7c5cac0b.png">
</p>

#### Step 4.2: Source Code Management

- In `Source Code Management Section` Select `Git`.

- In the `Repositories` Section, provide the `SSH` link from `Code -> Clone-> SSH` from the github repository.

<p align="center">
  <img src="https://user-images.githubusercontent.com/110366380/200582974-ec96b0bb-2b10-4924-a525-539db5f969d8.png">
</p>

- We will see an error displayed on the screen as we haven't provided the credentials for the key yet. The `SSH` Key pair we made earlier, The public key goes inside the github repo as a `deploy key`. 
- The `private identification key` needs to be added to `jenkins` for it to be linked to `github` using `SSH`.
- Click on the `Add` button, and in the next screen for `Add Credentials`:
  - Select the `Kind` to `SSH Username with private key`
  - Provide a username: `eng130_jenkins_abhishek`
  - Select `Enter directly` for the private key and copy and paste the private key from the bash terminal `$ cat eng130_jenkins_abhishek`.
  - Click on Add Key

<p align="center">
  <img src="https://user-images.githubusercontent.com/110366380/200583651-0738dc90-76df-4cd1-816a-b1ed235a895d.png">
</p>

- Select the key, and the error should disappear. 
- Change the `master` branch to `main` if you are using `main`.
- The screen should look like this:

<p align="center">
  <img src="https://user-images.githubusercontent.com/110366380/200585444-85e45203-f579-42da-81be-7aabb592de82.png">
</p>

#### Step 4.3: Change the build environment

- In the `Build Environment` Section. Select `Provide Node & npm bin/folder to PATH.
- This step is required as we need Node and npm installed on the system.
- In the `Build` Section provide with the sricpts that we need to execute.


<p align="center">
  <img src="https://user-images.githubusercontent.com/110366380/200587562-22d435ca-ab5a-462a-8eb1-ea7e8cbeb743.png">
</p>

#### Step 4.4: Specify a `Agent Node` to do the work instead of `Master Node`

- In `Office 365 Connector` section select `Restrict where this project can be run` and in `Label Expression` select `sparta-ubuntu-node`

<p align="center">
  <img src="https://user-images.githubusercontent.com/110366380/200588384-6a19d87f-29d3-4117-b543-77380ae524f8.png">
</p>


- Now click on the build in 'Jenkins` for the job we created. We can see the options availble.

<p align="center">
  <img src="https://user-images.githubusercontent.com/110366380/200588550-dfc00ddc-67e3-4767-8207-7f42574e6efb.png">
</p>

### Step 5: Automate using `Webhooks` 

#### Step 5.1: Setup Webhooks in GitHub

- To use webhooks, we navigate back to github, and select `Webhooks` in the setting page:

<p align="center">
  <img src="https://user-images.githubusercontent.com/110366380/200591403-bdd59517-ad88-4e9d-8ac0-5ad241e1a742.png">
</p>

- Click on `Add webhook` and enter the `Payload URL`. This is the same url we use for jenkins:

<p align="center">
  <img src="https://user-images.githubusercontent.com/110366380/200591111-12fc8552-becc-4b54-bf4e-dc59a9a186f0.png">
</p>

#### Step 5.2: Allow GitHub triggers in Jenkins

- In Jenkins, Select `Configure` for the build we want to add webhooks to and in the `Build Triggers` section select `GitHub hook trigger for GITScm polling`

<p align="center">
  <img src="https://user-images.githubusercontent.com/110366380/200592967-26c1f849-7cf7-42a1-91b0-4a7c440b7f52.png">
</p>


## Exercise 1

### Create a `dev` branch, make some changes and commit triggers job

- Create a new branch called `dev` and switch to that branch.

```
$ git checkout -b dev
```

- Add a new `test case` to the existing `testserver.js` file:

```
it('should display the correct fibonacci value at /fibonacci/6 GET', function(done) {
    chai.request(server)
      .get('/fibonacci/6')
      .end(function(err, res){
        res.should.have.status(200);
        res.text.should.contain('8');
        done();
      });
```

- Commit the changes and push it to github.

- As soon as we push it to git hub, the jenkins build is automatically run. We can see the new build `#7 at 15:37`.

<p align="center">
  <img src="https://user-images.githubusercontent.com/110366380/200608897-1b97b053-2128-41e9-8bc8-4ded7e64c4dd.png">
</p>


