# X408_L4


## Demo 1

If we have vagrant installed, the easy way to get started is to ask vagrant to create a vagrant file for you.  For this all you need to do is point it at box image.  We can find many boxes on vagrantup.com.
https://app.vagrantup.com/hashicorp/boxes/bionic64

With vagrant init, vagrant will reachout and download the boxfile and created a base vagrant file in your local directoy.

```
vagrant init hashicorp/bionic64

```

Ok so now we have a vagrant file and a box... what now?? Lets boot it.

```
vagrant up
```

Now we have a running VM... Lets access it

```
vagrant ssh
```

Lets conserve resources on our host machine. Lets turn this machine off.

```
vagrant halt
```
or
``
vagrant suspend
```
Suspending the virtual machine will stop it and save its current running state.
Halting the virtual machine will gracefully shut down the guest operating system and power down the guest machine.



We are done with the machine for good, or atleast a while.  Lets clean up.
```
vagrant destroy
```

We can even manage the boxes that we have downloaded.
```
vagrant box list
vagrant box remove hashicorp/bionic64

```

## Demo 2

Lets start with our last vagrant file...
Take a look at bootstrap.sh & index.html

Update the vagrant file
```
config.vm.provision :shell, path: "bootstrap.sh"
```

Lets poke around

Add the index.html in the vagrant file

I want to hit this super awesome website from my host how do we do that???
```
config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

```


Is there a less janky way of sharing....???

```
mkdir -p  ../demo_2_data/html
cp html/index.html ../demo_2_data/html/
```
Lets update the message

Update the vagrant file... & the bootstrap file....

```
config.vm.synced_folder "../demo_2_data", "/var/www/"
config.vm.synced_folder ".", "/vagrant", disabled: true
```

## Demo 3

We have two VMs... I can talk to them both.. What if I want them to be able to talk with each other
Can we make VM1 hit VM2?

So for VM1 we just need to enable private network...
```
config.vm.network "private_network", ip: "192.168.56.101"

```

For VM2 ..
We need to disable port forwarding
Enable private network
cleanup the bootscript

## Random stuff ...

vagrant global-status

```
config.vm.hostname = "mydevmachine"
config.vm.define :"mydevmachine" do |t|
end

config.vm.provider :virtualbox do |vb|
vb.name = "mydevmachine"
done



```
