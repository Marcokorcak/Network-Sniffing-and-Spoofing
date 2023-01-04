# Network-Sniffing and Spoofing Lab Description

Packet sniffing and spoofing are two important concepts in network security; they are two major threats
in network communication. Being able to understand these two threats is essential for understanding security measures in networking. There are many packet sniffing and spoofing tools, such as Wireshark,
Tcpdump, Netwox, Scapy, etc. Some of these tools are widely used by security experts, as well as by
attackers. Being able to use these tools is important for students, but what is more important for students in
a network security course is to understand how these tools work, i.e., how packet sniffing and spoofing are
implemented in software.
The objective of this lab is two-fold: learning to use the tools and understanding the technologies underlying these tools. For the second object, students will write simple sniffer and spoofing programs, and gain
an in-depth understanding of the technical aspects of these programs. This lab covers the following topics:

* How the sniffing and spoofing work
* Packet sniffing using the pcap library and Scapy
* Packet spoofing using raw socket and Scapy
* Manipulating packets using Scapy


# Lab Environment Setup!

<img src= "https://user-images.githubusercontent.com/77298953/210651908-05f23a8d-d696-4c99-9f10-44c7b3d98332.PNG" width=70% height=70%>

For this lab the only setup that was needed was to set up the docker containers. There were three

containers created in this lab which were hostB, hostA, and the seed-attacker container. The hostB

container has an IP of 10.9.0.6 while hostA has an IP address if 10.9.0.5 and the attacker container has

an IP address of 10.9.0.1. I used the commands “docker-compose build” and then the “docker-compose

up” commands to build and start the containers. Once this was all done I was able to start the lab.

</br>

# Task 1.1A Sniffing Packets
<img src= "https://user-images.githubusercontent.com/77298953/210651942-7718b574-d8de-4c22-bf7b-8100798050c5.PNG" width=70% height=70%>

The image above shows how the interface id was gotten using the “docker network ls”

command and the “ifconfig” command

</br>

<img src= "https://user-images.githubusercontent.com/77298953/210651979-4e743eb5-74d5-4729-9875-2f03e2d0096a.PNG" width=70% height=70%>

The image above shows the code that was entered in the sniffer.py file

</br>

<img src= "https://user-images.githubusercontent.com/77298953/210652023-fe589f68-8f6d-4765-8525-e2b6b2bf2b3c.PNG" width=70% height=70%>

The image above shows pinging the IP 10.9.0.6 in the host A container as root

<img src= "https://user-images.githubusercontent.com/77298953/210652054-0cbf59ad-4088-402d-8c54-9bb49d78d34c.PNG" width=70% height=70%>

The image above shows the results when running the sniffer file not as a root user

</br>

<img src= "https://user-images.githubusercontent.com/77298953/210652089-fd8e444d-dd36-4677-b5e2-2f244c059d19.PNG" width=70% height=70%>

The image above shows the results when running the sniffer.py file as a root user

## Task 1.1A Explanation
In this task, 1.1A, the objective was to create a packet sniffing program and examine

the difference between running this program as a regular user and root user. To start off, since it

is a packet sniffing program, I need to be able to send and capture packets. In order to do this, I

needed to get the iface id which would be targeted in my program. As seen in the first image, I

got the iface id using the “docker network ls” command as well as the “ifconfig” command. Both

of these commands provided me with the iface id, ffe000d674fe, that will be monitored in this

task. Once this was done, I created a sniffer.py file in the lab7 folder using the “touch sniffer.py”

command. This created the file in the volumes folder and then in the file I added the code that

was provided. I then used the command “chmod a+x sniffer.py” to make the file executable and

this was done as a root user using the “sudo su” command. I then went into the Host A container

using “dochsh ffe000d674fe” to get into the container and then used the “ping 10.9.0.6”

command so there would be packets to capture by the sniffer program. I was pinging the Host B

container in order to transmit and receive packets. Since I made the file executable I ran

“./sniffer.py” as a non-root user and got a error message saying “Operation not permitted”. This

can be seen in the image above and then when I switched to a root user the sniffer outputted

information relating to the packets sent by the ping command. The program revealed information

such as the src IP and the destination IP along with information related to the IP, ICMP, Ethernet

and Raw data.

</br>

# Task 1.1B

<img src= "https://user-images.githubusercontent.com/77298953/210652377-2fa239a9-e8e4-4a6a-af63-2e671ed8a6d9.PNG" width=70% height=70%>

The image above shows the sniffer.py file when looking for only icmp packets

<img src= "https://user-images.githubusercontent.com/77298953/210652432-7568d53d-a568-4345-960b-dc07d7d7f5c9.PNG" width=70% height=70%>

The image above shows the results when running the sniffer.py file as a root user and
looking for icmp packets

<img src= "https://user-images.githubusercontent.com/77298953/210652474-7da9160f-c190-4a86-bd08-4eeb0e5dcbe1.PNG" width=70% height=70%>

The image above shows the sniffer.py file when looking for only TCP packets from the IP
address of 10.9.0.1 and a destination port of 23

