### Step 1: Install Git
- Install Git if you don't already have it.
https://git-scm.com/install/

- To check if you have Git enter this into the command line.  
```
git --version
```

### Step 2: Install Obsidian
- Obsidian Installation:
https://obsidian.md/download

### Step 3: Set up Obsidian Vault
- Open Obsidian it will give you the option to create new vault or open folder as vault
- Make a new vault, name it ANEQ505_HW and add make sure for the location you remember where you put it. 

### Step 4: Set up Git Hub repository
- Login to github and make a new repository 
	- (Click the little cat on the upper left hand corner and click the green button that says new)
- Name the repository ANEQ505_HW
- Make sure the visibility is set to public so we can see your homework.
- Make sure add readme is off. 
### Step 5: Set up access token in Git hub. 
- Click your profile picture
- Go to settings
- Scroll all the way down and on the left had side click developer settings.
- Click personal access tokens
- Click tokens (classic)
- Click generate new token
- Then generate new token (classic)
- You can put in ANEQ505 for note
- Set expiration to No expiration
- Select repo
- Scroll to bottom and generate token
- Copy the long string of letters and numbers and store it somewhere for later

### Step 6: Initialize Git in the vault
- Go to the directory with your vault in the command line/terminal. 
	- In macs you can go to the file then right click 
	- click services
	- then open terminal at file
- Enter the following commands 

Initialize 
```
git init
```
