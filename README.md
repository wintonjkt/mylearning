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

**Configure ocp registry**  
  
oc adm ca create-server-cert \  
    --signer-cert=/etc/origin/master/ca.crt \  
    --signer-key=/etc/origin/master/ca.key \  
    --signer-serial=/etc/origin/master/ca.serial.txt \  
    --hostnames='docker-registry.default.svc.cluster.local,docker-registry.default.svc,161.202.177.33,docker-registry-default.161.202.177.33.nip.io' \  
    --cert=/etc/secrets/registry.crt \  
    --key=/etc/secrets/registry.key  
  
  
oc create secret generic registry-certificates \  
    --from-file=/etc/secrets/registry.crt \  
    --from-file=/etc/secrets/registry.key  
      
oc secrets link registry registry-certificates  
oc secrets link default  registry-certificates  
  
