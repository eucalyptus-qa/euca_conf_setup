
euca_conf_setup
===============

## Description

Configure eucalyptus.conf

## Procedure

1. Go through each component, and if its not WINDOWS or VMWARE
2. Run mod_euca_conf_beta.pl on each component
3. Set java home path and add it to the cloudopts
4. For CentOS,OpenSUSE, Fedora, RHEL: Set paths on NC for Bundle,Check-bucket,and Delete bundle
5. If Debian, set USE_VIRTIO_DISK to 1
6. Setup Euca_conf for each distro as follows
7. UBUNTU: 1) set the priv and public interfaces to eth0 2) Iin the NC set the bridge as the public interface for Managed mode and br0 for SYSTEM, Managed-NoVLAN or static 3) For the CC set the DHCPDAEMON to /usr/sbin/dhcpd3 and DHCPUSER to dhcpd
8. DEBIAN: Do all the same things as step 4 but set the DHCPDAEMON to /usr/sbin/dhcpd and leave the DHCPUSER as root
9. OPENSUSE: Change the bridge on the NC to the PUBINTERFACE if the component is a NC
10. SLES: set the VNET_INTERFACE to br0
11. CENTOS && RHEL: Set teh PUB and PRIV INTERFACES, If ist RHEL6 and its a NC set the VNET_BRIDGE to the PUB_INTERFACE in managed mode or br0 for any other, Set the VIRTIO_DISK VIRTIO_ROOT and VIRTIO_NET flags to 1
12. For all CCs set teh CLOUDIP to the CLC_IP
13. In managed network modes set the NETMASK, DNS_SERVER, and ADDRESSESPERNET
14. Populate the SUBNET and Public IPs from the test config file 2btested.lst
15. In static Mode set the same parameters in steps 13-14 but use predefined values ie SUBNET=192.168.0.0, NETMASK= 255.255.192.0, BROADCAST=192.168.63.255, ROUTER=192.168.7.1, DNS= 192.168.7.1
16. If not system mode then comment out VNET_MODE and add in VNET_MODE for the apporiate mode configured in the test


<hr><hr><hr>


# Eucalyptus Testunit Framework

Eucalyptus Testunit Framework is designed to run a list of test scripts written by Eucalyptus developers.



## How to Set Up Testunit Environment

On **Ubuntu** Linux Distribution,

### 1. UPDATE THE IMAGE

<code>
apt-get -y update
</code>

### 2. BE SURE THAT THE CLOCK IS IN SYNC

<code>
apt-get -y install ntp
</code>

<code>
date
</code>

### 3. INSTALL DEPENDENCIES
<note>
YOUR TESTUNIT **MIGHT NOT** NEED ALL THE PACKAGES BELOW; CHECK THE TESTUNIT DESCRIPTION.
</note>

<code>
apt-get -y install git-core bzr gcc make ruby libopenssl-ruby curl rubygems swig help2man libssl-dev python-dev libright-aws-ruby nfs-common openjdk-6-jdk zip libdigest-hmac-perl libio-pty-perl libnet-ssh-perl euca2ools
</code>

### 4. CLONE test_share DIRECTORY FOR TESTUNIT
<note>
YOUR TESTUNIT **MIGHT NOT** NEED test_share DIRECTORY. CHECK THE TESTUNIT DESCRIPTION.
</note>

<code>
git clone git://github.com/eucalyptus-qa/test_share.git
</code>

### 4.1. CREATE /home/test-server/test_share DIRECTORY AND LINK IT TO THE CLONED test_share

<code>
mkdir -p /home/test-server
</code>

<code>
ln -s ~/test_share/ /home/test-server/.
</code>

### 5. CLONE TESTUNIT OF YOUR CHOICE

<code>
git clone git://github.com/eucalyptus-qa/**testunit_of_your_choice**
</code>

### 6. CHANGE DIRECTORY

<code>
cd ./**testunit_of_your_choice**
</code>

### 7. CREATE 2b_tested.lst FILE in ./input DIRECTORY

<code>
vim ./input/2b_tested.lst
</code>

### 7.1. TEMPLATE OF 2b_tested.lst, SEPARATED BY TAB

<sample>
192.168.51.85	CENTOS	6.3	64	REPO	[CC00 UI CLC SC00 WS]

192.168.51.86	CENTOS	6.3	64	REPO	[NC00]
</sample>

### 7.2. BE SURE THAT YOUR MACHINE's id_rsa.pub KEY IS INCLUDED THE CLC's authorized_keys LIST

ON **YOUR TEST MACHINE**:

<code>
cat ~/.ssh/id_rsa.pub
</code>

ON **CLC MACHINE**:

<code>
vim ~/.ssh/authorized_keys
</code>

### 8. RUN THE TEST

<code>
./run_test.pl **testunit_of_your_choice.conf**
</code>


## How to Examine the Test Result

### 1. GO TO THE artifacts DIRECTORY

<code>
cd ./artifacts
</code>

### 2. CHECK OUT THE RESULT FILES

<code>
ls -l
</code>


## How to Rerun the Testunit

### 1. CLEAN UP THE ARTIFACTS

<code>
./cleanup_test.pl
</code>

### 2. RERUN THE TEST

<code>
./run_test.pl **testunit_of_your_choice.conf**
</code>


