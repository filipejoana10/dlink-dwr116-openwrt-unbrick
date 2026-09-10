🖥️ STEP 1: Configure Static IP on Windows
To communicate with the router in Emergency Recovery Mode, your computer must be set to the 192.168.0.x IP range.

On Windows, press Win + R, type ncpa.cpl, and press Enter.

Right-click your Ethernet Network Adapter and select Properties.

Double-click Internet Protocol Version 4 (TCP/IPv4).

Select "Use the following IP address" and fill in:

IP address: 192.168.0.2

Subnet mask: 255.255.255.0

Default gateway: 192.168.0.1

Click OK and then OK.2. Configure the Repeated Local Wi-Fi
To broadcast a new wireless network from the D-Link:

Under Network -> Wireless, click Add on the radio0 row.

Under General Setup:

Mode: Select Access Point.

ESSID: Set your new network name (e.g., Home-Ext3).

Network: Check the lan interface box.

Under Wireless Security:

Encryption: Select WPA2-PSK (CCMP).

Key: Enter the new password for your repeated Wi-Fi network.

Click Save, then click Save & Apply.🌐 STEP 4: Connect to Main Wi-Fi & Broadcast Local SSID
Navigate to Network -> Wireless to configure wireless reception and broadcast:

1. Connect to the Main Router (ISP Wi-Fi)
On the wireless radio (radio0), click the Scan button.

Find the primary Wi-Fi network (e.g., "Home") and click Join Network.

In the configuration popup:

WPA passphrase: Enter the password of the main Wi-Fi network.

Name of the new network: Ensure it is set to wwan.

Create / Assign firewall-zone: Select the wan zone.

Click Submit, then click Save & Apply at the bottom of the page.🧪 STEP 6: Connectivity Diagnostic Tests
Run ping tests in the SSH terminal to verify routing health:

Verify router internet reachability:

1> Bash
ping -c 4 google.com
Verify LAN bridge (br-lan) interface forwarding:

2> Bash
ping -c 4 -I br-lan 8.8.8.8
Setup is successful when both diagnostic tests return 0% packet loss.STEP 5: Enable Internet Passthrough for LAN Ports (via SSH)
To ensure outbound traffic on physical LAN ports (br-lan) is correctly masqueraded through OpenWrt 22.03's nftables firewall:

Open the Windows Command Prompt (Win + R -> cmd) and establish an SSH session:

 1> Bash
ssh root@192.168.1.1
Paste the following block into the terminal to bind wlan0 to the wan zone and update firewall rules:

2> Bash
uci set network.wwan=interface
uci set network.wwan.proto='dhcp'
uci set network.wwan.device='wlan0'
uci set firewall.@zone[1].network='wan wan6 wwan'
uci set firewall.@zone[1].masq='1'
uci add firewall forwarding
uci set firewall.@forwarding[-1].src='lan'
uci set firewall.@forwarding[-1].dest='wan'
uci commit
service network restart
service firewall restart
wifi reload
Force a DHCP request on the wireless interface:

3> Bash
udhcpc -i wlan0💾 STEP 7: Generate and Download System Backup
After confirming active internet connectivity across all interfaces, generate a full configuration backup:

Open LuCI in your browser (http://192.168.1.1).

Go to System -> Backup / Flash Firmware.

Under the Backup section, click Generate archive.

The browser will download an archive file (e.g., backup-OpenWrt-xxxx.tar.gz).

Store this file safely. If the router is reset to defaults, restoring this file under Restore backup will instantly re-apply all settings.

Documented by Filipe Joana.📊 How to Verify Internet Connection in LuCI
Go to Network -> Wireless and inspect the Wireless Overview section:

Active Connection Indicator (Associated Stations):

Under Associated Stations, you should see Client "Home" (wlan0) listing the MAC Address and Host/IP (e.g., 192.168.150.1) assigned by the primary router.

The Signal / Noise bar will display signal strength (e.g., -64 dBm).

Radio Modes (radio0):

SSID: Home | Mode: Client -> Confirms client association to the upstream router.

SSID: Home-Ext3 | Mode: Master -> Confirms local AP broadcast status.⚙️ STEP 3: Revert Windows to DHCP and Access LuCI
Return to the Windows adapter settings (Win + R -> ncpa.cpl -> IPv4 Properties).

Select "Obtain an IP address automatically" and click OK.

Your PC will automatically receive an IP address in the 192.168.1.x range.

Open your browser and go to: http://192.168.1.1.

On the OpenWrt (LuCI) login page:

Username: root

Password: (Leave blank)

Click Log in.🔄 STEP 2: Unbrick via Emergency Room (Recovery Mode)
Unplug the router's power supply from the wall.

Using a paperclip or pin, press and hold the RESET button on the rear panel.

While keeping the RESET button held down, plug the power cable back in.

Hold the RESET button for 10 to 15 seconds until the Power LED begins flashing orange/red.

Open your web browser and navigate to: http://192.168.0.1.

On the recovery web page, click Choose File and select the downloaded OpenWrt .bin factory file.

Click Upload / Send.

WAIT 3 TO 5 MINUTES without touching anything.

Critical Step: Once the Power LED stabilizes, unplug the router from power, wait 5 seconds, and plug it back in.