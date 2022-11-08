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


### Step 4: .


![image](https://user-images.githubusercontent.com/110366380/200581650-78e7bffd-935a-4e50-ae37-044f716a7324.png)


![image](https://user-images.githubusercontent.com/110366380/200582632-2a324f2b-700a-4558-8aa5-092b7c5cac0b.png)

![image](https://user-images.githubusercontent.com/110366380/200582974-ec96b0bb-2b10-4924-a525-539db5f969d8.png)


![image](https://user-images.githubusercontent.com/110366380/200583651-0738dc90-76df-4cd1-816a-b1ed235a895d.png)

- Select the key, and the error should disappear. Also change the `master` branch to `main` if you are using `main`.

![image](https://user-images.githubusercontent.com/110366380/200585444-85e45203-f579-42da-81be-7aabb592de82.png)


![image](https://user-images.githubusercontent.com/110366380/200587562-22d435ca-ab5a-462a-8eb1-ea7e8cbeb743.png)

selecting the node

![image](https://user-images.githubusercontent.com/110366380/200588384-6a19d87f-29d3-4117-b543-77380ae524f8.png)

click on build no

![image](https://user-images.githubusercontent.com/110366380/200588550-dfc00ddc-67e3-4767-8207-7f42574e6efb.png)

![cfd](https://user-images.githubusercontent.com/110366380/200591403-bdd59517-ad88-4e9d-8ac0-5ad241e1a742.png)


![image](https://user-images.githubusercontent.com/110366380/200591111-12fc8552-becc-4b54-bf4e-dc59a9a186f0.png)

![image](https://user-images.githubusercontent.com/110366380/200592967-26c1f849-7cf7-42a1-91b0-4a7c440b7f52.png)
