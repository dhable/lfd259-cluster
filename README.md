# lfd259-local-cluster

Automated local kubernetes cluster to work with LFD-259 labs.

## Prerequisite

This requires VirtualBox and Vagrant installed locally before continuing.

## Starting a Cluster

Run `vagrant up` and wait until finished. This will start a single control plane with 2 worker nodes. You can then run `vagrant ssh ldf259-control-plane` to SSH into the control plane node to run kubernetes commands.

## Cleaning Up

Just run `vagrant down` (or `vagant down --force`) to turn off the virtual machines and clean up the image artifacts.
