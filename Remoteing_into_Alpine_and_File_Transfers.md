#### Remoting into Alpine
- The user and password are from you NetID
```
ssh user@colostate.edu@login.rc.colorado.edu
```

pasword: mypasword,duo_key
- The duo_key comes from the duo app it is the passcode you see when you open the app

#### Transfer Files from Local Computer to Alpine using rsync
rsync
Test
- If uploading multiple files at a time test with one file first
```
rsync -avP ~/OneDrive/Paired_Housing_Pempek_2024/well_water_metaG/02_data/raw/B3.zip \
vtalbott@colostate.edu@login.rc.colorado.edu:/scratch/alpine/vtalbott@colostate.edu/lee_wellwater_metaG_05_2026/metaG/
```

- After testing with one file you can upload all of the files.
All files
```
rsync -avP --partial *.zip vtalbott@colostate.edu@login.rc.colorado.edu:/scratch/alpine/vtalbott@colostate.edu/lee_wellwater_metaG_05_2026/metaG/
```

### Notes 
- I like to remote in first using ssh to make sure my password and pass code are working. 
- Then type exit that will take you out of alpine and back into your computer
- To transfer files, refresh the duo passcode; it won't work if you use the same passcode twice.
