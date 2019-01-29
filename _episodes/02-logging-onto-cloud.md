---
title: "Logging onto Cloud"
teaching: 5
exercises: 5
questions:
- How do I connect to an instance on the SURFsara's cloud?
objectives:
- Log onto to the cloud  user interface
- Start a VM based on a template
- Log off from a running instance
keypoints:
- You can use one set of log-in credentials for many instances
- Logging off an instance is not the same as turning off an instance
---

<script language="javascript" type="text/javascript">
function set_page_view_defaults() {
    document.getElementById('div_aws_win').style.display = 'block';
    document.getElementById('div_aws_unix').style.display = 'none';
};

function change_content_by_platform(form_control){
    if (!form_control || document.getElementById(form_control).value == 'aws_win') {
        set_page_view_defaults();
    } else if (document.getElementById(form_control).value == 'aws_unix') {
        document.getElementById('div_aws_win').style.display = 'none';
        document.getElementById('div_aws_unix').style.display = 'block';
        document.getElementById('div_hpc').style.display = 'none';
        document.getElementById('div_cyverse').style.display = 'none';
    } else {
        alert("Error: Missing platform value for 'change_content_by_platform()' script!");
    }
}

window.onload = set_page_view_defaults;
</script>

## Important Note

This lesson covers how to start a VM based on a already created image and template, and log in and out of a running instance

If you're returning post-workshop and want to launch your own instance, use follow the [SURFsara tutorial](https://doc.hpccloud.surfsara.nl/tutorials/) on how to create a template and instance from scratch.

## Background

To save time, your instructor configured a template for a virtual machine for you prior
to the workshop, and connected it to our lesson data.

## Access the User Interface

The User Interface (UI) is the web site that allows you to manage your _Virtual Machines_ (_VMs_) on the HPC Cloud.

### Log in to the UI

>**Note**
>
>You will receive your access credentials from the workshop facilitators.

