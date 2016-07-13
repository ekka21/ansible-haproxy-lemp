# LEMP Stack + HAProxy infrastructure

- Requires Ansible v2.0.x
- Expects CentOS/RHEL 7 hosts

The goal is to create an immutable and scalable infrastructure(AKA Horizontal Scalability!) with Ansible. The idea is that I want to be able to provision my remote servers(Vagrant/Cloud) from my Mac.

## Infrastructure


- Load Balancer (HaProxy)
- Database (MariaDB)
- App1, App2 (Nginx, PHP7)

I will install and configure a Nginx web server with HAProxy Load Balancer in front, and deploy a PHP web application to web servers.
```
192.168.0.100 lb
192.168.0.110 db
192.168.0.121 app1
192.168.0.122 app2
```



## Vagrant

What I'm trying to do is to spin up multiple virtual machines and can ssh into each boxes with the same ssh-key on my Mac. The trick here is that Vagrant as of v1.7.3 will automatically generate ssh-key pairs for each VMs and insert them into each box. So you'll have to add `config.ssh.insert_key = false` in `Vagrantfile`. After that Vagrant will use `~/.vagrand.d/insecure_private_key` instead. 

Now you should be able to ssh like this `ssh -i ~/.vagrant.d/insecure_private_key vagrant@IP-ADDRESS`. You can add that as an bash alias if you are lazy like me but I like to take it even further because I want to connect to my VMs like this`ssh lb` or `ssh db` or `ssh app1` or `ssh app2`

- First, update `/etc/hosts` in your favorite text editor
```
192.168.0.100 lb
192.168.0.110 db
192.168.0.121 app1
192.168.0.122 app2
```

- Second, update `~/.ssh/config` in your favorite text editor
```
Host lb
	HostName 192.168.0.100
	User vagrant
	IdentityFile ~/.vagrant.d/insecure_private_key

Host db
	HostName 192.168.0.110
	User vagrant
	IdentityFile ~/.vagrant.d/insecure_private_key

Host app1
	HostName 192.168.0.121
	User vagrant
	IdentityFile ~/.vagrant.d/insecure_private_key

Host app2
	HostName 192.168.0.122
	User vagrant
	IdentityFile ~/.vagrant.d/insecure_private_key
```

You are golden!

*The range of ip-addresses are 192.168.[0-255].[0.255]. Don't be like me and try to create a VM and couldn't ssh into. It was because I was trying to create 192.168.0.301 and 192.168.0.302. You're welcome, I just save 10 minutes of your life!.*


# Ansible as a provisoner

First, let's make sure that we can ping our Vms `cd provision && ansible all -m ping && cd -`, you should get something like
```
db | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
lb | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
app1 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
app2 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}```

Good!







