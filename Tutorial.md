# Assignment 3 part 1
## Create a new regular user
### Create the user and their home directory

Assuming we are connected to the new droplet via the root user, the first step is to make a new, personal user so that we dont have to connect via root.
- To achive this, we use the command `useradd`
- Additionaly, we are going to use the following options:
  - `-m` which creates the users home directory based on the files in the skeleton directory.
  - `-s SHELL` which specifies the path to the login shell for the user. We are going to use bash.

Altogether, the command looks like this:

```bash
useradd -ms PATH <user-name>
```

**Example command**

I want to add the user 'IanM'. This is the command I would run:

```bash
useradd -ms /bin/bash IanM
```



### Grant the new user sudo privileges 

In order to complete the admin tasks for this system, the user needs to be added to a group named sudo.
This group grants them the elevated privileges required to run many of the commands involved with the rest of this tutorial.
- To add a user to a group, we use the command `usermod`
- Additionaly, we are going to use the following options:
  - `a` which adds the user to the groups we provide with the next option, without removing them from any groups they are currently in
  - `G GROUP1[,GROUP2,...[,GROUPN]]` which is where we provide the system with the group(s) that we want to add the user to.
 
Altogether, the command looks like this:

```bash
usermod -aG GROUP1,GROUP2 <user-name>
```

**Example command**

I want to add the user 'IanM' to the sudo group. This is the command I would run:

```bash
usermod -aG sudo IanM
```



### SSH connection 

Now that the user is set up for us, we need a way to connect to it. We can use the same ssh key by copying the content of our
root user ssh folder into the new user's home directory.

```bash
cp -r /root/.ssh /home/<user-name>
```

Verify it moved correctly with this command:

```bash
ls /home/<user-name> -al
```

We need to change the ownership too. To do that we use `chown`.
- The `<group-name>` is the same as your `<user-name>`
- `-R` means recursive. (This will change the ownership of every file in the directory)

```bash
chown -R <user-name>:<group-name> /home/<user-name>/.ssh 
```

Run this command again to verify the change in ownership.

```bash
ls /home/<user-name> -al
```

Now let's try it. 
1. Disconnect from your system.
2. Recconect with:

```
ssh -i <path-to-your-key> <user-name>@<debian machine ip> 
```

3. Admire your new account

## Prevent root user connection via SSH

**Dont skip anything above before following these steps. **

We don't want anyone to be able to SSH into the root user, especially now that you have your own user. 

To do this, we need to edit the ssh configuartion file on your linux system. 
1. Lets start by opening the file.

```bash
sudo vim /etc/ssh/sshd_config
```

2. Find the *PermitRootLogin* line.
3. Change the 'yes' to 'no'
4. Save and close the file
5. In order for the system to see our changes, we need to instruct it to refresh.

```bash
systemctl restart ssh.service
```

You should not be able to make an SSH connection to the root user of this system anymore

6. Double check.
Disconnect from the system and attempt to conect to the root user as you have in the past.
You should be denied access. 

## Install nginx

## Configure nginx