* Open the UI page in your browser: [https://ui.hpccloud.surfsara.nl/](https://ui.hpccloud.surfsara.nl/)
* Enter the username & password provided
* Hit the `Login` button

<!---
#### Switch to "user view"

The interface supports several so-called views, which are a way to arrange information on the screen. For this course you should use the _user view_.

* Locate the *buddy* icon <i class="fa fa-user"></i> with your user name at the top-right corner of the screen and click it.
* In the drop-down menu, move to *<i class="fa fa-eye"></i> Views* and in that sub-menu select *user*.
-->

### Add your public SSH key

To complete the setup of your HPC Cloud account, you need to **add a Secure Shell (SSH) public key to your UI account**. This is a one-time task!

>**Note**
>
>If you are not familiar with the SSH authentication method, please read about it on our [documentation page](https://doc.hpccloud.surfsara.nl/SSHkey).


First, you need to own an SSH private/public key pair.

To create a new SSH key pair:

1. Open a terminal on Linux or macOS, or Git Bash / WSL on Windows.
2. Generate a new ED25519 SSH key pair:
```ssh-keygen -t ed25519 -C "email@example.com"```
Or, if you want to use RSA:
```ssh-keygen -o -t rsa -b 4096 -C "email@example.com"```
The -C flag adds a comment in the key in case you have multiple of them
and want to tell which is which. It is optional.


3. Next, you will be prompted to input a file path to save your SSH key pair to.
If you don't already have an SSH key pair, use the suggested path by pressing
Enter. Using the suggested path will normally allow your SSH client
to automatically use the SSH key pair with no additional configuration.

If you already have an SSH key pair with the suggested file path, you will need
to input a new file path and declare what host
this SSH key pair will be used for in your ~/.ssh/config file.


4. Once the path is decided, you will be prompted to input a password to
secure your new SSH key pair. It's a best practice to use a password,
but it's not required and you can skip creating it by pressing
Enter twice.
If, in any case, you want to add or change the password of your SSH key pair,
you can use the -pflag:
```ssh-keygen -p -o -f <keyname>```

Next, you need to copy the public SSH key (`id_rsa.pub`) to the UI. The matching private key (`id_rsa`) remains safe in your laptop.

* Copy the content of your **public** SSH key to the clipboard (e.g. `cat ~/.ssh/id_rsa.pub`, then select and copy all the text)
* Go to the [UI](https://ui.hpccloud.surfsara.nl/) and select *<i class="fa fa-cog"></i> Settings* from the *buddy* icon  <i class="fa fa-user"></i>
* Locate the section _Public SSH Key_ and click on the blue edit icon <i class="fa fa-pencil-square-o" style="color:#0098c3;"></i>
* Paste the content of your public SSH key file into the text box
* There is no _Save_ button. Click outside the text box to complete your action
* Briefly check the contents of the text box against your public key and verify they match: it should start with `ssh-rsa AAAAB`...

### My first VM

But first, you need a place to log *into*! To find the instance that's attached to that data,
you'll need something called an IP address. Your instructor should have given this to you
at the beginning of the workshop.

An IP address is essentially the numerical version of a web address like www.amazon.com

Recall that cloud computing is about choice. You can rent just a single processor on a large computer
for a small project, or you can rent hundreds of processors spread across multiple computers for
a large project. In either case, once you rent the collection of processors, Amazon will
present your rental to you as if it was a single computer. So, the physical computers that host your
instances don't really move, *but* every time you launch a new instance, it will have a new IP address.

So, each time you launch a new instance, the *IP address* changes, but your *Log-in Credentials* don't have to.

## Connection Protocols

We will use a protocol called Secure Shell (SSH) that, as the name implies, provides you
with a secure way to use a [shell](http://swcarpentry.github.io/shell-novice). In our case,
the shell will be running on a remote machine. This protocol is available for every
operating system, but sometimes requires additional software.

## Logging onto a cloud instance

**Please select the platform you wish to use for the exercises: <select id="id_platform" name="platformlist" onchange="change_content_by_platform('id_platform');return false;"><option value="aws_unix" id="id_aws_unix" selected> AWS_UNIX </option><option value="aws_win" id="id_aws_win" selected> AWS_Windows </option></select>**


<div id="div_aws_win" style="display:block" markdown="1">

#### Connecting using PC

*Prerequisites*: You must have an SSH client. There are several free options but you should have installed [PuTTY.exe](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) at the begining of the workshop, and we're going to continue using that.


1. Open PuTTY
2. Paste in the 'Host Name (or IP address)' section the IP address provided by your instructor (or the IP address of an instance you have provisioned yourself)

    *Keep the default selection 'SSH' and Port (22)*

    ![](../fig/putty_screenshot_1.png)

2. Click 'Open' 
    
    You will be presented with a security warning

    ![](../fig/putty_screenshot_2.png)

3. Select 'Yes' to continue to connect
3. In the final step, you will be asked to provide a login and password
    
    **Note:** When typing your password, it is common in Unix/Linux not see see any asterisks (e.g. `****) or moving cursors. Just continue typing

    ![](../fig/putty_screenshot_3.png)

You should now be connected!

</div>


<div id="div_aws_unix" style="display:block" markdown="1">


#### Connecting using Mac/Linux

Mac and Linux operating systems will already have terminals installed. 

1. Open the terminal

    Simply search for 'Terminal' and/or look for the terminal icon

    ![terminal icon](../fig/terminal.png)

2. Type the following command substituting `ip_address` by the IP address your instructor will provide (or the IP address of an instance you have provisioned yourself)

    ~~~
    $ ssh dcuser@ip_address
    ~~~
    {: .bash}

    *Be sure to pay attention to capitalization and spaces*

3. You will receive a security message that looks something like the message below

    ~~~
    The authenticity of host 'ec2-52-91-14-206.compute-1.amazonaws.com (52.91.14.206)' can't be established.
    ECDSA key fingerprint is SHA256:S2mMV8mCThjJHm0sUmK2iOE5DBqs8HiJr6pL3x/XxkI.
    Are you sure you want to continue connecting (yes/no)?
    ~~~
    {: .bash}

4. Type `yes` to proceed
5. In the final step, you will be asked to provide a login and password
    
    **Note:** When typing your password, it is common in Unix/Linux not see any asterisks (e.g. `****`) or moving cursors. Just continue typing.

You should now be connected!

</div>

## Logging off a cloud instance

Logging off your instance is a lot like logging out of your local computer: it stops any processes
that are currently running, but doesn't shut the computer off. AWS instances acrue charges whenever
they are running, *even if you are logged off*.

If you are *completely* done with your AWS instance, you will need to **terminate** it after you log off. Instructions for terminating an instance are here: [launching cloud instances on your own](../LaunchingInstances).

To log off, use the `exit` command in the same terminal you connected with. This will close the connection, and your terminal will go back to showing your local computer:

~~~
dcuser@ip-172-31-62-209 $ exit

Amandas-MacBook-Pro-3 $
~~~
{: .bash}

## Logging back in

Internet connections can be slow or unstable. If you're just browsing the internet, that means you have
reload pages, or wait for pictures to load. When you're working in cloud, that means you'll sometimes
be suddenly disconnected from your instance when you weren't expecting it. Even on the best internet
connections, your signal will occasionally drop, so it's good to know the above SSH steps, and be able
to log into AWS without looking up the instructions each time.

In the next section, we'll also show you some programs that you can use to keep your processes going
even if your connection drops. But for now, just practice logging on and off a few times.
