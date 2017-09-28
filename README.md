## Guide for connecting to eduroam on Arch Linux.

# Required Program/s:

* wpa_supplicant
* dhcpcd'
* ip'

' Should be preinstalled.

# NIC Driver:

Firstly ensure that your network interface card has the correct driver loaded and is recognised. You can test this with
	
	$ip link show

# Config File:

Create a file as follows and replace %username% and %password% accordingly:

	network={
	  ssid="eduroam"
	  key_mgmt=WPA-EAP
	  pairwise=CCMP
	  group= CCMP TKIP
	  eap=PEAP
	  identity="%username%@bangor.ac.uk"
	  domain_suffix_match="wifi.bangor.ac.uk"
	  phase2="auth=MSCHAPV2"
	  password="%password%"
	}

Save this file as eduroam.conf or something sensible. Take note of the path (maybe create this as root for better protection of plaintext password).

# Connection Script:

Find the NIC identifier by typing

	$ip link show
	
as suggested above.

So for example your wireless device may be known as wlo1.

Next you need to create another file, replacing %/PATHTO/eduroam.conf% and %NIC ID% accordingly:

	ip link set %NIC ID% up
	wpa_supplicant -B -i %NIC ID% -c %/PATHTO/eduroam.conf%
	dhcpcd %NIC ID%

Save this file as something like eduroamConnect.sh. This will be the script used to make the connection. You can set this to be run automatically upon boot.

# Connection:

To establish the connection, simply type into the terminal:

	sh /PATH/eduroamConnect.sh
