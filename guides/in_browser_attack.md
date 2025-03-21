# IN BROWSER ATTACK - DevCybSec

__This is document to show how the attackers do an in browser attack from scratch__

> First we have to install kali linux without GUI to simulate we are renting a cloud hosted kali linux machine like in aws.

__Install as always and uncheck any desktop environment packages such as:__
- Kali Desktop Environment - Xfce
- GNOME
- KDE

 ⚠️ Don't forget to install GRUB boot loader to the primary drive (e.g., /dev/sda).

 > When your OS is working correctly, we are going to install and config an ssh server.

 Below is a guide on configuring an SSH server in Kali Linux (or most Debian‐based systems) in a secure manner. These recommendations focus on reducing attack surface and strengthening authentication measures.

---

## 1. Install the SSH Server Package

If it’s not already installed:
```bash
sudo apt update
sudo apt install openssh-server
```

The default installation will create and enable a systemd service for `sshd`.

---

## 2. Verify/Enable the SSH Service

1. **Check Service Status**:
   ```bash
   sudo systemctl status ssh
   ```
   If it’s active (running), you should see a green “active” status.

2. **Enable on Boot**:
   ```bash
   sudo systemctl enable ssh
   ```
   This ensures the SSH service starts automatically whenever the system boots.

3. **Start the Service** (if not already running):
   ```bash
   sudo systemctl start ssh
   ```

---

## 3. Basic SSH Configuration

### Edit the `sshd_config` file

The primary SSH daemon configuration file is located at:
```
/etc/ssh/sshd_config
```
Open it with your preferred text editor, for example:
```bash
sudo nano /etc/ssh/sshd_config
```

Below are some recommended changes for better security.

1. **Disable Root Login**  

   Prevent direct SSH login as `root`. Instead, log in as a normal user and use `sudo` for administrative tasks.  
   ```
   PermitRootLogin no
   ```
   _(If you need occasional direct root SSH login for some reason, consider setting it to `prohibit-password` and using only keys, but “no” is more secure.)_

2. **Change the Default SSH Port** (optional but can reduce random scans)  

   ```
   Port 2222
   ```
   Replace `2222` with any non‐standard port above 1024. Keep in mind you also need to update your firewall rules (if any) and specify the port when connecting (e.g., `ssh -p 2222 user@host`).

3. **Protocol and Address Family**  

   Ensure only SSH v2 is used:
   ```
   Protocol 2
   ```
   If you only need IPv4, you can restrict listening to IPv4:
   ```
   AddressFamily inet
   ```
   _(If you want to allow IPv6, leave this out or set it to `any`.)_

4. **Limit Auth Methods (Key‐Based Authentication)**  

   For maximum security, consider using only key‐based authentication. Generate an SSH key pair, copy the public key to your server, and disallow password logins:
   ```
   PasswordAuthentication no
   PubkeyAuthentication yes
   ```
   (We’ll discuss key‐based authentication more in the next section.)

5. **Idle Session Timeout**  

   Automatically log out idle sessions:
   ```
   ClientAliveInterval 300
   ClientAliveCountMax 2
   ```
   This will disconnect idle SSH sessions after about 10 minutes (300 seconds x 2).

6. **Limit SSH to Specific Users** (optional)  

   ```
   AllowUsers yourusername
   ```
   If you only want SSH logins for certain user(s), list them here.  

7. **Banner (Optional)**  

   Some organizations use a warning banner. You can specify a file:
   ```
   Banner /etc/issue.net
   ```
   and place your legal banner text in `/etc/issue.net`.

**After making changes**, save the file and reload/restart SSH:
```bash
sudo systemctl restart ssh
```
Use a second session to test your new settings so you don’t lock yourself out in case of misconfiguration.

---

## 4. Set Up Key‐Based Authentication

If you decided to disable password logins, you need to ensure key‐based authentication is set up. Here’s how:

1. **Generate Key Pair (on your local machine)**  

   On your client system (not the server), run:
   ```bash
   ssh-keygen -t ed25519
   ```
   - **`-t ed25519`**: Use the Ed25519 key algorithm (preferred for strong security and small key size).  
   - You can also use RSA (`-t rsa -b 4096`) if you prefer.  
   - You’ll be asked for a file name (default is `~/.ssh/id_ed25519`) and optionally a passphrase.

2. **Copy Your Public Key to the Server**  

   Use the `ssh-copy-id` helper script:
   ```bash
   ssh-copy-id -i ~/.ssh/id_ed25519.pub youruser@yourserver
   ```
   or manually append the content of `~/.ssh/id_ed25519.pub` to the server’s `~/.ssh/authorized_keys`.

3. **Test the Key Login**  

   From your local machine:
   ```bash
   ssh -i ~/.ssh/id_ed25519 youruser@yourserver
   ```
   You should log in without a password prompt (unless you set a passphrase on your key).

4. **Disable PasswordAuthentication**  

   Once confirmed that key authentication works, edit `/etc/ssh/sshd_config` on the server and ensure:
   ```
   PasswordAuthentication no
   ```
   Then:
   ```bash
   sudo systemctl restart ssh
   ```
   This ensures only key‐based login is accepted.

---

## 5. Configure a Host Firewall (Optional but Recommended)

A local firewall reduces the exposed attack surface. Kali comes with `ufw` (Uncomplicated Firewall) or the more traditional `iptables`. Using `ufw` is simpler:

