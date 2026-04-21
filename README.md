*This project has been created as part
of the 42 curriculum by clouden.*

# Description
I will configure small-scale networks. To do so, it is necessary to understand how
TCP/IP addressing works, including concepts such as the subnet mask and the default gateway.

A “Description” section that clearly presents the project, including its goal and a
brief overview.

# Instructions 
First, download the file attached to the project’s page.
* Then, extract the files into any folder of your choice.
* In this folder, run the run.sh file. This shell script will launch a web server and
open your preferred web browser to the dedicated page.
* This interface should open in your web browser:
* For each level, a non-functioning network diagram is displayed.
* At the top of your window, you will see one or more objectives that you must achieve by
adjusting the available configuration so that the network functions properly. 
* There are two buttons you can use:
    * **[Check again]** to verify whether your configuration is correct.
    * **[Get my config]** to download your configuration whenever you need to; you will
need it when submitting your assignment.


An “Instructions” section containing any relevant information about compilation,
installation, and/or execution.
* The Instructions section must explain how to run the training interface (e.g.,
using run.sh), how to export configurations, and submission requirements.


# Resources
## TCP/IP addressing 

There are IPv4 (192.168.1.1) with four 8-bit groups each represened by decimal numbers (0-255) and each group separated by separated by a dot. This gives a total of 32-bits and 2<sup>32</sup> or 4,294,967,296 possible bit combinations.

And IPv6 (2001:0db8:85a3:0000:0000:8a2e:0370:7334) with six hexadecimals groups separated by colons. This produces 128-bits and 2<sup>128</sup> or 340,282,366,920,938,463,463,374,607,431,768,211,456 possible combinations.

## OSI layers

The IP address is relavent at the Network Layer
of the Open Systems Interconnecion (OSI) Model

| Layers        | Role |
| ------------- |:-------------:|
| Application   | Human-Computer interaction    |
| Presentation  | Ensures data is in usable format  |
| Session       | Maintains connections, ports and sessions |
| Transport     | Transmits data with TCP or UDP |
| Network       | Decides which phyisical path to take  |
| Data Link     | Defines format of data    |
| Physical      | Transmits Raw Bit stream over physical data   |

In this project I am only working with IPv4.

So lets break this down. An IP address has 2 parts: the network part on the left and the local part on the right. The division of these sections is determined by the Network ID and Mask. 

If, for example, we have an IP address 192.168.1.1 and we are given a Mask 255.255.255.0 we can determine all we need to know about this network: Network ID, first available address (usually Gateway), last available address, and Broadcast address.

So how does this work? 

## Subnet Masks

The bit wise & operator when comparing any value to a binary 1, will allow that compared value to pass through. Following this truth table:

|  & | 0 | 1 |
| ------------- |-------------| --|
| 0 |  0 | 0 |
| 1 |  0 | 1 |


$$
\begin{array}{r}
  1111&1111 \ \text{(mask) } \\
\& 1010&1100 \ \text{(addre)} \\
\hline
  1010&1100 \ \text{(result)}
\end{array}
$$

So, it keeps the value of whatever is being compared to binary 1s intact.
When each of the 8 bits is at maximum value, each bit holds a 1, the value is is represented in decimal as 255.
$$
11111111 = 255
$$

So that is why we see that the given Mask is actually 3 groups of 8 bits, 

$$
\begin{array}{r}
255.255.255.0
\end{array}
$$

and each bit of each group is set to 1, which means that it is a (8 X 3) 24-bit Mask.

Everything that is compared to it with an & operator produces itself. 
So our with out example address:


$$
\begin{array}{r}
  255.&255.&255.&0 \ \text{(mask) } \\
\& 192.&168.&1.&1 \ \text{(addre)} \\
\hline
  192.&168.&1.&0 \ \text{(result)}
\end{array}
$$


Guess what? This MASK indicates that 192.168.1.0 is the ID of the network where our address 192.168.1.1 is found. And the fact that our mask was 24-bits long means that you will see the network id followed by that number of bits. So our full network id is 192.168.1.0/24. This will give us the starting address of the network and the mask will determine the full extent of the range within that network.

Since, the mask is used to identify the network id then what the mask does not allow to filter through is the total range of addresses that are within that network.

So we have a zero in the least significant group (on the right end of the address). This means all values from 0 - 255 are valid for this network.

Now the Network ID is not a valid address so our first valid address comes from adding 1 to the least significant group (the 8-bit group on the right end) so 192.168.1.1 is the first valid address. 
The last address of the network is the max value for our 8-bits which is 255. That gives us 192.128.1.255 which is our Broadcast. This is a reseved address that can communicate to all addresses on the network at once, so it is not our last valid address.

The last valid address is the broadcast - 1on the lest significant group which is 192.168.1.254.

We have a number of invalid addresses because some addresses have been reserved by the Internet Assigned Numbers Authority. 
My short hand is to remember anything that starts with
* 10
* 100
* 127
* 169
* 192
* 198
* 203
* 224 and anything north of that

There is a more complete list but we won't go into the weeds this is good enough for our netpractice.
 
We can see that between 1 - 254 are all the valid address for this network. That is great becuase I have exactly 254 computers at home to connect them to this network. 

Well, not all of us have or need to connect 254 devices to the network. Maybe, we could make a smaller network. That is exactly what we can do but first some binary math.

|MASK | no. of bits in group | no. of values/subnet | no. of subnets | range of 1st Subnet | range of 2nd Subnet | etc ...|
|-----|----|------------|-----------------|-----|-----------------------|------|
|0 | 0 | 255 |1| 0-255 | NA | NA |
| 128 | 1 | 128 |2| 0-127 | 128 - 255 | NA |
| 192 | 2 | 64 |4| 0-63 | 64-127| 128 - 191|
| 224 | 3 |32 |8| 0-31 | 32- 63 | 64-95|
|240|4| 16 |16| 0-15 |16-31| 32-47|
|248 | 5| 8 |32| 0-7 | 8-15 | 16-23|
|252 | 6| 4 |64| 0-3 | 4-7|8-11|
|254 | 7| 2 |128| 0-1 | 2-3| 4-5|
|255 | 8 | 1 |255|0|1|2|


## Example
This below shows an address and a mask

|  |   |
| ------------- |-------------|
| MASK | 255 . 255 . 255 . 128 |
| ADDR | 123 . 240 . 240 . 34 |

We can derive the following from ADDR and MASK.

|  |   | |
| ------------- |-------------|-----------|
| SubNetID | 123 . 240 . 240 . 0 | This is typically followed by a number of bits in the MASK like /25 |
| First/Gateway | 123 . 240 . 240 . 1 | The first valid address is usually the Gateway to leave the current network|
| Last |  123 . 240 . 240 . 126 | We can see that valid addresses can range from .1 - .127|
| Broadcast |  123 . 240 . 240 . 127 | The broadcast can communicate with all devices connected here|

## gateways, routers and switches
These hardware devices eacch serve distinct roles.

First and easiest is a switch which simply allows communication between devices. This has no interface and no address. It simply connects two or more devices that are in the same network.

Then we have routers, these are in fact more complex. They have an interface for each network that it serves as a gateway for. It also contains a table where other network ids must be registered.

The way to the registered ID is indicated by the address of the next hop that must be made to get there, with the following syntax: 

$$
destination Address=>address Of Next Hop
$$

Each device that is connected to a given network has a table that shows the router's address as the gateway to leave the local network.

Often the value indicated as leave local network is 
`default` or `0.0.0.0/0`
