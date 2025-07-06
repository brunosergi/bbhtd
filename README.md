# 🪲 BBHTD - Bug Bounty Hunter's Toolkit in Docker

> **A containerized toolkit I built to stop hunting for bug bounty tools and start actually hunting bugs**

Tired of spending hours setting up bug bounty environments? BBHTD is a Docker environment with 25+ essential security tools that builds in under 30 minutes and eliminates setup headaches.

![BBHTD Banner](/images/bbhtd.png)

[![Docker](https://img.shields.io/badge/Docker-Ready-2496ED?logo=docker)](https://docker.com) [![Kali Linux](https://img.shields.io/badge/Based_on-Kali_Linux-purple.svg)](https://www.kali.org/) [![Tools](https://img.shields.io/badge/Tools-25+-orange.svg)](#-essential-tools) [![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

## 🚀 Quick Start

```bash
# Clone and launch
git clone https://github.com/brunosgio/bbhtd.git
cd bbhtd

# Enable GUI support (Linux/macOS)
sudo xhost +local:docker

# Launch the toolkit
docker compose up -d

# Get a shell
docker compose exec bbhtd bash
```

**That's it!** 🎉 You now have 25+ essential security tools ready to use.

## 🧪 Usage & Tool Verification

### Quick Tool Check
```bash
# Verify all core tools are working
subfinder -version
nuclei -version  
httpx -version
ffuf -V
nmap --version | head -1
amass version
```

### Basic Bug Bounty Workflow
```bash
# 1. Subdomain discovery
subfinder -d target.com -silent | head -5
echo "target.com" | assetfinder --subs-only

# 2. Check what's alive  
echo "https://target.com" | httpx -title -status-code -silent

# 3. Quick vulnerability scan
echo "https://target.com" | nuclei -t ~/nuclei-templates/http/technologies/ -silent

# 4. Directory discovery
ffuf -u https://target.com/FUZZ -w /usr/share/seclists/Discovery/Web-Content/common.txt -fc 404 -t 10 -s

# 5. Network scanning
nmap -T4 -p 80,443 target.com
```

### Utility Tools Test
```bash
# URL manipulation
echo "https://example.com/test?id=1&name=test" | unfurl domains
echo "https://example.com/test?id=1&name=test" | qsreplace FUZZ

# Deduplication
echo -e "url1\nurl2\nurl1\nurl3" | anew

# Web crawling
echo "target.com" | gau --threads 5 | head -5
echo "target.com" | waybackurls | head -5
```

### GUI Tools
```bash
# Launch Burp Suite
burpsuite &

# Open Firefox  
firefox https://target.com &
```

## 🛠️ Essential Tools Included

| Category | Tools |
|----------|-------|
| **🔍 Reconnaissance** | subfinder, amass, assetfinder, sublist3r, dnsrecon, dnsx, massdns |
| **🎯 Vulnerability Scanning** | nuclei, sqlmap, dalfox |
| **📁 Directory Discovery** | ffuf, gobuster, dirsearch |
| **🌐 Network Scanning** | nmap, masscan, naabu |
| **🌍 HTTP Tools** | httpx, httprobe, gowitness |
| **🔧 Utility Tools** | anew, unfurl, qsreplace, gf, seclists |
| **🖥️ GUI Tools** | Burp Suite, Firefox |
| **🕷️ Web Crawling** | gau, waybackurls, gospider |
| **🔎 OSINT** | theharvester, shodan |

## 💡 Why This Toolkit?

✨ **25+ Essential Tools** - Core tools that 90% of bug bounty hunters actually use  
⚡ **Fast Build** - Builds in ~30 minutes vs 1+ hours for bloated alternatives  
🐳 **One Command Setup** - No dependency hell or version conflicts  
🖥️ **GUI Support** - Burp Suite and Firefox with proper X11 forwarding  
📁 **Shared Workspace** - Easy file exchange via `shared/` directory  
🔧 **Kali-Native** - Uses official Kali packages for reliability  

## 🔧 Container Management

```bash
# Start/stop
docker compose up -d
docker compose down

# Rebuild after changes
docker compose build --no-cache

# Quick access alias (add to ~/.bashrc)
alias bbhtd='docker compose exec bbhtd bash'
```

## ⚙️ Customization

Want to add more tools? Edit `tools.env`:

```bash
# Enable additional tools
INSTALL_MASSCAN=true
INSTALL_GOWITNESS=true

# Rebuild
docker compose build --no-cache
```

## 🐛 Troubleshooting

**Tools not found?**
```bash
# Check PATH
echo $PATH
export PATH=/root/go/bin:$PATH
```

**Build issues?**
```bash
# Clean rebuild
docker system prune -a
docker compose build --no-cache
```

**GUI not working?**
```bash
sudo xhost +local:docker
```

## 🎯 What's Next

- **Performance Optimization** - Even faster builds and smaller images
- **Specialized Builds** - Web-only, Mobile, Cloud-focused variants  
- **Tool Updates** - Latest versions as they emerge
- **Extended Toolkit** - Optional full 80+ tool mode

---

<div align="center">

**⭐ Star this repo if it saves you setup time!**

Built by a bug bounty hunter, for bug bounty hunters 🎯

</div>