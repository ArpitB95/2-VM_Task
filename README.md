# 2-VM_Task


## What is monolithic architecture ?
- Monolithic applications consist of a single codebase that interacts with a single database. The application’s client interface, single database, frontend, backend and business logic are incorporated into one codebase. There is only one test and deployment pipeline to maintain.


![mon](https://user-images.githubusercontent.com/110182832/184827812-339ebf9f-60c8-4a85-a15d-5ebbec3284ea.png)


### Two Virtual Machines

<img width="551" alt="new final dia" src="https://user-images.githubusercontent.com/110182832/184918495-92a490b8-1cda-4576-a46b-c06e68b42864.png">





### Vagrant File to create 2 Virtual Machines:
````
Vagrant.configure("2") do |config|
    config.vm.define "app" do |app|  ## this will create first machine named 'app'
      app.vm.box = "ubuntu/bionic64"  # creating a virtual machine ubuntu
      app.vm.network "private_network", ip: "192.168.10.100"  # add private network
      app.vm.synced_folder ".", "/home/vagrant/app" # change it to your home location  ## now let's sync our app folder from localhost and sync it to VM
      app.vm.provision "shell", path: "provision.sh", privileged: false    ## config provision file
                                     # provide path for your provision.sh 
    end
  
    config.vm.define "db" do |db|  ## this will create second machine called 'db'
      db.vm.box = "ubuntu/bionic64"
      db.vm.network "private_network", ip: "192.168.10.150"
      
    end
  end
````



### Provision.sh file to automate the installation tasks:

````  
  ## Automate download of dependencies

## update & upgrade
sudo apt-get update -y
sudo apt-get upgrade -y

## Install nginx
sudo apt-get install nginx -y

sudo systemctl enable ngninx # to enable nginx

sudo systemctl start nginx  # to start nginx

## Download nodejs version 6 or above
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -  # to navigate the web link to download nodejs version 6 or above

## Install nodejs

sudo apt-get install nodejs -y

## Install pm2

sudo npm install pm2 -g
````

## Reverse proxy
- We need to create reverse proxy in VM-1 (app)
- To do that, first run 'vagrant ssh app'
- Inside app VM run 'sudo nano /etc/nginx/sites-available/default'
- Now, you'll land into default file, in that file go to the server_name and under location block add 'proxy_pass http://localhost:3000;'


<img width="368" alt="rever pro" src="https://user-images.githubusercontent.com/110182832/184925072-e983257e-5b8b-4ff4-83a4-24ed078062db.png">

- after editing, save it and exit the file.
- Now, check the syntax error by running 'sudo nginx -t'
- Now go to app/app and run 'npm start'



<img width="712" alt="new today dia" src="https://user-images.githubusercontent.com/110182832/185105510-99e1b8b0-238e-40be-b1dc-0e18d5306d55.png">



<img width="301" alt="final add" src="https://user-images.githubusercontent.com/110182832/185172041-e6581e1f-2452-4cc3-afc1-6a534a2441eb.png">


