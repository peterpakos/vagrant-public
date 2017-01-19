# WANdisco Vagrant boxes
The boxes can be found at [atlas.hashicorp.com](https://atlas.hashicorp.com/boxes/search?provider=virtualbox&q=wandisco).

## Setup
The framework consists of a few basic BASH scripts for building, packaging and testing boxes.
Each box's Vagrantfile and OS upgrade script reside in a separate directory which name corresponds
to official name of the box after the ```wandisco/``` bit (e.g. wandisco/centos-7-64). The ```build``` and ```
test``` scripts can be found in the ```tools``` directory. Each box's directory contains symbolic links to those scripts.
They are run locally on the host machine while ````upgrade_os```` and ```upgrade_vbguest``` are executed on the guest
machine.

## VirtualBox Guest Additions
The script for installing/upgrading VirtualBox Guest Additions (```upgrade_vbguest```) can be found in the ```tools```
directory and is shared between all boxes.

## Building and packaging boxes
The process of building a box consists of the following steps:
- spin up a base box defined in Vagrantfile
- run upgrade_os script in the guest machine
- run upgrade_vbguest script in the guest machine
- package the guest machine into a box (resulting file: ```package.box```)

### Example
In order to update the CentOS 7 box simply edit the required config files and run the ```build``` script:
```
$ cd centos-7-64
$ vim upgrade_os
$ vim ../tools/upgrade_vbguest
$ ./build
```
If all steps complete successfully you will find a new file called ```package.box``` in the box's directory.

## Testing boxes
When a new box is built it is always a good idea to check that it works as expected before sharing it with the rest of
the world. In order to do this, simply run the following command:
```
$ ./test
```
This will spin up a Vagrant environment using the newly created box. When you exit the VM, the Vagrant environment will
be automatically destroyed and the box's directory cleaned up. If you are happy with the box you can then upload and
publish the resulting ```package.box``` file to the [atlas.hashicorp.com](https://atlas.hashicorp.com/) web site.