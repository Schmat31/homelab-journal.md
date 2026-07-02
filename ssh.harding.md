Code ran to add pub and private keys to get one set closer to getting ride of passwords on the VM 
===== RAN ON POWERSHELL ========

# 1 Gen the key pair Using thee ed25519 algo and a space to label the keys

ssh-keygen -t ed25519 -C "your-label-here"

# (pressed Enter to accept default save location, set a passphrase when prompted)

# 2 Copy the public to key to VM from powershell 

type $env:USERPROFILE\.ssh\id_ed25519.pub |  ssh vboxuser@192.168.1.24 "mkdir -p ~/.ssh && chmod 700 ~/.ssh && cat >> ~/.ssh/authorized_keys && chmod 600 ~/.ssh/authorized_keys"

# 3 Test key based login (Run in new terminal)

ssh vboxuser@192.168.1.24


# 4 tkaing away password authentiction and adding key auth 

sudo nano /etc/ssh/sshd_config 
PasswordAuthentication no
PubkeyAuthentication yes 

# 5 vaildate everything and testing the ssh 

sudo sshd -t 

# if no errors come up then 

sudo systemctl restart sshd 

# keep this terminal open and then open a new terminal 

ssh vboxuser@192.168.1.24



