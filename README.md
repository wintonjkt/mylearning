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
  
https://docs.openshift.com/container-platform/3.11/install_config/registry/securing_and_exposing_registry.html  

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
  
After exposing the registry, update your /etc/sysconfig/docker file by adding the port number to the OPTIONS entry. For example:  
  
OPTIONS='--selinux-enabled --insecure-registry=172.30.0.0/16 --insecure-registry registry.example.com:80'  
  
**oc cluster up with public IP**  
  
fgrep -RIl 127.0.0.1:8443 openshift.local.clusterup/ | xargs sed -i 's/127.0.0.1:8443/$PUBLIC_IP:8443/g'  
  
oc edit cm webconsole-config -n openshift-web-console -o yaml  
  
oc cluster up --public-hostname=x.x.x.x  
  
**Login as system:admin**  
  
oc login -u system:admin -n default --config=/root/ocp/openshift.local.clusterup/openshift-controller-manager/admin.kubeconfig   
  
