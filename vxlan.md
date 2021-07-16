# vxlan 

Linux bridge vxlan discovers other vxlan bridges using multicast UDP messages, distributed using IGMP.

<img src="https://labs.alta3.com/courses/sd-wan/images/vxlan/Slide1.PNG" alt="vxlan" width="50%" >"

0. Add namespaces

    `sudo ip netns add peach`  
    `sudo ip netns add bowser`  

0. Create Bridges

    `sudo ip link add br-vxlan10 type bridge`  
    `sudo ip link add br-vxlan20 type bridge`

0. Create the veths

    `sudo ip link add net2peach  type veth peer name peach2net`  
    `sudo ip link add net2bowser type veth peer name bowser2net`  

0. Create a vxlan interface

    `sudo ip link add vxlan10 type vxlan id 10 group 239.1.1.1 dstport 0 dev ens3`  
    `sudo ip link add vxlan20 type vxlan id 20 group 239.1.1.1 dstport 0 dev ens3`
 
0. Attach the vxlan interface to the linux bridge

    `sudo ip link set vxlan10 master br-vxlan10`  
    `sudo ip link set vxlan20 master br-vxlan20`   

0. Bring up the interfaces
 
    `sudo ip link set vxlan10 up`  
    `sudo ip link set vxlan20 up`  
    `sudo ip link set br-vxlan10 up`  
    `sudo ip link set br-vxlan20 up`  
    `sudo ip netns peach  exec ip link set dev peach2net up`  
    `sudo ip netns bowser exec ip link set dev bowser2net up`  
    `sudo ip netns peach  exec ip link set dev lo up`  
    `sudo ip netns bowser exec ip link set dev lo up`      

0.  Plug in the veths

    `sudo ip link set bowser2net netns bowser`  
    `sudo ip link set peach2net  netns peach`

0. Add IP addresses to veths

   `sudo ip netns exec peach  ip a add 10.64.2.2/24 dev peach2net`  
   `sudo ip netns exec bowser ip a add 10.64.2.3/24 dev bowser2net` 
   
0. Set the MTU
 
    `sudo ip netns exec peach  ip link set dev peach2net mtu 1450`
    `sudo ip netns exec bowser ip link set dev bowser2net mtu 1450`

0. Check out what has been installed

    `ip link list`

