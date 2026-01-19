### Introduction to Alpine
#### What is Alpine?
- The CU Boulder High Performance Cluster (HPC)
- Basically, it is a supercomputer that is located at CU Boulder and CSU has access to

#### System Architecture
When you are trying to visualize what a supercomputer looks like, you can picture something like this:

![[Pasted image 20260119102147.png]]

#### Alpine Hardware:
- AMD Milan compute nodes
    - 64 cores / node
    - ~240 GB ram / node
    - An average laptop has around 4 cores & 8-16 GB ram, so each node is about 16X more powerful than your average laptop
- High memory AMD Milan compute nodes 
    - 48 cores / node
    - ~1 TB ram / node (😱)  
        
- GPU nodes
- Special high compute storage 
- Omni-path network
    - If we had an omni-path network, we could interconnect and put all of our laptops together to do one job to make a supercomputer
    - Alpine's resources combined with its omni-path network makes it a supercomputer
- General parallel file system (GPFS) - distribute and manage data across multiple servers

![[Pasted image 20260119102237.png]]

#### Nodes
Alpine has different kinds of nodes. For the purpose of this class, you can think of a node as different computer modes where you are given different types of compute resources and are meant for running different tasks. You can access these nodes in several ways (explained below). Alpine has 4 different types:

- **Login node**
    - This is where you "land" when you login
    - Do not run commands while in login
    - For script editing and job submissions  
          
        
- **Compile node**
    - Translates human-readable source code that we give into computer-executable machine code
    - Used to install software
    - This is where we will install QIIME2
    - Use `acompile` to start a compile session  
          
        
