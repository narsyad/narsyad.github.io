---
title: Caddy Web Server
slug: caddy-web-server
description: Caddy simplifies web hosting with automatic HTTPS, easy configuration, and powerful reverse proxy features — making it an excellent choice for modern web deployment.
longDescription: ""
cardImage: "https://narsyad.github.io"
tags: ["caddy"]
readTime: 10
featured: true
timestamp: 2025-11-15T02:39:03+00:00
---

# Caddy Web Server Tutorial

## Introduction
Caddy is a modern, easy-to-use, and secure web server that automatically manages HTTPS using Let's Encrypt.  
This guide walks you through installation, configuration, and basic usage.

---

## 1. Install Caddy

### **Linux (Ubuntu/Debian)**
```bash
sudo apt install -y debian-keyring debian-archive-keyring apt-transport-https
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/gpg.key' | sudo gpg --dearmor -o /usr/share/keyrings/caddy.gpg
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/debian.deb.txt' | sudo tee /etc/apt/sources.list.d/caddy-stable.list
sudo apt update
sudo apt install caddy
```

### **macOS (Homebrew)**
```bash
brew install caddy
```

---

## 2. Basic Usage

### **Run a simple HTTP server**
```bash
caddy file-server --listen :8080 --root ./site
```

---

## 3. Caddyfile Configuration

### **Basic Caddyfile**
```text
example.com {
    root * /var/www/html
    file_server
}
```

Place this file at:
```
/etc/caddy/Caddyfile
```

Reload Caddy:
```bash
sudo systemctl reload caddy
```

---

## 4. Reverse Proxy Example
```text
example.com {
    reverse_proxy localhost:3000
}
```

---

## 5. Enable SSL Automatically
Caddy will automatically issue and renew HTTPS certificates with Let's Encrypt.  
Just use a real domain pointing to your server.

---

## 6. Run as a System Service

Check status:
```bash
sudo systemctl status caddy
```

Restart:
```bash
sudo systemctl restart caddy
```

Enable at startup:
```bash
sudo systemctl enable caddy
```

---

## 7. Logs and Debugging

Check logs:
```bash
journalctl -u caddy -f
```

---
