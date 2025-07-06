FROM kalilinux/kali-rolling:latest

# Metadata
LABEL maintainer="Bruno Sergio @brunosgio" \
      description="Bug Bounty Hunter's Toolkit in Docker - Essential Tools Only" \
      version="2.0-minimal"

# Environment variables
ENV DEBIAN_FRONTEND=noninteractive \
    GOPATH=/root/go \
    PATH=$GOPATH/bin:$PATH \
    GO111MODULE=on

# Set working directory
WORKDIR /root

# Create necessary directories
RUN mkdir -p /root/go/bin /root/tools

# Copy tools configuration
COPY tools.env /root/tools.env

# Configure shell aliases and environment
RUN echo "export DISPLAY=:0" >> ~/.profile && \
    echo "export PATH=/root/go/bin:\$PATH" >> ~/.profile && \
    echo "export PATH=/root/go/bin:\$PATH" >> ~/.bashrc && \
    echo "alias burpsuite='java -jar -Xmx2G /usr/bin/burpsuite --no-sandbox &> /dev/null &'" >> ~/.bashrc && \
    echo "alias firefox='/usr/bin/firefox --no-sandbox &> /dev/null &'" >> ~/.bashrc && \
    echo "source /root/tools.env" >> ~/.bashrc

# Install system dependencies and essential tools
RUN apt-get update && apt-get install -y \
    # Build essentials
    build-essential apt-utils sudo locate vim nano tmux wget curl git \
    # Network tools
    whois host netcat-openbsd traceroute tor proxychains4 prips ftp ssh \
    # System libraries
    libpcap-dev pv net-tools dnsutils iputils-ping libldns-dev \
    libssl-dev libffi-dev libxml2-dev libxslt1-dev zlib1g-dev libgmp-dev \
    # GUI support
    libcanberra-gtk3-0 libgl1-mesa-dri libgl1-mesa-dev \
    libpulse0 libv4l-0 fonts-symbola xvfb x11-xserver-utils \
    # Additional tools
    apt-transport-https ca-certificates upx xclip awscli \
    # Development tools
    golang python3 python3-pip python3-dev python3-setuptools python3-venv gcc cmake ruby \
    openjdk-11-jdk-headless dpkg unrar zip jq \
    && rm -rf /var/lib/apt/lists/*

# Install GUI tools (if enabled)
RUN . /root/tools.env && \
    if [ "$INSTALL_BURPSUITE" = "true" ] || [ "$INSTALL_FIREFOX" = "true" ]; then \
        apt-get update && apt-get install -y \
            $([ "$INSTALL_BURPSUITE" = "true" ] && echo "burpsuite") \
            $([ "$INSTALL_FIREFOX" = "true" ] && echo "firefox-esr") \
        && rm -rf /var/lib/apt/lists/*; \
    fi

# Install apt-based tools
RUN . /root/tools.env && \
    TOOLS_TO_INSTALL="" && \
    [ "$INSTALL_AMASS" = "true" ] && TOOLS_TO_INSTALL="$TOOLS_TO_INSTALL amass"; \
    [ "$INSTALL_DIRSEARCH" = "true" ] && TOOLS_TO_INSTALL="$TOOLS_TO_INSTALL dirsearch"; \
    [ "$INSTALL_DNSRECON" = "true" ] && TOOLS_TO_INSTALL="$TOOLS_TO_INSTALL dnsrecon"; \
    [ "$INSTALL_GOBUSTER" = "true" ] && TOOLS_TO_INSTALL="$TOOLS_TO_INSTALL gobuster"; \
    [ "$INSTALL_MASSCAN" = "true" ] && TOOLS_TO_INSTALL="$TOOLS_TO_INSTALL masscan"; \
    [ "$INSTALL_MASSDNS" = "true" ] && TOOLS_TO_INSTALL="$TOOLS_TO_INSTALL massdns"; \
    [ "$INSTALL_NAABU" = "true" ] && TOOLS_TO_INSTALL="$TOOLS_TO_INSTALL naabu"; \
    [ "$INSTALL_NMAP" = "true" ] && TOOLS_TO_INSTALL="$TOOLS_TO_INSTALL nmap"; \
    [ "$INSTALL_SECLISTS" = "true" ] && TOOLS_TO_INSTALL="$TOOLS_TO_INSTALL seclists"; \
    [ "$INSTALL_SQLMAP" = "true" ] && TOOLS_TO_INSTALL="$TOOLS_TO_INSTALL sqlmap"; \
    [ "$INSTALL_SUBLIST3R" = "true" ] && TOOLS_TO_INSTALL="$TOOLS_TO_INSTALL sublist3r"; \
    [ "$INSTALL_THEHARVESTER" = "true" ] && TOOLS_TO_INSTALL="$TOOLS_TO_INSTALL theharvester"; \
    if [ -n "$TOOLS_TO_INSTALL" ]; then \
        apt-get update && apt-get install -y $TOOLS_TO_INSTALL && rm -rf /var/lib/apt/lists/*; \
    fi

# Install Python tools (fix package conflicts)
RUN . /root/tools.env && \
    python3 -m pip install --break-system-packages --upgrade pip setuptools wheel && \
    [ "$INSTALL_SHODAN" = "true" ] && python3 -m pip install --break-system-packages --no-deps shodan || true; \
    echo "Python tools installation completed"

# Install Go tools (using go install instead of deprecated go get)
RUN . /root/tools.env && \
    [ "$INSTALL_ANEW" = "true" ] && go install github.com/tomnomnom/anew@latest; \
    [ "$INSTALL_ASSETFINDER" = "true" ] && go install github.com/tomnomnom/assetfinder@latest; \
    [ "$INSTALL_DALFOX" = "true" ] && go install github.com/hahwul/dalfox/v2@latest; \
    [ "$INSTALL_DNSX" = "true" ] && go install github.com/projectdiscovery/dnsx/cmd/dnsx@latest; \
    [ "$INSTALL_FFUF" = "true" ] && go install github.com/ffuf/ffuf@latest; \
    [ "$INSTALL_GAU" = "true" ] && go install github.com/lc/gau/v2/cmd/gau@latest; \
    [ "$INSTALL_GF" = "true" ] && go install github.com/tomnomnom/gf@latest; \
    [ "$INSTALL_GOSPIDER" = "true" ] && go install github.com/jaeles-project/gospider@latest; \
    [ "$INSTALL_GOWITNESS" = "true" ] && go install github.com/sensepost/gowitness@latest; \
    [ "$INSTALL_HTTPROBE" = "true" ] && go install github.com/tomnomnom/httprobe@latest; \
    [ "$INSTALL_HTTPX" = "true" ] && go install github.com/projectdiscovery/httpx/cmd/httpx@latest; \
    [ "$INSTALL_NUCLEI" = "true" ] && go install github.com/projectdiscovery/nuclei/v2/cmd/nuclei@latest; \
    [ "$INSTALL_QSREPLACE" = "true" ] && go install github.com/tomnomnom/qsreplace@latest; \
    [ "$INSTALL_SUBFINDER" = "true" ] && go install github.com/projectdiscovery/subfinder/v2/cmd/subfinder@latest; \
    [ "$INSTALL_UNFURL" = "true" ] && go install github.com/tomnomnom/unfurl@latest; \
    [ "$INSTALL_WAYBACKURLS" = "true" ] && go install github.com/tomnomnom/waybackurls@latest; \
    echo "Go tools installation completed"


# Update nuclei templates if nuclei is installed
RUN . /root/tools.env && \
    [ "$INSTALL_NUCLEI" = "true" ] && timeout 300 /root/go/bin/nuclei -update-templates 2>/dev/null || true && \
    echo "Nuclei templates updated"

# Clean up
RUN apt-get autoremove -y && \
    apt-get autoclean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    go clean -cache -modcache -testcache 2>/dev/null || true

# Set the default command
CMD ["/bin/bash"]