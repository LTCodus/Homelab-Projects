# OpenVPN Setup

I found myself needing a secure way to access my homelab services remotely, and after doing some digging I found out that my OPNsense router has a plugin to allow it to run an openVPN server straight in it, eliminating the need to run it in a VM or anything else. Super Easy!

## Getting Started

So this took a lot less effort than I thought it would. I initially started off following [this guide](https://homenetworkguy.com/how-to/configure-openvpn-opnsense/), and it made this whole process a lot easier for me considering it was my first time, so thanks to them for this great guide!

## Setting up the Certificates and users

Following the instructions in the guide I linked, I was able to make certificates on my router to allow the openVPN service to work, as well as authenticate users on my router. An added benefit to this is I can set up 2FA for my users to make connecting a lot more secure than a password alone. 

First, I had to make an internal Certificate Authority for openVPN. This is pretty easy if I go through the System>Trust>Authorities section in OPNsense. From here I created openVPN CA, as well as an internal certificate to allow the openVPN service to run correctly. 

Next, I created an additional user in OPNsense for myself to avoid having to log in with the admin or root account. This was pretty straight forward, System>Access>Users, and then fill out the form to make an account. Here I was able to set up 2fa to make sure this account is extra secure. 

## Starting the service

Now that the users and CAs are made, its time to start up the openVPN server. This is pretty simple too. First I just go to the VPN>openVPN tab in OPNsense, and configure a new server. A lot of these settings are auto filled. I made sure to turn on User Auth+TOTP to make sure there was 2fa for logging into the vpn. Setting the IPv4 tunnel netowrk to match my internal IP and enabled Dynamic IP, Address Pool, and Topology. Thats it for configuring the service!

## Firewall Rules

I had to make some firewall rules to allow traffic in to the network over the vpn. OPNsense makes this pretty straight forward by just adding some firewall rules for the WAN interface. All thats left is to set up the client device! 

## Adding a Client

For this step I installed openVPN on my linux machine, and exported my user profile from OPNsense to this machine, then the VPN client took care of the rest. I now have secure access to my services from anywhere!