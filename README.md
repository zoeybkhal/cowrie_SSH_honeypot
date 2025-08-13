# Building an SSH Honeypot with Cowrie
*A project to simulate and analyse cyber attacks by emulating a vulnerable Linux server.*  

- **Goal**: Understand how industry professionals use honeypots to investigate attacker behaviour by trapping their activity in a controlled environment.  
- **Why Cowrie?** Cowrie is lightweight, python-based and logs rich attack data. (JSON/TTY)

## **Setup & Configuration**  
### **1. Why choose Ubuntu Server Over Desktop?**  
- **Reasoning**: Lower resource usage → more room for the actual honeypot tools.
Smaller attack surface → less software for attackers to exploit.
- **Key Steps**:  
  ```bash
  # Install Ubuntu Server (no GUI)
  sudo apt update && sudo apt upgrade -y
