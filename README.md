**Step #1 – Networking information**
Start both of your virtual machines and get the IPs of both machines (one should be Kali and the 
other Windows 10). 
For Kali, open the terminal and type: 
**sudo ifconfig**
or: 
**ip a** 
Without the sudo command. 
On the Windows 10 machine type CMD after clicking on the Windows icon bottom left of your 
screen. Then, enter this command: 
**ipconfig**
You will need to copy the IPv4 addresses which will be in a 4 dotted decimal format: e.g. 
192.168.0.0  
Take note of the default gateway address as well which will be easiest to view on the Windows 
machine. (See screenshot below)
<img width="906" height="718" alt="image" src="https://github.com/user-attachments/assets/6cb5c1a7-7a0b-4998-b63f-f0f6ed8e79d2" />
<img width="980" height="716" alt="image" src="https://github.com/user-attachments/assets/90ad986b-6ae9-4477-8ea1-a0fc790f064d" />
**Step #2 – Launching Ettercap** 
In the Kali VM, pull up the terminal and type: 
**sudo ettercap -G** 
This starts the GUI and you can view the newly designed interface below:
<img width="906" height="718" alt="image" src="https://github.com/user-attachments/assets/ef759cf5-9a29-4ce5-976c-49de3e4021ec" />
**Step #3 – Adding Hosts to Ettercap**
Begin by looking at the top left of the application window and click on the magnifying glass icon. 
This will scan for hosts within your network. In our case, we are looking for the IP you copied 
from Step 1, which will be our Windows 10 machine. 
You also need to look for the default gateway address in the host list as well. 
Below you will find 3 screenshots that show each step with the last enabling you to view the 
current host list:
<img width="906" height="718" alt="image" src="https://github.com/user-attachments/assets/f12ea538-96ca-443e-aa2b-ad2fd1decec4" />
<img width="906" height="718" alt="image" src="https://github.com/user-attachments/assets/8a393941-7e03-44d5-83ae-1fad9819cca3" />
<img width="906" height="718" alt="image" src="https://github.com/user-attachments/assets/d1f020ed-4997-4920-82e8-e616b0d77b86" />
**Step #4 – Adding Targets** 
Looking at your current Hosts list, select the default gateway address which in this case is 
192.168.58.2 and click Add to Target 1. 
Next select the IP of your Windows 10 machine (in my case it’s 192.168.58.129) and click Add 
to Target 2.
<img width="906" height="718" alt="image" src="https://github.com/user-attachments/assets/1435658d-9920-4403-8437-f383ec497e08" />
<img width="906" height="718" alt="image" src="https://github.com/user-attachments/assets/3f322e5e-64be-4b2d-91fe-58ca26a3ced7" />
**Step #5 – Starting the Spoofing Attack** 
Now we have 2 targets added that we want to conduct the MitM attack on, poisoning the ARP 
cache of our Windows 10 machine. 
Remember we will be sitting in the middle of the gateway and the target. The default gateway 
(router) will think that the target IP is our MAC address and forward all traffic to our attack 
machine.  
The Windows 10 machine will think that the router or default gateway IP is our MAC address 
and forward all traffic to our Kali attack machine. 
For our attack machine to correctly then forward the traffic to and from both targets, we need to 
enable IP forwarding. This is done by entering the following command via the terminal: 
**sudo sysctl -w net.ipv4.ip_forward=1**
<img width="906" height="718" alt="image" src="https://github.com/user-attachments/assets/5c0a0561-c07b-484b-b82f-18484de418a6" />
**Step #6 – Analyzing Traffic in Wireshark** 
Now, go to your Windows 10 machine and open a browser and go to an HTTP website. In this 
example, I went to both popcorn.com and website.org. (Both HTTP not HTTPS) 
Now we need a way to analyze the traffic on our network to see if the target’s traffic is being sent 
to our machine. There are several ways of doing this. For this tutorial, I used tcpdump to dump 
the traffic. I also used -w flag to write the traffic’s output to a .pcap file that I then analyzed with 
Wireshark. 
Pay close attention to the tcpdump and Wireshark screenshots as they will show that our attack 
machine intercepted the traffic going to and from popcorn.com and website.org.  
http://testphp.vulnweb.com/login.php 
Or in google search you can give  
“any http site for testing” 
In a real attack where the user would unknowingly think that their traffic was secure, we could 
potentially see passwords or other information entered by the user on those sites.  
Lastly, I want to go over the tcpdump command that we need to enter in the terminal to capture 
the traffic. On our Kali machine pull up the terminal and enter the following: 
**sudo tcpdump -i eth0 -A -v port 80** 
<img width="902" height="714" alt="image" src="https://github.com/user-attachments/assets/97ba9670-1f37-42e6-b9bc-078beda6a2d6" />
<img width="906" height="718" alt="image" src="https://github.com/user-attachments/assets/a40f2940-ef54-47ca-a7e8-30f235107c1b" />
As you can see both websites our Windows 10 machine visited, we were able to capture using 
Ettercap from our Kali VM. 
After performing the attack, make sure to stop the MitM attack by going to the stop icon shown 
above. (Next to the earth icon) 
Then in the top left of the application window, you will want to press the square icon to stop the 
unified sniffing.  
<img width="906" height="718" alt="image" src="https://github.com/user-attachments/assets/96e991a3-7ba4-4950-928e-bba287395c67" />






