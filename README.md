# mylearning
  
  
  
  
**Run VSCode as Root**  
  
1. Create a working directory 
2. Run from terminal: sudo code --user-data-dir your_working_dir_path
  
  

**Set UBUNTU Desktop app to always run as root**  
  
Pin the application to the launcher as normal.  
  
Locate the applications .desktop file which will be in either:  
  
/usr/share/applications/APPNAME.desktop  
~/.local/share/applications/APPNAME.desktop  
or somewhere else, use locate .desktop|grep APPAME  
Open with gedit:  
  
gksudo gedit /usr/share/applications/APPNAME.desktop  
Then change the line  
  
Exec=APP_COMMAND  
to  
  
Exec=gksudo -k -u root APP_COMMAND  
Save  
