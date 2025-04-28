# project3
# SSH login attempt Monitoring and IP blocking using Bash Scripting
# Description
This project monitors SSH login attempts and detects multiple failed login attempts and block ip automatically using a Bash script.

# Tool used
Putty – Used to simulate SSH login attempts from Windows.

Bash Scripting – Automate log monitoring and detection.

VirtualBox – Kali Linux machine set up as Victim/User.

Windows PC – Used to act as the Attacker machine.

# Bash Scripting for SSH login attempt identify and block the ip
        logfile= " /var/log/auth.log "
        outputfile= "ssh_login_check"
        attempt=3
        > "$outputfile"
       
        grep "Failed password" "$logfile" | awk '{print $(NF-3)}' | sort | uniq -c > tempfile.txt
        cat tempfile.txt | while read  line
        
        do
        
         COUNT=$(echo $line | awk '{print $1}')
          IP=$(echo $line | awk '{print $2}')

         if [ $COUNT -gt $attempt ]; then
        echo "IP : $IP - Failed Attempts : $COUNT" > "$outputfile"
        iptables -A INPUT -s $IP -j DROP
         
         fi
         
        done 
        echo "Detection Completed. check $outputfile for result"
