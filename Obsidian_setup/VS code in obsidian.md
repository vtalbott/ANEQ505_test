### Mac 

1. Install VS code found here: https://code.visualstudio.com/download
2. Open Obsidian
3. Go to community plugins
4. Find and install Open vault in VS Code
5. Enable Open Vault in VS Code plug in
6. Now open VS code 
7. Open the command panel (CMD + Shift + P)
8. type  ``` Shell Command: Install 'code' command in PATH ```
9. You will get a confirmation message
10. Open a terminal
11. type ``` which code ```
12. You should get something that looks like ```/usr/local/bin/code``` copy this
13. Go back to obsidian and open the settings for the Open vault in VS code plug in 
14. Under the Open via `code` CL settings paste `/usr/local/bin/code` in front of what was already there so it should look like this:
`/usr/local/bin/code "{{vaultpath}}" "{{vaultpath}}/{{filepath}}"`