<img src= "https://user-images.githubusercontent.com/77298953/210652530-06c7f59a-eabf-4369-9fe1-9dad2517842b.PNG" width=70% height=70%>

The image above shows the results when running the sniffer.py file as a root user and
looking for TPC packets coming from the IP address of 10.9.0.1 and with a destination port of 23

<img src= "https://user-images.githubusercontent.com/77298953/210652580-df393376-2701-4fbf-b23d-a65c9c7d5b38.PNG" width=70% height=70%>

The image above shows the sniffer.py file when looking for packets through the subnet
128.230.0.0/16

<img src= "https://user-images.githubusercontent.com/77298953/210652973-8ef3f53c-f421-4d82-85c1-e605543e6c92.PNG" width=70% height=70%>

The image above shows the results when running the sniffer.py file as a root user and
looking for packets going to the subnet 128.230.0.0

</br>

## Task 1.1B  Explanation
In this task the objective was to apply filters to the sniffing program to filter out

specific packets being captured. The first task was to capture only ICMP packets and this was

done by setting a filter using the filter=‘icmp’ code. I logged into the Host A container and

pinged Host B in order to capture packets. When running the executable sniffer.py file, the

program outputs information relating only to ICMP packets. The first output image shows that

the protocol used is ICMP as well as the destination IP of 10.9.0.06 and the source IP of 10.9.0.5.

The second filter set was to capture only TCP packets from a particular IP with a destination port

number of 23. In order for this to work correctly I logged into the telnet protocol using the

command “telnet 10.0.9.5” which was Host A. Telnet was used because TCP is a network

protocol that allows a user on one computer to log into another computer that is part of the same

network and goes through port 23. Port 23 is the Telnet Protocol for unencrypted text

communications which was why telnet was used since port 23 was the port we were targeting. I

changed the filter to ‘tcp && src host 10.9.0.1 && dst port 23’ in order to properly capture the

packets. Once I logged in, the sniffer program output information relating to the packets. The

image above shows that the port for TCP was telent and the protocol for IP was TCP. Moreover,

the src IP we specified was 10.9.0.1 and the destination port was 23 so the data shown meets all

of the criteria. Lastly, the third task was to capture packets coming from the subnet

128.230.0.0/16. I first set the filter in the code to “filter=net 128.230.0.0/16” and used the iface

br- ffe000d674fe. I then pinged 128.230.0.0 while the sniffer program was running and it output

information relating to the packets being transferred. This showed the src IP as 10.9.05 and the

destination IP of 128.230.0.0. The modification of the sniffer program allowed me to set filters

and see packets only matching the requirements set by the filters.

</br>


# Task 1.2

<img src= "https://user-images.githubusercontent.com/77298953/210653372-0e2bf06e-9cb2-414e-93d5-f34d9131e43e.PNG" width=70% height=70%>

The image above shows the sniffer.py file and the code that was used to spoof ICMP
packets

<img src= "https://user-images.githubusercontent.com/77298953/210653415-f9e9d6c2-ffd2-4b04-b817-b621b598441b.PNG" width=70% height=70%>

The image above shows the wireshark user interface along with host 1.2.3.4 selected and
enp0s2 selected

<img src= "https://user-images.githubusercontent.com/77298953/210653513-592f9f64-e515-461f-acef-ac4d19be573b.PNG" width=70% height=70%>

The image above shows the output of the ICMP packet captured by wireshark

<img src= "https://user-images.githubusercontent.com/77298953/210653551-649b7ec0-cd28-42b3-8980-23c30ac71db3.PNG" width=70% height=70%>

The image above shows the data output by the sniffer program


## Task 1.2 Explanation
In this task, the objective was to spoof IP packets with an arbitrary source IP address. I

would spoof ICMP echo request packets and send them to another VM on the network. I used

wireshark to observe whether the request was accepted and if it was then output would be shown

on the sniffer program. First, I edited the sniffer.py file and added the iface used to the send(p)

part of the code. I edited the code to send(p, iface= ‘br- ffe000d674fe’) and changed the

destination IP to 1.2.3.4. Once this was done, I loaded up the wireshark application and entered

“host 1.2.3.4” since that is the spoofed IP. When I ran the sniffer program, the wireshark

application showed that there were packets being received on the en0s3 interface. When

examining the interface, it was clear that it was my spoofed packets. Looking closer the image

above shows that the destination IP was 1.2.3.4 and the source IP was 10.0.2.4 which was picked

up by the sniffer program.

</br>

# Task 1.3

<img src= "https://user-images.githubusercontent.com/77298953/210654036-b2cb9078-6b56-43ce-b1da-2c501a8ea9f0.PNG" width=70% height=70%>

The image above shows the code that was added to the sniffer.py file

<img src= "https://user-images.githubusercontent.com/77298953/210654077-e667d492-6512-4c6f-810f-2750efd70984.PNG" width=70% height=70%>

The image above shows the IP address of the last router it passed through

<img src= "https://user-images.githubusercontent.com/77298953/210654141-7ce8765c-18b5-450a-8d80-8f484227841b.PNG" width=70% height=70%>

The image above shows the IP address of the last router it passed through when 2 routers
were specified

