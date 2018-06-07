# couchbaseUpgradeAnsible

## Requirements
  1. Ansible
  2. Travelsampleloadgen application (built)
  3. Nodes to perform Upgrade on
  
## Changes in hosts file
Ansible takes in a hosts file that specifies the nodes it has to perform actions on. The hosts files are present in ansible folder of the project.
You can modify the existing host files or create a new one by copying one of the host file as template.
  1. The hosts file contains the ip addresses of the hosts that has to be included in the cluster. 
  2. Each line under [centos] section specifies a node and the services the node must have in the cluster
  3. [centosupgradenode] section must mandatorily have a node in it. This node will be used to initiate the swap rebalance during the upgrade process
  4. [all:vars] contains the required information for ssh to the machine. "ansible_user" is the username, "ansible_ssh_pass" is the password for the user.
  
  ## Changes to the index.yml
  In the centos folder, the actions to be performed during the test is specified. 
  One of the file is index.yml that contains the index definitions to be created during the test. This file needs to be changed to reflect the correct nodes where the index will be created.
  For each index node in the cluster, a corresponding index definitions has to be present in the index.yml file. There are 12 index definitions per node.
  
  ## Changes to testConstants.ini
  Various input to the tests can be given via testConstants.ini file under centos folder. It has two sections, testConstants and loadgenerator.
  
  ### testConstants
  initial_version: Initial version of the couchbase server (ex: 4.6.3)
  initial_build_no: Initial build number of the couchbase server (ex: 4084)
  initial_flavor: Initial flavour of the couchbase server (ex: watson, spock, vulcan)
  upgrade_version: Couchbase server version that the servers should be upgraded to (ex: 5.0.0)
  upgrade_build_no: Couchbase server build number that the servers should be upgraded to (ex: 3358)
  upgrade_flavor= Couchbase server flavor that the servers should be upgraded to (ex: watson, spock, vulcan)
  node_init: Initial number of nodes to be included in the cluster (ex: 6)
  
  ### loadgenerator
  initial_loadgen: Location for the loadgen properties file to run the initial load.
  loadgen_jar_location: Location where the loadgen is built (Ex: /root/applicationUpgrade/travelsampleloadgen/target/)
  loadgen_jar=TravelSampleLoadGenerator.jar (Initial loadgen jar)
  updated_loadgen_jar=TravelSampleLoadGenerator2.0.jar (Updated loadgen jar)
  loadgen_sample_data=TravelSampleData.json
  update_loadgen: Location for the loadgen properties file to run the update load.
  updated_loadgen: Location for the loadgen properties file to run the update load on upgraded loadgen jar.
  
  ## Running the test
  1. To run the test, first build travelsampleloadgen on the machine. Also build the upgraded loadgen with latest sdk
  2. Create initial loadgen properties file, updates loadgen properties file and updates loadgen properties file for upgraded loadgen.
  3. Change the testConstants.ini files as mentioned above
  4. Change index.yml file as mentioned above
  5. Create a hosts file including all the nodes information.
  6. Run the application by running the following command from the root folder of this project.
     ansible-playbook -i <host files> centos/main.yml
     
  