- **Compute node**
    - We do not navigate into these
    - This is where jobs run after submission
    - There are different kinds of compute nodes
    - Here is a list of the compute node resources available on Alpine ([link to more detailed info hereLinks to an external site.](https://curc.readthedocs.io/en/latest/clusters/alpine/alpine-hardware.html "(opens in a new window)") )
    - Use `sbatch` to submit jobs  
          
        
- **Interactive node**
    - We can navigate here to run jobs interactively 
    - We will use an interactive node to run our Qiime2 commands
    - Use `ainteractive` to start an interactive session

#### Directories (fancy word for folders)
There are also different **storage spaces** within Alpine that are meant for different things. 

- **Home directory**
    - /home/$USER
    - This is where you "land" when you first login
    - Not for direct computation
    - Small quota (2 GB)
    - Backed up  
          
        
- **Scratch directory**
    - /scratch/alpine/$USER
    - 10 TB quota - can ask for more if needed
    - High performance storage for computation
    - Files are purged after 90 days of inactivity - **Be careful about this!!!**
    - Folders will persist   
          
        
- **Project directory**
    - /projects/$USER
    - Mid-level quota (250 GB)
    - Large file storage
    - Not high performance storage
    - Backed up - if you choose to use Alpine for long-term storage, this is where you keep your files

Think of these spaces like folders on a computer, where "home" is a folder within "/" (root directory), and "$USER" is a folder within "home", and so on.

![[Pasted image 20260119102959.png]]

  
For this class, we will **work in the scratch directory** and back our files up to the projects directory. 

Alpine documentation: [https://curc.readthedocs.io/en/latest/Links to an external site.](https://curc.readthedocs.io/en/latest/index.html "(opens in a new window)") (can be really helpful!)


--------------------------------------------------------------
### Logging into Alpine

We will use the On Demand website to launch a terminal window that will connect us to the Alpine cluster.

1. Navigate to [ondemand-rmacc.rc.colorado.edu. Links to an external site.](http://ondemand-rmacc.rc.colorado.edu/ "(opens in a new window)")You should see this webpage:
![[Pasted image 20260119103150.png]]

2. Change ORCID to Colorado State University and click Log On.  
      
3. You will next be prompted to login with your NetID.  
![[Pasted image 20260119103241.png]]

4. You will then be prompted to use Duo to authenticate.
![[Pasted image 20260119103310.png]]

5. You should see the CU Research Computing OnDemand landing page.

![[Pasted image 20260119103334.png]]
6. Launch a terminal session by clicking on Clusters at the top and then select >_ Alpine.  This should open a new window that looks like this:
![[Pasted image 20260119103449.png]]
You have successfully logged into Alpine!


--------------------------------------------------------------
### File Transfers

When doing your microbiome analyses on Alpine, you will need a way to transfer files from your local computer to Alpine and vice versa. While there are several ways of doing this, our preferred way is to use OnDemand or FileZilla.  

#### OnDemand

1. After logging in, click on Files. You will see paths to your Home, Scratch, and Projects directories. Click on one!
![[Pasted image 20260119103638.png]]

2. This will show you all your folders and files in that directory.
![[Pasted image 20260119103702.png]]

3. From here you'll be able to:
    - Download files
    - Upload files
    - Rename files
    - Delete files
    - View contents of a file
    - Open a terminal window from that location


--------------------------------------------------------------

### Linux Navigation

Now we will go over general commands to use on the command line. These will work on MacOS and Linux systems, but not necessarily PCs. Here is the "anatomy" of what a basic command might look like.

![[Pasted image 20260119103759.png]]

- First, you have the **command** itself. `ls` is a command that says to list the files in your current working directory, which you can think of as the "folder" you are currently in. 
- You can supply **options** to the command.
    - For example, `ls -a` is saying to list _all_ of the files in your current directory, including any hidden files that start with a ".". 
    - The `-l` option says to list the directory contents in a long listing format
    - The `-G` option says to not print group names in a long listing
    - Explore more options to supply to the ls command [hereLinks to an external site.](https://man7.org/linux/man-pages/man1/ls.1.html "(opens in a new window)")
- You can supply **arguments** to the command.
    - The last line of the graphic above is saying to list all hidden files in a long listing format in the Downloads directory.

These are some common commands that you should learn, as we will use them throughout the semester:
![[Pasted image 20260119103831.png]]

Now let's explore. We have just logged into Alpine, so where are we? Let's use `pwd`, for "print working directory". The below is our first example of a code chunk, which are things that you should make sure you run along with me.

`pwd`

You should see an output that looks like /home/USER@colostate.edu. Remember the three different types of spaces (home, projects, scratch). We landed in the "home" space!

Now let's navigate back to the root, sometimes also referred to as home. This is not the home space that we are in now, but the home that contains our home, projects, and scratch directory. If you are confused, refer above to the spaces diagram, and we are navigating into the "/" area. 

`cd /`

We did this using cd, or "change directory." Now let's list the contents of our new directory.

`ls`

There are a lot of directories here that we will never use, but if you look closely, you can see our home, projects, and scratch spaces. Let's move into the projects space and list the contents of that directory. 

`cd projects`

`ls`

Whoa! There is a ton of usernames here. These are all of the project directories of people in Alpine. Let's navigate into our personal projects directory. This is a good opportunity to learn about tab complete. Type cd, the first few letters of your NetID, and then press tab for it to autocomplete, and then press enter.

`cd USERNAME`

`ls`

What kind of folders do you have in your projects directory?

Now let's move to the scratch directory. We can do this in one command, without having to go all the way back to "/". Then list the contents of that directory.

`cd /scratch/alpine/USER@colostate.edu`

`ls`

This is a good time to talk about **relative vs absolute paths**.

- An **absolute** path is the path directly from the root, as shown directly above.
- A **relative** path is a path based on where you are now. For example, if I am in the "alpine" directory, the path to get to the scratch directory would be:
    - ./USER@colostate.edu
    - The "." at the beginning of the path simply means "here." So we are here, and going to there. 

What if we want to know more about a specific command? Let's learn more about 'ls". We can do this using the "man" command.

`man ls`

Scroll through the page, and look at all of the extra arguments. You can also find the arguments we discussed during the "anatomy of a command" section.

#### Quick Exercise
Now that you are more familiar with the command line, you will do a small exercise on your own to see if you understand the navigation system. Use the Linux cheat sheet provided above to figure out how to do these steps if you aren't sure, and feel free to ask questions. 

1. Navigate to your projects directory
2. Create a new directory called "aneq505". 
    - Make sure not to use spaces or weird characters. Spaces have other meanings in the command line, so if you need a space, use an underscore. 
3. Navigate into your new directory.
4. Make a new text file and type a note to yourself about what this class is.
5. Name your text file and save it, then check in your aneq505 directory to make sure your text file and the text inside it is still there. 

If you would like more practice using the command line, here are some resources:

- A fun game for when you want to procrastinate but still be productive: [http://web.mit.edu/mprat/Public/web/Terminus/Web/main.htmlLinks to an external site.](http://web.mit.edu/mprat/Public/web/Terminus/Web/main.html "(opens in a new window)")
- Another Unix/Linux command cheat sheet: [https://files.fosswire.com/2007/08/fwunixref.pdf](https://files.fosswire.com/2007/08/fwunixref.pdf "(opens in a new window)")

--------------------------------------------------------------
### Keeping Track of Commands
- Come back to this after talking with Val