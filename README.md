# websphere855
Udemy Course repo for WebSphere 8.5.5

## :link: Install IBMIM using script 
### extract IBMIM zip file 

### install IBMIM using the commandline script 

##  :link: install WASND
### extract the zip files in WASND directory 
cd $HOME/WAS_RELATED_FILES
mkdir WASND 
unzip -d WASND was.repo.8550.ndtrial_part1.zip
unzip -d WASND was.repo.8550.ndtrial_part2.zip
unzip -d WASND was.repo.8550.ndtrial_part3.zip

### list the package in the extracted zip file 
```
cd /opt/IBM/InstallationManager/eclipse/tools      # location of IBMIM installation 
./imcl listAvailablePackages -repositories $HOME/WAS-RELATED-FILES/WASND/repository.config
com.ibm.websphere.NDTRIAL.v85_8.5.5000.20130514_1044

```

### install WASND using the repository 
```
export IBMIM_ROOT=/opt/IBM/InstallationManager
cd ${IBMIM_ROOT}/eclipse/tools
./imcl install com.ibm.websphere.NDTRIAL.v85_8.5.5000.20130514_1044 \
-repositories $HOME/WAS-RELATED-FILES/WASND/repository.config \
-installationDirectory /opt/IBM/WebSphere85/AppServer \
-acceptLicense \
-sharedResourcesDirectory /opt/IBM/shared \
-showVerboseProgress
```
---
## setup the profiles on both boxes(servers)
### setup dmgr profile and custom profile1 on server1
```
export WAS_ROOT=/opt/IBM/WebSphere85/AppServer
cd  ${WAS_ROOT}/bin
./manageprofiles.sh -create \
-profileName Dmgr03 \
-profilePath /opt/IBM/WebSphere85/AppServer/profiles/Dmgr03 \
-templatePath  /opt/IBM/WebSphere85/AppServer/profileTemplates/management \
-hostName wasserver1 \
-serverType  DEPLOYMENT_MANAGER \
-nodeName ncdprddmgr \
-cellName ncdprdcell \
-enableAdminSecurity true  \
-adminUserName wasadmin  \
-adminPassword wasadmin
```

```
export WAS_ROOT=/opt/IBM/WebSphere85/AppServer
cd  ${WAS_ROOT}/bin
./manageprofiles.sh -create \
-profileName customprofile1 \
-profilePath /opt/IBM/WebSphere85/AppServer/profiles/customprofile1 \
-templatePath  /opt/IBM/WebSphere85/AppServer/profileTemplates/managed \
-hostName wasserver1 \
-nodeName ncdprd-node01
```
### start Dmgr
```
export WAS_ROOT=/opt/IBM/WebSphere85/AppServer
cd $WAS_ROOT/profiles/Dmgr03/bin
./startManager.sh
```
### federate the cusom profile int dmgr 
```
export WAS_ROOT=/opt/IBM/WebSphere85/AppServer
cd $WAS_ROOT/profiles/customprofile1/bin
./addNode.sh 192.168.1.181 8879 \
-username wasadmin \
-password wasadmin

```

### setup custom-profile2 on server2


#  :link: Miscellanious things 
## disabling security 
* https://geekflare.com/disable-websphere-security/