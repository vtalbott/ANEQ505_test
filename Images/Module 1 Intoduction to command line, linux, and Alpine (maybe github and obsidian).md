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

  
For this class, we will ~={red}**work in the scratch directory**=~ and back our files up to the projects directory. 

Alpine documentation: [https://curc.readthedocs.io/en/latest/Links to an external site.](https://curc.readthedocs.io/en/latest/index.html "(opens in a new window)") (can be really helpful!)


--------------------------------------------------------------
### Logging into Alpine
