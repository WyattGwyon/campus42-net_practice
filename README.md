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
TCP/IP addressing 

There are IPv4 (192.168.1.1) with four 8-bit groups each represened by decimal numbers (0-255) and each group separated by separated by a dot. This gives a total of 32-bits and 2<sup>32</sup> or 4,294,967,296 possible bit combinations.

And IPv6 (2001:0db8:85a3:0000:0000:8a2e:0370:7334) with six hexadecimals groups separated by colons. This produces 128-bits and 2<sup>128</sup> or 340,282,366,920,938,463,463,374,607,431,768,211,456 possible combinations.

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

So that is why we see that the given Mask 255.255.255.0 is actually 3 groups of 8 bits, and each bit is set to 1, which means that it is a 24-bit Mask.

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


Guess what? That means 192.168.1.0 is the ID of the network this address is in. And the fact that our mask was 24-bits long means that you will see the network id followed by that number of bits. So our full network id is 192.168.1.0/24. This will give us the starting address of the network and the mask will determine the full extent of the range within that network.

Since, the mask is uesd to identify the network id then what the mask does not allow to filter through is the total range of addresses that are within that network.

So we have a zero in the least significant group (on the right end of the address). This means all values from 0 - 255 are valid for this network.




Now the Network ID is not a valid address so our first valid address comes from adding 1 to the least significant group (the 8-bit group on the right end) so 192.168.1.1 is the first valid address. 
The last address of the network is the max value for our 8-bits which is 255. That gives us 192.128.1.255 which is our Broadcast. This is a reseved address that can communicate to all addresses on the network at once, so it is not our last valid address.

The last valid address is the broad cast - 1on the lest significant group which is 192.168.1.254.
we can see that between 1 - 254 are all the valid address for this network. That is great becuase i have exactly 254 computers at home to connect them to this network. 

Well, not all of us have or need to connect 254 devices to the network. Maybe, we could make a smaller network. That is exactly what w can do but first some binary math.

0000 0000
$$
\begin{matrix}
1 & 2 & 3 \\
4 & 5 & 6 \\
7 & 8 & 9
\end{matrix}
$$

We are going to start counting but instead of starting on the right with the least significant bit we are going to start on the left with the most significant bit.

1000 0000

that value all the way out there represents a 128 in decimal.
and can be represented in the mask as 128.
so 255.255.255.128
this gives us 

A “Resources” section listing classic references related to the topic (documentation, articles, tutorials, etc.), as well as a description of how AI was used —
specifying for which tasks and which parts of the project.
• The Resources section must explicitly mention the networking concepts studied,
such as TCP/IP addressing, subnet masks, default gateways, routers and
switches, OSI layers, etc.


• Submission details must state that 10 exported configuration files (one per level)
must be placed at the repository root.

