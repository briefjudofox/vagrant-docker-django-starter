# vagrant-docker-django-starter

Notes on setting up a boilerplate vagrant, docker, django, postgres project using [cookiecutter-django](https://github.com/pydanny/cookiecutter-django)

# Add docker ready vagrant box (optional)

`vagrant box add williamyeh/ubuntu-trusty64-docker`
([Thanks William Yeh](https://github.com/William-Yeh))

## Create your project dir on the host machine
`mkdir whatever; cd whatever`

## Init docker ready vagrant box
`vagrant init williamyeh/ubuntu-trusty64-docker`

## Bring it up
`vagrant up`

## SSH to VM
`vagrant ssh`

## Navigate to the synced folder on the VM (i.e. /vagrant)
`cd /vagrant`

## Update
`sudo apt-get update`

## Install pip
`sudo apt-get install python3-pip`

## Install Cookie Cutter
`sudo pip3 install "cookiecutter>=1.4.0"`

## Make a dev dir in the synced folder
`mkdir dev; cd dev`

## Run cookiecutter with django template
`cookiecutter https://github.com/pydanny/cookiecutter-django`

## Build the stack
`docker-compose -f dev.yml build`

## Create env var so you don't have to run docker-compose -f dev.yml up (Consider adding to .bashrc)
`export COMPOSE_FILE=dev.yml`

## Fire up the containers
`docker-compose up -d`

## Check that they're running
`docker ps`

## Install Postgres Client
```shell
sudo apt-get install software-properties-common python-software-properties
sudo add-apt-repository "deb http://apt.postgresql.org/pub/repos/apt/ trusty-pgdg main"
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
sudo apt-get update
sudo apt-get install postgresql-client-9.6
```

## Get shell on postgres docker container and use psql
`docker exec -it <your postgres container id> bash`

### To see all dbs
```shell
psql -U postgres
\l
```

### To see django db where "demoz" is your db name
`psql demoz postgres`

## OR from the vagrant shell connect to the db container remotely
### SEE docker inspect &lt;your postgres container id&gt; to get IP address
`psql -h 172.18.0.2 -p 5432 -U postgres -d demoz`

## Add port forwarding configs to your .Vagrant file and reload vagrant
```shell
config.vm.network "forwarded_port", guest: 80, host: 8080
config.vm.network "forwarded_port", guest: 8000, host: 8000
```

## Add allowable IPs and Hosts to your django settings (SEE .../config/settings/local.py)
```shell
ALLOWED_HOSTS = ['localhost', '127.0.0.1']
INTERNAL_IPS = ['localhost','127.0.0.1', '10.0.2.2', ]
```
