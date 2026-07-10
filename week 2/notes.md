# Week 2 — 3rd of july to 10th of july

**What I did**
- Practiced core Linux CLI commands until they stopped requiring a man page check: 
  `ps aux`, `netstat -tulnp`, `grep`, `find`, `cat`, `less`, `head`, `tail`, and chaining with `|`
- Configured a static IP on my Kali box via CLI (`ip addr`, `/etc/network/interfaces`) instead of GUI/NetworkManager
- Installed Wireshark and captured 5 minutes of my own network traffic, filtered on `http` and `dns`
- I also completed RedHats free Ansible Basics: Automation Technical Overview
- Completed 3 more TryHackMe Pre-Security rooms (Linux Fundamentals Part 1-3)

**What I learned-ish**
- netstat -tulnp finally made sense once I connected it to 'what's listening and who owns it'
- learn the difference between `/etc/network/interfaces` and netplan/NetworkManager conflicts on newer Kali

**What confused me**
- I could see DNS queries but didn't fully understand why some were repeated / what a retransmission looked like.

**What I'm revisiting and why**
- Definetly DNS queries 

**Proof**
- Wireshark capture screenshot (in home-labs/wireshark)
- TryHackMe profile: https://tryhackme.com/p/nathanpereramm
- Static IP config screenshot or terminal output (in home-labs/network)
- Redhat doesnt offer certs for free courses so i just took a screenshot of me completing the course i put it inside week 2
