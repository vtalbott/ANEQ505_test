### Step 1: Install Git
- Install Git if you don't already have it.
	- https://git-scm.com/install/
- Git for windows
	- [https://gitforwindows.org/](https://nam10.safelinks.protection.outlook.com/?url=https%3A%2F%2Fgitforwindows.org%2F&data=05%7C02%7Ctubyez.lucky%40colostate.edu%7C3d643be3f4fe4899e28608ddcf923c0f%7Cafb58802ff7a4bb1ab21367ff2ecfc8b%7C0%7C0%7C638894949153995973%7CUnknown%7CTWFpbGZsb3d8eyJFbXB0eU1hcGkiOnRydWUsIlYiOiIwLjAuMDAwMCIsIlAiOiJXaW4zMiIsIkFOIjoiTWFpbCIsIldUIjoyfQ%3D%3D%7C0%7C%7C%7C&sdata=lccPDa%2BZLL9TLhlRqha%2FalLu2My7mLrHpRt6csRg1wc%3D&reserved=0 "Original URL: https://gitforwindows.org/. Click or tap if you trust this link.")

- To check if you have Git installed enter this into the command line.  
```
git --version
```

### Step 2: Install Obsidian
- Obsidian Installation:
https://obsidian.md/download

### Step 3: Set up Obsidian Vault
- Open Obsidian it will give you the option to create new vault or open folder as vault
- Make a new vault, name it ANEQ505_HW and make sure you remember where you put the folder/vault. Obsidian will have problem pushing/pulling to GitHub if the vault is in OneDrive or Dropbox. **Make sure to put the folder/vault in a place that is not backed up by a cloud service.**
- Delete the welcome note in the vault.

### Step 4: Set up GitHub repository
- Login to github and make a new repository 
	- (Click the little cat on the upper left hand corner and click the green button that says new)
- Name the repository ANEQ505_HW
- Make sure the visibility is set to public so we can see your homework.
- Make sure add readme is off. 
### Step 5: Set up access token in GitHub. 
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

Initialize Git
```
git init
```

Check branch name 
```
git branch
```
Create an initial commit
```
git add .
git commit -m "Initial commit"
```

Add GitHub as a remote
- the https can be found in you GitHub repository on the GitHub website
```
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO.git

```

Push and set upstream branch
```
git push -u origin main
```

- github will then through your terminal ask for your username type in your github username. 
- Then a field will popup with a lock asing for your password and paste in your access token from earlier. 

### Step 7: Set up Git in your obsidian vault
- Go to settings in your obsidian vault 
- Click community plugins
- Then click browse
- Then search for Git
- Select Git and then Install
- Then click enable. 
- Go to the obsidian git settings 
	- make sure that Custom base path is blank
	- If you don't see custom base path in your settings then it is already blank
- Restart obsidian

### Step 8: Push and Commit in obsidian
- Reopen your vault and add a new note and make a change to the note. 
- Then open the command pallet (command/control + p)
- Search for Git: commit and select it
- In the command palette again search for Git: push and select it. 
- Go to the obsidian git settings and select a time for automatic push/pull from git hub so you don't have to manually do it. 
### Step 9: Check your GitHub online to see if the files were committed. 
- You should see the file that you edited and an .obsidian file
- The .obsidian file has all the settings and community plugins so that If you or someone else downloads the vault they have those settings built in. 


#### Some Notes
For bigger pushes 
- This includes a lot of figures or if you haven't pushed in a while 
- Use this in the Terminal from anywhere to Ingrease Git's HTTP buffer. 
	- This prevents GitHub from dropping the connection during larger pushes. 

```
git config --global http.postBuffer 524288000
```



