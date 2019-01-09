# Demonstration of broken routing mesh for docker swarm over IPv6

I have reproduced this problem with docker-ce 18.09.

## Description

When nodes in docker swarm communicate over IPv6, it is still possible to deploy IPv4 services. However, the service is visible only on the nodes where the corresponding docker container runs. In other words, routing mesh does not work.

## Prerequisites

A computer with [vagrant](https://www.vagrantup.com/), [vagrant provider for libvirt](https://github.com/vagrant-libvirt/vagrant-libvirt), and [ansible](https://www.ansible.com/); powerful enough to run two Centos 7 boxes at the same time. IPv6 support in underlying OS.

## Problem reproduction

1. Clone this repository and run `vagrant up --provider=libvirt`.
        * Vagrant will start two boxes: "swarmmanager" and "swarmworker"
        * IPv6 connectivity will be set up between those
        * Docker swarm will be created with one manager (at "swarmmanager") and one worker ("swarmworker"), using IPv6 for master-worker communication
        * One copy of Nginx container will be deployed as a service
        * Two curl command lines will be output by ansible: one to access Nginx service through manager node, and one to access Nginx service through worker node.
2. Run suggested curl commands and make sure that only one succeeds.

## Playing with setup

Use `vagrant ssh swarmmanager` and `vagrant ssh swarmworker` to log in to two boxes and check things (`sudo` is passwordless).

## Cleaning up

`vagrant destroy` from the same directory

## Changing the setup

You can change IPv4 and IPv6 addresses of the boxes at top of `Vagrantfile`; run `vagrant destroy` and `vagrant up --provider=libvirt` afterwards.



