# Several nodes LAN with Vagrant and Virtualbox

Simple set up to create virtual machine nodes with Vagrant to use as a test machine. Since sometimes creating one or two machines on the notebook was not enough, I wanted to use other computers on the LAN for my tests.

## Public certificate 

If you put an `id_rsa.pub` on the directory it will copy to all the machines created. It’s usefull if you are going to use the nodes with ansible.

## Usage

Clone repo and start vagrant:

```
vagrant up
```

By default will create 8 machines with 1GB RAM and 1 CPU each on addresses 192.168.101 to 192.168.108. You should set up your `BRIDGE_IF`, or just probably set to 1 so you get the first interface. If you don’t write, then vagrant will just ask for the network card on creating.

### Interface names on Virtualbox

To list your interface names you can use `VBoxManage list bridgedifs` command, if is not in the PATH, then will be on your Virtualbox install directory. You can get just the names with `vboxif.bat` on windows and `vboxif.sh` on Linux/OsX.

## Environment variables

- `NUM_INSTANCES`: Default to 8 instances
- `NODE_MEM`: Default to 1024 MB
- `NODE_CPUS`: Default to 1 CPU/node
- `BRIDGE_IF`: Name of the bridge adapter to set up the IP address, default to 'Intel(R) Ethernet Connection (2) I218-V'
- `BASE_IP_NET`: Default to 192.168.0
- `BASE_IP_SUBNET`: Subnet, default to 100. (First IP will be `192.168.0.101`, second `192.168.0.102`, ...)
- `BASE_NODE_NAME`: Basename for nodes, default `nodo-`. Firs name will be `nodo-01`, second `nodo-02`, ...

You can set your own scripts to run different nodes without editing `Vagrantfile` just using environment variables. Example

Run 4 machines with 512MB:

- run4machines.sh

```
#!/bin/bash
NUM_INSTANCES=4 NODE_MEM=512 vagrant up
```

- stop4machines.sh
```
#!/bin/bash
NUM_INSTANCES=4 NODE_MEM=512 vagrant halt
```

