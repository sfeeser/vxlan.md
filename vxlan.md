# vxlan 

Linux bridge vxlan discovers other vxlan bridges using multicast UDP messages, distributed using IGMP.

<img src="https://labs.alta3.com/courses/sd-wan/images/vxlan/Slide1.PNG" alt="vxlan" width="50%" >"


1. Create a vxlan interface

    `ip link add vxlan10 type vxlan id 10 group 239.1.1.1 dstport 0 dev ens3`
    
0. Create a linux bridge    

    `ip link add br-vxlan10 type bridge`

0. Attach the vxlan interface to the linux bridge

    `ip link set vxlan10 master br-vxlan10`
    
0. Bring up the interfaces
 
    `ip link set vxlan10 up`
    
    `ip link set br-vxlan10 up`
    

0. **Now let's create a target vxlan bridge**

0. Create a vxlan interface

    `ip link add vxlan20 type vxlan id 20 group 239.1.1.1 dstport 0 dev ens3`

0. Create a brdige

    `ip link add br-vxlan20 type bridge`
    
0. Attach the vxlan interface to the linux bridge

    `ip link set vxlan20 master br-vxlan20`

0. Bring up the vxlan interfaces
    
    `ip link set vxlan20 up`
    
    `ip link set br-vxlan20 up`