1. **Install and Enable `ufw`**:
   ```bash
   sudo apt install ufw
   sudo systemctl enable ufw
   ```
2. **Allow SSH (if you changed the default port, add it here)**:
   ```bash
   sudo ufw allow 2222/tcp
   ```
   If you left SSH on port 22, use:
   ```bash
   sudo ufw allow ssh
   ```
3. **Enable `ufw`**:
   ```bash
   sudo ufw enable
   ```
4. **Check Status**:
   ```bash
   sudo ufw status
   ```

---

## 6. (Optional) Brute‐Force Protection with Fail2ban

To protect SSH against brute force attempts:

1. **Install fail2ban**:
   ```bash
   sudo apt install fail2ban
   ```
2. **Configure**:
   - Copy the default config:
     ```bash
     sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
     ```
   - Edit `/etc/fail2ban/jail.local` to change default settings, ban times, etc. 
   - By default, the `[sshd]` section is present and will be used to monitor SSH login attempts.
3. **Start and Enable fail2ban**:
   ```bash
   sudo systemctl enable fail2ban
   sudo systemctl start fail2ban
   ```
4. **Check Status**:
   ```bash
   sudo fail2ban-client status sshd
   ```
   You can see how many IPs have been banned, etc.

---

## 7. Test & Verify

After completing all configurations:

1. **Restart SSH**:  
   ```bash
   sudo systemctl restart ssh
   ```
2. **Test From Another Machine**:  
   - Try connecting via SSH on the specified port and confirm key‐based login works.  
   - Confirm that password login is denied (if you disabled it).  
   - If you changed the port, remember to specify it (`ssh -p 2222 user@host`).
3. **Check Logs**:  
   ```bash
   sudo tail -f /var/log/auth.log
   ```
   Observe any SSH login activity or potential errors.

---

We will add a second network adapter in our no-gui kali machine to access from a third client machine:

1. **We will apply the following command to know the new interface name**
```bash
ip link show
```
 `result:`

 ```nginx
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP mode DEFAULT group default qlen 1000
    link/ether 00:0c:29:f0:c3:dc brd ff:ff:ff:ff:ff:ff
3: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP mode DEFAULT group default qlen 1000
    link/ether 00:0c:29:f0:c3:e6 brd ff:ff:ff:ff:ff:ff
                                                         
 ```

2. **We Will modify our /etc/network/interface config file now we know our interface name**

```bash
auto eth1
iface eth1 inet static
    address 192.168.154.4
    netmask 255.255.255.0
    gateway 192.168.154.1
```

3. **We must bring up our interface**
```
sudo ifdown eth1 && sudo ifup eth1
```

4. **(Optionally) Restart the networking service**
```
sudo systemctl restart networking
```

---

We will install a service called VNC to access our "cloud machine" (in this case the no-gui kali linux) to expose a graphical interface. 

But before we install vnc server we have to install a graphical user interface on the server because cloud providers do not provide a graphical way to manipulate their machines.

Then we will install a vnc client in our attacker machine to communicate with the vnc server. 

> Installing graphical user interface and add ons

```
sudo apt install xfce4 xfce4-goodies
```

> Installing VNC server
```
sudo apt install tightvncserver
```

⚠️ __We have to modify the ⁓/.vnc/xstartup and add this line__ ⚠️
```bash
#!/bin/sh
xrdb $HOME/.Xresources
startxfce4 &
```

> Allow the vnc port with ufw
```terminal
sudo ufw allow 5901/tcp
```

> Set a vnc password running 
```terminal
vncpasswd
```

> To start the vnc server we have to run:
```
tightvncserver -geometry 1920x1080 -depht 30
```

⚠️ _This will ask us for a password that we must remember to connect to the server from our client_ ⚠️

> Install remote ripple [Remote Ripple](https://remoteripple.com/) as vnc client.

We just have to provide the remote kali machine ip - port - and the password we set before.

⚠️ To stop(kill) the vnc server we just have to type

```
tightvncserver -kill <desktop-created>
```

---

### Now we will install NoVNC to access our cloud machine using a regular browser

```terminal
sudo apt install novnc
```

Allow the http port with ufw
```terminal
sudo ufw allow 80/tcp
```
__We have to find the novnc_proxy location, in some cases we could find it in: /usr/share/novnc/utils/novnc_proxy and tell it to listen in port 80 and where is running our vnc server.__ 

```
/usr/share/novnc/utils/novnc_proxy --listen 80 --vnc localhost:5901
```

-------------------------------------------

We have to install a browser on out cloud machine, i will use firefox and run it in kiosk mode.

```terminal
sudo apt install firefox-esr 
firefox --kiosk <website we are spoofing>
```

⚠️ If you cannot run firefox, because is not detecting some display, run this:

```terminal
export DISPLAY=:1
```

and we will modify the novnc html file to improve our window view.

```terminal
nano /usr/share/novnc/vnc_lite.html
```

```html
<title>The website we are spoofing</title>

<style>
   #top_bar{
      .
      .
      display: none;
   }

   #status{
      .
      .
      display: none;
   }

   ##sendCtrlAltDelButton{
      .
      .
      .
      display: none;
   }
</style>
```

----

### Now we can visit the url to watch the result.
http://<MACHINE_IP>/vnc_lite.html?autoconnect=true&password=<password>&quality=9&scale=true