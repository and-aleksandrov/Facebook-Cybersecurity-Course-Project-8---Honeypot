# Facebook Cybersecurity Course Project 8: Honeypot

## Objective: Setup a honeypot and provide a working demonstration of its features.

### 1. Overview & Setup | Which Honeypot(s) you deployed

* To deploy honeypots I've used Google Cloud. I've downloaded and installed Google Cloud Platform SDK so I was able to run ```gcloud``` from a command line.

* For the VM i've used Ubuntu 14.04 with following open ports: 80, 3000, 10000. I've created the following firewall rules with command
   ``` 
   $ gcloud beta compute firewall-rules create mhn-allow-admin --direction=INGRESS --priority=1000 --network=default --action=ALLOW -- rules=tcp:3000,tcp:10000 --source-ranges=0.0.0.0/0 --target-tags=mhn-admin
   ```
   Next step was to set up admin virtual machine:
   ```
   $ gcloud compute instances create "mhn-admin" --machine-type "f1-micro" --subnet "default" --maintenance-policy "MIGRATE"  --scopes "https://www.googleapis.com/auth/devstorage.read_only","https://www.googleapis.com/auth/logging.write","https://www.googleapis.com/auth/monitoring.write","https://www.googleapis.com/auth/servicecontrol","https://www.googleapis.com/auth/service.management.readonly","https://www.googleapis.com/auth/trace.append" --tags "mhn-admin","http-server","https-server" --image "ubuntu-1404-trusty-v20171010" --image-project "ubuntu-os-cloud" --boot-disk-size "10" --boot-disk-type "pd-standard" --boot-disk-device-name "mhn-admin"
   ```
   From the output we can notice the external ip address for access to the admin panel
   ```
   NAME         ZONE           MACHINE_TYPE  PREEMPTIBLE  INTERNAL_IP  EXTERNAL_IP    STATUS
   mhn-admin  us-central1-c  f1-micro                   10.128.0.6   35.184.64.139  RUNNING
   ```
* After the set-up was complete I've installed the MHN Admin application
   ```
   $ gcloud compute ssh mhn-admin
  ...
  $ sudo apt-get update
  $ sudo apt-get install git -y
  $ cd /opt
  $ sudo git clone https://github.com/RedolentSun/mhn.git
  $ cd mhn
  $ sudo ./install.sh
  ...
  $ exit
   ```
   During the installation I was asked multiple configuration questions, answers were either no, or default

* Next step was to create a honeypot VM itself, starting with firewall rules
   ```
   $ gcloud beta compute firewall-rules create mhn-allow-honeypot --direction=INGRESS --priority=1000 --network=default --action=ALLOW --rules=all --source-ranges=0.0.0.0/0 --target-tags=mhn-honeypot
   ```
   <img src="firewall.png" width="800">
   and creating the instance:
   ```
   $ gcloud compute instances create "mhn-honeypot-1" --machine-type "f1-micro" --subnet "default" --maintenance-policy "MIGRATE"  --scopes "https://www.googleapis.com/auth/devstorage.read_only","https://www.googleapis.com/auth/logging.write","https://www.googleapis.com/auth/monitoring.write","https://www.googleapis.com/auth/servicecontrol","https://www.googleapis.com/auth/service.management.readonly","https://www.googleapis.com/auth/trace.append" --tags "mhn-honeypot","http-server" --image "ubuntu-1404-trusty-v20171010" --image-project "ubuntu-os-cloud" --boot-disk-size "10" --boot-disk-type "pd-standard" --boot-disk-device-name "mhn-honeypot-1"
   ```
   In the output we will get internal and externa IP addresses
   ```
   NAME            ZONE           MACHINE_TYPE  PREEMPTIBLE  INTERNAL_IP  EXTERNAL_IP    STATUS
   mhn-honeypot-1  us-central1-c  f1-micro                   10.128.0.3   23.236.55.129  RUNNING
   ```
   
* Install the Honepot application
   Enter the ssh mode for the created honeypot
   ```
   gcloud compute ssh mhn-honeypot-1
   ```
   Go to the admin panel using the ip address that I've noticed earlier ```35.184.64.139 ``` and clicked the Deploy button. From the drop down menu for my first Honeypot I've chose ```Ubuntu-Dionaea``` and copied the following deploy command to the ```mhn-honeypot-1``` ssh:
   ```
   wget "http://35.184.64.139/api/script/?text=true&script_id=2" -O deploy.sh && sudo bash deploy.sh http://35.184.64.139 1VEYNzcb
   ```
   <img src="deploy.png" width="800">
* Attach the honeypot
   To test the attack on the honeypot i've used nmap command and IP address of honeypot-1:
   <img src="nmap.png" width="800">
   As we can see there are 13 open ports on the honeypot and 987 closed ones
   
* To set up different types of honeypots I've repeated the last 3 steps except creating a firewall. I've replaced mhn-honeypot-1 with mhn-honeypot-2, mhn-honeypot-3 etc in the create instance command.
   When we go to the  ```mhn-admin``` admin panel using my ```35.184.64.139 ``` IP address and choose ```Sensors> View sensors``` we can see all deployed honeypots with details and the attack number
   I've creted the following types of Honeypots:
   <img src="sensors.png" width="800">
   

   
   






I've repeted the 4 last actions for other honeypots, where I changed 



2. Issues encountered

3. Summary of the data collected

4. Any unresolved questions raised by the data collected


1. First ordered list item
2. Another item
  * Unordered sub-list.
