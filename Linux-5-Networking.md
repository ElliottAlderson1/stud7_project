# Lab 05 Introduction to Networking

## In this lab you will cover

* [Accessing your student VM from the Command prompt using SSH](#task-1-accessing-your-student-vm-from-the-command-prompt-using-ssh)
* [Changing the state of a network interface](#task-2-changing-the-state-of-a-network-interface)
* [Adding an secondary IP address to your network interface](#task-3-adding-an-secondary-ip-address-to-your-network-interface)
* [Configuring DNS related files on a Linux system](#task-4-configuring-dns-related-files-on-a-linux-system)

### Resources

* See instructional material

### Task 1 - Access your student VM from the Command prompt using SSH

#### Step 1 - Open the Command Prompt program on Windows

#### Step 2 - Remotely connect to your Student Virtual Machine(VM) using SSH

### Task 2 - Changing the state of a network interface

#### Step 1 - View the available network interfaces on your student VM

* View the IP address of your student VM:

  ip address show
* You should see your wired internet connection on `interface 2:` and the IP address of your student VM on `interface 2:`:

2: ens34: ... ... inet 172.16.##.10/24...

> Your output may be slightly different, but the IP address of your student VM will be on one of the interfaces in the listing.

#### Step 2 - Turn off a network interface

* View the current state of the loopback (`lo`) interface:

  ip address show
* You should see:

  1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN ...

  > Notice that `interface 1` is named `lo` and says `state UNKNOWN`. This actually means that this particular interface is up for the purposes of our example. If you look at `interface 2` you will see the words `state UP`.
* Verify that the `loopback` interface `lo` is up by pinging it:

  ping 127.0.0.1

  > `127.0.0.1` is always the IPv4 address of the `loopback` interface.
* You should see:

```
  64 bytes from 127.0.0.1: icmp_seq=1 ttl=64 time=0.067 ms
  64 bytes from 127.0.0.1: icmp_seq=2 ttl=64 time=0.063 ms
  64 bytes from 127.0.0.1: icmp_seq=3 ttl=64 time=0.059 ms
  64 bytes from 127.0.0.1: icmp_seq=4 ttl=64 time=0.071 ms
```

> The above output verifies that the loopback interface is `UP`.Your time values may be slightly different. By default, the `ping` program in Linux will continue to run until you stop it.

* Stop the `ping` command by holding the `ctrl` button and pressing the `z` key. You could also press the `c` key, but if you accidentally pressed the `c` key again while holding the `ctrl` key after the ping stops, you will exit the SSH session. This would require you to SSH back into your student VM.
* Turn off the `loopback` network interface using sudo permission:

  ```
  sudo ip link set lo down
  ```

  > After you enter `sudo ip`, press the tab key twice to see the optional commands available to you.
* Verify that the `loopback` interface is `down`:

  ```
  ip address show
  ```
* You should see:

1: lo: mtu 65536 qdisc noqueue state DOWN ...

> The `state DOWN` indicates that the `loopback` interface is down or off.

* Verify that the `loopback interface is`down`using the`ping\` command:

  ```
  ping 127.0.0.1
  ```

> The ping command will not return any output because it cannot reach the IP address of the loopback interface `127.0.0.1`.

* Cancel the `ping` to `127.0.0.1` by holding down the `ctrl` key and pressing the `z` key.

#### Step 3 - Turn on a network interface

* Turn on the `loopback` network interface using sudo permissions:

  ```
  sudo ip link set lo up
  ```
* Ping the `loopback` IP address `127.0.0.1` to verify that the interface us `up`:

  ```
  ping 127.0.0.1
  ```

```
      64 bytes from 127.0.0.1: icmp_seq=1 ttl=64 time=0.067 ms  
      64 bytes from 127.0.0.1: icmp_seq=2 ttl=64 time=0.063 ms  
      64 bytes from 127.0.0.1: icmp_seq=3 ttl=64 time=0.059 ms  
      64 bytes from 127.0.0.1: icmp_seq=4 ttl=64 time=0.071 ms  
```

> Your time values may be slightly different.

* Cancel the `ping` to `127.0.0.1` by holding down the `ctrl` key and pressing the `z` key.

### Task 3 - Adding an secondary IP address to your network interface

#### Step 1 - Identify the network interface that has the IP address you SSH into

* List the network interfaces to get the name of the network interface that has the `IP address you use to SSH into your student VM`:

  ```
  ip address show
  ```
* You should see:

2: ens34: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000 link/ether 00:50:56:81:fd:ee brd ff:ff:ff:ff:ff:ff altname enp2s2 inet 172.16.x.10/24 brd 172.16.x.255 scope global noprefixroute ens34 valid_lft forever preferred_lft forever

> In my example, interface `2:` is where the IP of my VM (`172.16.#.10`) is assigned. The name of the interface is `ens34`. We will use this name in a later step. Your interface name may be different, so use your interface name on your student VM.

#### Step 2 - Add an additonal IP address to an existing interface

* Add the IP address `172.200.200.200/24` to your interface using sudo permission:

  ```
  sudo ip address add 172.200.200.200/24 dev <interface name>
  ```

  > After you enter `dev` in the above line, you can press the `tab` key twice, and receive a listing of all of the interfaces on your student VM. You can the enter the specific name of the interface you want to add the additional IP to.
* Verify that the `172.200.200.200/24` was added to your network interface:

  ```
  ip address show
  ```
* You should see:

...  
2: ens34: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000 link/ether 00:50:56:81:fd:ee brd ff:ff:ff:ff:ff:ff altname enp2s2 inet 172.16.x.10/24 brd 172.16.10.255 scope global noprefixroute ens34 valid_lft forever preferred_lft forever inet 172.200.200.200/24 scope global ens34 valid_lft forever preferred_lft forever

> Your interface name, number and primary IP address may be different. Notice that in the interface you have two IPv4 addresses indicated by `inet`.

* Restart the Network Manager:

  sudo systemctl restart NetworkManager
* Ping the new IP address:

  ```
  ping 172.200.200.200
  ```

  > You should see successful pings.
* Cancel the `ping` to `172.200.200.200` by holding down the `ctrl` key and pressing the `z` key.

### Task 4 - Configuring DNS related files on a Linux system

The Domain Name Service (DNS) is the system that automatically translates internet addresses to the numeric machine addresses that computers use. In your student VM there is a file: `/etc/hosts` that acts as a Domain Name Server (DNS):

#### Step 1 - Configure a local DNS entry in the `/etc/hosts` file

* Open the `etc/hosts` file in `nano` using sudo permission:

  ```
  sudo nano /etc/hosts
  ```
* You should see:

  ```
  127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
  ::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
  ```
* Add the following line to a new line below the existing lines in the file:

  ```
  172.200.200.200   mycomputer
  ```
* Save your changes to the `/etc/hosts` file by holding down the `ctrl` key and pressing the `o` key.
* When prompted with: `"File Name to Write: /etc/hosts"`, press `enter`
* Exit `nano` by holding down the `ctrl` key and pressing the `x` key.
* Verify that your entry was saved to the `/etc/hosts` file:

  ```
  cat /etc/hosts
  ```
* You should see:

  ```
  127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4  
  ::1         localhost localhost.localdomain localhost6 localhost6.localdomain6  
  172.200.200.200  mycomputer  
  ```
* Verify that the host file is used as the first DNS by pinging the name `mycomputer`:

  ```
  ping mycomputer
  ```
* You should see:

  ```
  PING mycomputer (172.200.200.200) 56(84) bytes of data.  
  64 bytes from mycomputer (172.200.200.200): icmp_seq=1 ttl=64 time=0.046 ms  
  64 bytes from mycomputer (172.200.200.200): icmp_seq=2 ttl=64 time=0.058 ms  
  64 bytes from mycomputer (172.200.200.200): icmp_seq=3 ttl=64 time=0.058 ms  
  ```

  > Notice that in the output, `mycomputer` is translated to `172.200.200.200`. This is because the ping command performed a lookup of the name `mycomputer` in the local DNS `/etc/hosts` file and found the address 172.200.200.200. This ping command to `mycomputer` only works on your student VM because the `/etc/hosts` file is only accessable from your computer as configured.

#### Step 2 - Add a DNS nameserver to the `/etc/resolv.conf` file

* View the contents of the `/etc/resolv.conf` file:

  ```
  sudo nano /etc/resolv.conf
  ```
* add the below if not already listed:

  ```
  search devops.soroc.mil
  nameserver 10.200.99.30
  nameserver 10.200.99.31
  ```
* Use the `nslookup` command to find the ip address of `google.com`:

  ```
  nslookup google.com
  ```
* You should see:

```
Server:         10.200.99.30
Address:        10.200.99.30#53

Non-authoritative answer:
Name:   google.com
Address: 142.250.105.138
Name:   google.com
Address: 142.250.105.100
Name:   google.com
Address: 142.250.105.139
Name:   google.com
Address: 142.250.105.102
Name:   google.com
Address: 142.250.105.113
Name:   google.com
Address: 142.250.105.101
Name:   google.com
Address: 2607:f8b0:4002:c1b::8a
Name:   google.com
Address: 2607:f8b0:4002:c1b::64
Name:   google.com
Address: 2607:f8b0:4002:c1b::71
Name:   google.com
Address: 2607:f8b0:4002:c1b::65
```

> Notice that the IP address to the right of `Server:` is the first address listed in the `/etc/recolv.conf` file.

* Verify that the IP address listed under google goes to google.com by entering the ip address into your web browser. In the example above, the address is 142.250.105.138, but your `nslookup` may return a different number because Google has multiple IP addresses assigned to it.
* Open the `/etc/resolv.conf` file using `nano` and the sudo permissions:

  ```
  sudo nano /etc/resolv.conf
  ```
* Add the google DNS server `8.8.8.8` above the highest `nameserver` entry in the `/etc/resolv.conf` file:
* Using the example above from my `/etc/resolv.conf` file and adding `8.8.8.8` it looks like:

  ```
  search devops.soroc.mil
  nameserver 8.8.8.8
  nameserver 10.200.99.30
  nameserver 10.200.99.31
  ```

  > Yours may look different, but it is important that you add `nameserver 8.8.8.8` above any other nameserver entries.
* Save your changes to the `/etc/resolv.conf` file by holding down the `ctrl` key and pressing the `o` key.
* When prompted with: `"File Name to Write: /etc/resolv.conf"`, press `enter`.
* Exit `nano` by holding down the `ctrl` key and pressing the `x` key.
* Use the `nslookup` command again to find the ip address of `google.com`:

  ```
  nslookup google.com
  ```
* You should see:

```
Server:         8.8.8.8
Address:        8.8.8.8#53
  
Non-authoritative answer:
Name:   google.com
Address: 142.250.105.138
Name:   google.com
Address: 142.250.105.100
Name:   google.com
Address: 142.250.105.139
Name:   google.com
Address: 142.250.105.102
Name:   google.com
Address: 142.250.105.113
Name:   google.com
Address: 142.250.105.101
Name:   google.com
Address: 2607:f8b0:4002:c1b::8a
Name:   google.com
Address: 2607:f8b0:4002:c1b::64
Name:   google.com
Address: 2607:f8b0:4002:c1b::71
Name:   google.com
Address: 2607:f8b0:4002:c1b::65
```

> Notice that the nslookup command now used the google dns of 8.8.8.8 to find the address and not the other nameserver IP addresses in the /etc/resolv.conf file.

#### Step 3 - Additional useful commands

* View the contents of the `/etc/hostname` file:

cat /etc/hostname

> This is the file that sets the hostname of your student VM.

* View the hostname of your student VM with the following command:

hostname

* The `hostnamectl` command can be used to get more information about your system:

hostnamectl

* You should see:

Static hostname: Student## Icon name: computer-vm Chassis: vm Machine ID: 9c69cfcac1fc4e6da857e8e3ffccb5d6 Boot ID: b970f299a6bd44b3a3f68b78091f11ec Virtualization: vmware Operating System: Rocky Linux 8.8 (Green Obsidian) CPE OS Name: cpe:/o:rocky:rocky:8:GA Kernel: Linux 4.18.0-477.27.1.el8_8.x86_64 Architecture: x86-64

> Your output may be different.

### Task 5 - Lab Cleanup

#### Step 1 - Remove the mycomputer entry in the `/etc/hosts` file

* Open the `/etc/hosts` file using `nano` and the sudo permissions:

  ```
  sudo nano /etc/hosts
  ```
* Delete the line: `172.200.200.200 mycomputer`.
* Save your changes to the `/etc/hosts` file by holding down the `ctrl` key and pressing the `o` key.
* When prompted with: `"File Name to Write: /etc/hosts"`, press `enter`.
* Exit `nano` by holding down the `ctrl` key and pressing the `x` key.
* Verify that your entry was saved to the `/etc/hosts` file:

  ```
  cat /etc/hosts
  ```
* You should see:

  ```
  127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
  ::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
  ```

  > Yours may have more entries, but the line `172.200.200.200 mycomputer` should now be removed.

#### Step 2 - Remove the additional ip address 172.200.200.200 on your interface

* Find the `name` of the network interface that you added the IP address `172.200.200.200` to:

ip address show

* You should see:

2: ens34: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000 link/ether 00:50:56:81:fd:ee brd ff:ff:ff:ff:ff:ff altname enp2s2 inet 172.16.10.25/24 brd 172.16.10.255 scope global noprefixroute ens34 valid_lft forever preferred_lft forever inet 172.200.200.200/24 scope global ens34 valid_lft forever preferred_lft forever

> Your output may be slightly different.

* Remove the IP address `172.200.200.200/24` from the interface using sudo permission:

  ```
  sudo ip address del 172.200.200.200/24 dev ens34
  ```

  > Your interface may be different. Mine was `ens192`. You will put the name of the interface that the 172.200.200.200/24 address is assigned to on your system.
* Verify that the IP address `172.200.200.200/24` was removed from the network interface:

  ```
  ip address show
  ```

  > You should no longer see the IP address in the output.

#### Step 3 - Remove the additional DNS `8.8.8.8` entry

* Open the `/etc/resolv.conf` file using `nano` and the sudo permission:

  ```
  sudo nano /etc/resolv.conf
  ```
* Remove the google DNS server `nameserver 8.8.8.8` entry in the `/etc/resolv.conf` file:
* Save your changes to the `/etc/resolv.conf` file by holding down the `ctrl` key and pressing the `o` key.
* When prompted with: `"File Name to Write: /etc/resolv.conf"`, press `enter`.
* Exit `nano` by holding down the `ctrl` key and pressing the `x` key.
* Verify that your entry was saved to the `/etc/resolv.conf` file:

  ```
  cat /etc/resolv.conf
  ```
* Using the example above from my `/etc/resolv.conf` file it looks like:

  ```
  search devops.soroc.mil
  nameserver 10.200.99.30
  nameserver 10.200.99.31
  ```

  > Yours may look different, but `nameserver 8.8.8.8` should no longer be in the `/etc/resolv.conf` file.