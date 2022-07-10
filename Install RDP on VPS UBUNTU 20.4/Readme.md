Remote Desktop Protocol allows users to access remote systems desktops. The XRDP service provides you a graphical login to the remote machines using Microsoft RDP (​Remote Desktop Protocol). The XRDP also supports two-way clipboard transfer (text, bitmap, file), audio redirection, and drive redirection (mount local client drives on the remote machines).

## Step 1 – Install Desktop Environment
Open a terminal and upgrade all installed packages with the following command:
```bash
sudo apt update && sudo apt upgrade
```
Once your system is updated, install the Tasksel utility to install a desktop environment:
```bash
apt install tasksel -y
```
After installing Tasksel, launch the Tasksel utility with the following command:
```bash
tasksel
```
``You should see the following interface:``

![tasksel-ubuntu-desktop](https://user-images.githubusercontent.com/81378817/178128383-95a301d7-8e46-4d01-b197-034294957f23.png)

Use the arrow key to scroll down the list and find Ubuntu desktop. Next, press the `Space key` to select it then press the `Tab key` to select `OK` then hit `Enter` to install the Ubuntu desktop

Once all the packages are installed, you will need to set your system boots into the graphical target. You can set it with the following command:
```bash
systemctl set-default graphical.target 
```
Next, restart your system to apply the changes.

## Step 2 – Installing XRDP on Ubuntu

The Xrdp packages are available under the default system repositories. You can install a remote desktop on your Ubuntu system by executing the following command.
```bash
sudo apt install xrdp -y 
```
Once the xrdp installation finished successfully, its service will be started automatically. To verify the service status run the command:
```bash
sudo systemctl status xrdp 
```
![running-xrdp-on-ubuntu2004](https://user-images.githubusercontent.com/81378817/178128471-2984c8bd-afac-4d60-ae3b-84284b4e3980.png)

The above output shows the Xrdp service is up and running.

## Step 3 – Configuring Xrdp
During the installation, xrdp added a user in your system named “xrdp”. The xrdp session uses a certificate key file “/etc/ssl/private/ssl-cert-snakeoil.key”, which plays an important role with remote desktop.

In order to work it properly, add the xrdp user to the “ssl-cert” group with the following command.
```bash
sudo usermod -a -G ssl-cert xrdp 
```
Sometimes user faces issue with black screen appears in background. So, that I ahave included steps to resolve black screen issue in background. Edit the xrdp file `/etc/xrdp/startwm.sh` in a text editor:
```bash
sudo nano /etc/xrdp/startwm.sh 
```
Add these commands before the commands that test & execute Xsession as shown below:
```bash
unset DBUS_SESSION_BUS_ADDRESS
unset XDG_RUNTIME_DIR
```
![fix-blank-screen-error-with-xrdp-on-ubuntu-2004](https://user-images.githubusercontent.com/81378817/178128565-cbed8677-f289-4813-be15-878e2c18ec20.png)

Press `CTRL+O` to write out and then `CTRL+X` to exit from the editor.

Restart the Xrdp service by running the command given below:
```bash
sudo systemctl restart xrdp 
```
## Step 4 – Adjust Firewall
The Xrdp listens on port 3389, which is the default port for the RDP protocol. You need to adjust the firewall to allow access to port 3389 for remote systems.

Systems running with UFW firewall, use the following command to open port `3389` for the LAN network.
```bash
sudo ufw allow from 192.168.1.0/24 to any port 3389 
```
Reload the UFW to apply the new rules.
```bash
sudo ufw reload
```
Congrats, Your system is ready to access over RDP protocol. you can open on Pc and Android

## Conclusion
This tutorial helped you to set up a remote desktop service on Ubuntu 20.04 system with Xrdp. The tutorial also includes steps to install Desktop Environment on a Ubuntu system

Thank you
