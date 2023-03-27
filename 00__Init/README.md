#				REPORT


----------------------------Network-----------------------------------

1- ifconfig -s | awk '{if (NR > 1) print }'

2-a)ifconfig eth0 | grep 'broadcast' | awk '{print }'

  b)ip addr show eth0 | awk '/inet / {print }'

3- ifconfig wlan0 | grep 'HWaddr' | awk '{print }'
   ifconfig eth0 | grep 'ether' | awk '{print }'

4-ip route show default | awk '/default/ {print }'
  route -n | awk '/^0.0.0.0/ {print }'
  
5-dig slash16.org

6- The  complete path of the file that contains the IP address of the DNS server i'm using is /etc/resolv.conf

7-dig slash16.org @8.8.8.8

8- No provider

9-163.172.250.16

10- traceroute slash16.org

11- homerouter.cpe

12- ip addr show
    ifconfig

13- host ip_addr (for me, no host return)

14-/etc/systemd/resolved.conf

15- To do this, first open the file /etc/hosts and then add the line "46.19.122.85 intra.42.fr"





-------------------------------System---------------------------------

1- /etc/os-release

2- sudo hostnamectl set-hostname <new-hostname>

3- /etc/hostname

4- uptime

5- systemctl status ssh.service

6- systemctl restart ssh.service

7- pgrep sshd

8-/home/<username>/.ssh/authorized_keys

9- w

10- sudo fdisk -l

11- df -h

12- du -h -d 1 /var/

13- top

14- done

15- kill

16- it's cron

17- ssh username@IP_address

18- sudo systemctl stop ssh

19- sudo systemctl list-unit-files | grep enabled

20- cut -d: -f1 /etc/passwd

21- cut -d: -f1,3 /etc/passwd | grep ':[1-9][0-9][0-9][0-9]$' | cut -d: -f1

22- sudo adduser <username>

23- 
To connect as a new user with a graphical session, follow these steps:

*Log out of your current session and return to the login screen.
*Click the "Switch User" button to display the login screen for a new session.
*Enter the username and password of the new user you want to log in as.
*Choose the desktop environment or window manager you want to use, if applicable.
*Click the "Log In" button to start the new session.

To connect as a new user with an SSH session, follow these steps:

*Open a terminal or command prompt on your local computer.
*Use the ssh command to connect to the Debian VM. Here's the command:

ssh <username>@<IP address>

*Replace <username> with the username of the new user you want to connect as, and <IP address> with the IP address or hostname of the Debian VM.

*Enter the password for the new user when prompted.

You should now be logged in as the new user via SSH. You can execute commands and use the terminal as usual.

24- dpkg -l





-----------------------------Scripting--------------------------------


1- 
#!/bin/bash

# Read the "/etc/passwd" file line by line
while read -r line; do

  # Extract the login, UID, and path fields from the line
  login=$(echo "$line" | cut -d: -f1)
  uid=$(echo "$line" | cut -d: -f3)
  path=$(echo "$line" | cut -d: -f6)

  # Display the login, UID, and path fields
  echo "Login: $login"
  echo "UID: $uid"
  echo "Path: $path"
  echo " "

done < "/etc/passwd"


2-
#!/bin/bash

# Ask for the username to be deleted
read -p "Enter the username to be deleted: " username

# Check if the user exists and is currently logged in
if id "$username" &>/dev/null; then
  if who | grep -wq "^$username"; then
    echo "User '$username' is currently logged in. Please log them out first."
    exit 1
  fi
else
  echo "User '$username' does not exist."
  exit 1
fi

# Remove the user and their home directory
userdel -r "$username"

echo "User '$username' has been deleted."


3-
here's an example script that prints a welcome message and then asks the user to enter their name and age. It then checks if the user is old enough to vote and prints an appropriate message

#!/bin/bash

# Print a welcome message
echo "Welcome to the voting eligibility checker!"

# Ask for the user's name and age
read -p "Please enter your name: " name
read -p "Please enter your age: " age

# Check if the user is old enough to vote
if [[ $age -ge 18 ]]; then
  echo "Congratulations $name, you are eligible to vote!"
else
  echo "Sorry $name, you are not yet old enough to vote."
fi
