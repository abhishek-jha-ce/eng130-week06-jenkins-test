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

<p align="center">
  <img src="https://user-images.githubusercontent.com/110366380/200579153-09540d8f-a683-4a46-a252-9b2c67e4d347.png">
</p>

<p align="center">
  <img src="https://user-images.githubusercontent.com/110366380/200578770-5a848850-5021-4462-a186-35b0c76447e7.png">
</p>


