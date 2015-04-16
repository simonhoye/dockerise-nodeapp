# dockerise-nodeapp
Playing around with Vagrant, CoreOS and Docker

1. Install dependencies:
  * Virtual Box: https://www.virtualbox.org/wiki/Downloads
  * Vagrant: https://www.vagrantup.com/downloads.html
  * FleetCTL: https://github.com/coreos/fleet

  To install fleet using Homebrew:
  ```
  $ brew update
  $ brew install fleetctl
  ```

2. Clone CoreOS’s Vagrantfile repository:
  ```
  $ git clone https://github.com/coreos/coreos-vagrant/
  $ cd coreos-vagrant
  ```

3. Generate discovery token and update user-data:
  ```
  $ DISCOVERY_TOKEN=`curl -s https://discovery.etcd.io/new` && perl -p -e "s@#discovery: https://discovery.etcd.io/<token>@discovery: $DISCOVERY_TOKEN@g" user-data.sample > user-data
  ```
  
4. Set number of instances
  ```
  $ export NUM_INSTANCES=1
  ```
  
5. Start up vagrant box
  ```
  $ vagrant up
  ```
  
6. Set up FleetCTL to speak to vagrant via SSH
  ```
  $ export FLEETCTL_TUNNEL=127.0.0.1:2222
  $ ssh-add ~/.vagrant.d/insecure_private_key
  ```
  
7. Load unit file and start up
  ```
  $ fleetctl submit dockerise-nodeapp.service
  $ fleetctl start dockerise-nodeapp.service
  ```
  
  You can watch the logs to see when it's finsihed downloading everything by running:
  ```
  $ fleetctl journal -f dockerise-nodeapp.service
  ```
  
8. Application should now be viewable at http://172.17.8.101:3000

