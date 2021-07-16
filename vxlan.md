# vxlan 

Linux bridge vxlan discovers other vxlan bridges using multicast UDP messages, distributed using IGMP.

<img src="https://labs.alta3.com/courses/sd-wan/images/vxlan/Slide1.PNG" alt="vxlan" width="50%" >"

0. Add namespaces

    `sudo ip netns add peach`  
    `sudo ip netns add bowser`  

0. Create Bridges

    `sudo ip link add br-vxlan10 type bridge`  
    `sudo ip link add br-vxlan20 type bridge`
    
    
2. sudo ovs-vsctl add-port br-vxlan10 peach -- set interface peach type=internal

0. sudo ovs-vsctl add-port br-vxlan20 bowser -- set interface bowser type=internal

0. Create a vxlan interface

    `sudo ip link add vxlan10 type vxlan id 10 group 239.1.1.1 dstport 0 dev ens3`
    
0. Create a linux bridge    



0. Attach the vxlan interface to the linux bridge

    `sudo ip link set vxlan10 master br-vxlan10`
    
0. Bring up the interfaces
 
    `sudo ip link set vxlan10 up`
    
    `sudo ip link set br-vxlan10 up`
    

0. **Now let's create a target vxlan bridge**

0. Create a vxlan interface

    `sudo ip link add vxlan20 type vxlan id 20 group 239.1.1.1 dstport 0 dev ens3`

0. Create a brdige

    `sudo ip link add br-vxlan20 type bridge`
    
0. Attach the vxlan interface to the linux bridge

    `sudo ip link set vxlan20 master br-vxlan20`

0. Bring up the vxlan interfaces
    
    `sudo ip link set vxlan20 up`
    
    `sudo ip link set br-vxlan20 up`