<img src= "https://user-images.githubusercontent.com/77298953/210654200-4c195e69-79a1-40a3-a3f8-a56d97f7b18a.PNG" width=70% height=70%>

The image above shows the IP address of the last router it passed through when 3 routers
were specified

<img src= "https://user-images.githubusercontent.com/77298953/210654260-f16aabc8-6d44-4690-8f4b-3160d7a90c0b.PNG" width=70% height=70%>

The image above shows the IP address of the last router it passed through when 4 routers
were specified

<img src= "https://user-images.githubusercontent.com/77298953/210654338-0c575c39-f1e0-4005-9df7-9c7d8ae1afb2.PNG" width=70% height=70%>

The image above shows the IP addresses of the last router it passed through when 6 - 10
routers were specified

</br>

## Task 1.3 Explanation
The objective of this task was to traceroute a packet. The code I wrote in the sniffer.py file

specifies that the destination IP is 8.8.4.4 and the time to live of the packet is set to argv[1]. This means

that when running the sniffer.py executable file I can add a number after the command to state how

many routers the packet should pass through, indicating the packet’s time to live. Along with being able

to write how many routers the packet would pass through it would also print out the IP address of the

router it reached so I was able to verify based on the IP address on how many packets it took to reach

the final destination. As seen in the images above, it took 10 routers for the packet to reach its final

destination, 8.8.4.4. Each router that the packet went through, its IP address was printed out. This was

done manually so after each time the program ran, I increased the number of routers the packet would

travel through. Once I reached 10 routers, the packet made it to the destination IP address specified.

</br>

# Task 1.4
<img src= "https://user-images.githubusercontent.com/77298953/210654931-853504a9-364c-4470-b570-1bb536a28a70.PNG" width=70% height=70%>

The image above shows the code that was in sniffer.py when using the IP address 1.2.3.4

<img src= "https://user-images.githubusercontent.com/77298953/210654975-4d93ee33-5de8-473c-aa5d-2fb92f88bb62.PNG" width=70% height=70%>

The image above shows the original and spoofed packets captured by the sniffer.py file for IP address
1.2.3.4

<img src= "https://user-images.githubusercontent.com/77298953/210655031-d7dcd021-da09-4f48-9d54-dab21a82a72e.PNG" width=70% height=70%>

The image above shows the code that was in sniffer.py when using the IP address 10.9.0.99

<img src= "https://user-images.githubusercontent.com/77298953/210655097-0f8a6123-8714-4992-847d-e69f4365bbb3.PNG" width=70% height=70%>

The image above shows the original and spoofed packets captured by the sniffer.py file for IP address
10.9.0.99

<img src= "https://user-images.githubusercontent.com/77298953/210655136-5883df3d-0a3e-499a-be13-ee72907f3e29.PNG" width=70% height=70%>

The image above shows the response when 10.9.0.99 was pinged

<img src= "https://user-images.githubusercontent.com/77298953/210655186-21e61ce7-eb26-4b8b-96f1-951d1f390e41.PNG" width=70% height=70%>

The image above shows the code that was in sniffer.py when using the IP address 8.8.8.8

<img src= "https://user-images.githubusercontent.com/77298953/210655225-f834ec0b-0e87-4fde-b194-bb6f3c9edcc1.PNG" width=70% height=70%>

The image above shows the original and spoofed packets captured by the sniffer.py file for IP address
8.8.8.8

</br>

## Task 1.4 Explanation

The objective of this task was to combine what I learned on sniffing and spoofing packets. In

order to complete this I needed two machines on the same LAN so I used the VM and the user

container. From the container I will ping the given IP address and this will allow my sniffer to print out

the data received to verify if the packets were received and spoofed. To start this, I first implemented a

function called spoof\_pkt that takes in a packet in the parameter. I wrote code that prints the source

and destination IP of the original packets and then the source and destination IP’s of the spoofed

packets. In the first task I set the filter ‘icmp and host 1.2.3.4’. For the packet sniffing I used the code

‘br- ffe000d674fe’ since that is what we wanted to target and monitor. I first pinged 1.2.3.4 from

the Host A container and then executed the sniffer.py file which captured the original and

spoofed packets. The image above shows that for the original packet the source IP address was

10.9.0.5 and the destination IP was 1.2.3.4 and for the spoofed packet the source IP was 1.2.3.4

and the destination IP was 10.9.0.5. For the next task I used the IP address 10.9.0.99 and put that

in my code. When running the executable sniffer.py file nothing was output and that was because

when pinging the IP address from the Host A container, it gave the error “Destination Host

Unreachable”. Since the IP address was not reached, no packets were transferred so no packets

could be picked up. Lastly, I pinged the IP address 8.8.8.8 which the sniffer.py executable picked

up the original packets as well as the spoofed packets. The original packet had a source IP of

10.9.0.5 and a destination IP of 8.8.8.8 while the spoofed packet had a source IP of 8.8.8.8 and

the destination IP was 10.9.0.5. These results were unique because the IP addresses 1.2.3.4 and

8.8.8.8 were both IP addresses on the Internet and provided results. The IP address 10.9.0.99 was

a non-existing host on the LAN which provided no output.


