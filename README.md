# Building an SSH Honeypot with Cowrie
*A project to simulate and analyse cyber attacks by emulating a vulnerable Linux server.*  

- **Goal**: Understand how industry professionals use honeypots to investigate attacker behaviour by trapping their activity in a controlled environment.  
- **Why Cowrie?** Cowrie is lightweight, python-based and logs rich attack data. (JSON/TTY)

## **Setup & Configuration**  
### **1. Why choose Ubuntu Server Over Desktop?**  
- **Reasoning**: 

Lower resource usage → more room for the actual honeypot tools.

Smaller attack surface → less software for attackers to exploit.
- **Key Steps**:  
  ```bash
  # Install Ubuntu Server (no GUI)
  sudo apt update && sudo apt upgrade -y

## **Port Reconfiguration**
**Problem**: Cowrie defaults to port 2222, but attackers target port 22 (standard SSH). I learnt that in order for attackers to hit the honeypot, all traffic on port 22 must be intercepted and redirected to port 2222, where Cowrie is actively running and pretending to be a real SSH server.
Since our real SSH runs on port 22 by default, we must move this to a different port that attackers will not scan, for safety purposes.

**Solution**:
```bash
    # Change real SSH to port 2200
    sudo nano /etc/ssh/sshd_config  # Edit 'Port 2200'
    sudo systemctl restart ssh

    # Redirect port 22 to Cowrie (2222)
    sudo iptables -t nat -A PREROUTING -p tcp --dport 22 -j REDIRECT --to-port 2222
```
In the process of reconfiguring Cowrie to port 22 (for making attacks more likely) I learnt about port conflicts and firewall rules, that I was previously unaware of.

## **Connecting from Windows PowerShell**
**VirtualBox Port Forwarding:**
Host: 127.0.0.1:2222 → Guest: 2222

**Test Connection:**
```bash
ssh root@127.0.0.1 -p 2222  # Triggers Cowrie's fake shell
```

## **Keeping Cowrie Running**
**To ensure Cowrie survives reboots, I created a systemd service:**
```bash
sudo nano /etc/systemd/system/cowrie.service
```
**Paste the following:**
```bash
[Unit]
Description=Cowrie SSH Honeypot
After=network.target

[Service]
User=cowrie
WorkingDirectory=/home/cowrie/cowrie
ExecStart=/home/cowrie/cowrie/bin/cowrie start
Restart=always

[Install]
WantedBy=multi-user.target
```
**Then enable it**:
```bash
sudo systemctl enable cowrie && sudo systemctl start cowrie
```

- Feel free to check out the 'docs' file for screenshots with side-by-side explanations! 
- If you have also experimented with honeypots, I would love to hear about your experience and tips!

✉️ Contact me via LinkedIn! (https://www.linkedin.com/in/zoey-bou-khalil-a422a4272/)
