# Facebook Cybersecurity Course Project 8: Honeypot

## Objective: Setup a honeypot and provide a working demonstration of its features.

### 1. Overview & Setup | Which Honeypot(s) you deployed

* To deploy honeypots I've used Google Cloud. I've downloaded and installed Google Cloud Platform SDK so I was able to run ```gcloud``` from a command line.

* For the VM i've used Ubuntu 14.04 with following open ports: 80, 3000, 10000. I've created the following firewall rules with command
``` 
$ gcloud beta compute firewall-rules create mhn-allow-admin --direction=INGRESS --priority=1000 --network=default --action=ALLOW --rules=tcp:3000,tcp:10000 --source-ranges=0.0.0.0/0 --target-tags=mhn-admin
```
Next step was to set up admin virtual machine:
```
gcloud compute instances create "mhn-admin" --machine-type "f1-micro" --subnet "default" --maintenance-policy "MIGRATE"  --scopes "https://www.googleapis.com/auth/devstorage.read_only","https://www.googleapis.com/auth/logging.write","https://www.googleapis.com/auth/monitoring.write","https://www.googleapis.com/auth/servicecontrol","https://www.googleapis.com/auth/service.management.readonly","https://www.googleapis.com/auth/trace.append" --tags "mhn-admin","http-server","https-server" --image "ubuntu-1404-trusty-v20171010" --image-project "ubuntu-os-cloud" --boot-disk-size "10" --boot-disk-type "pd-standard" --boot-disk-device-name "mhn-admin"
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
```
During the installation I was asked multiple configuration questions, answers were either no, or default

* Next step was to create a honeypot VM itself





I've repeted the 4 last actions for other honeypots, where I changed 



2. Issues encountered

3. Summary of the data collected

4. Any unresolved questions raised by the data collected


1. First ordered list item
2. Another item
  * Unordered sub-list.
