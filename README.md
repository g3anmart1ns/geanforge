<p align="center">
  <img src="https://img.shields.io/badge/geanforge-v1.0.0-blue?style=flat-square" alt="Version">
  <img src="https://img.shields.io/badge/license-MIT-green?style=flat-square" alt="License">
  <img src="https://img.shields.io/badge/platform-Linux%20%7C%20macOS-lightgrey?style=flat-square" alt="Platform">
  <img src="https://img.shields.io/badge/bash-4.0%2B-orange?style=flat-square" alt="Bash">
  <img src="https://img.shields.io/github/last-commit/geanmartins/geanforge?style=flat-square" alt="Last Commit">
</p>

<h1 align="center">🔐 geanforge</h1>

<p align="center">
  <strong>Secure password generator for production, Kubernetes, and homelab environments</strong><br>
  <em>Forge strong passwords from the comfort of your terminal</em>
</p>

<p align="center">
  <a href="#-features">Features</a> •
  <a href="#-installation">Installation</a> •
  <a href="#-quick-start">Quick Start</a> •
  <a href="#-usage">Usage</a> •
  <a href="#-examples">Examples</a> •
  <a href="#-security">Security</a>
</p>

---

## ✨ Features

- **🔒 Cryptographically Secure**: Uses `/dev/urandom` and `openssl` with bias reduction
- **🐳 Kubernetes Ready**: Built-in base64 encoding perfect for K8s secrets
- **🔑 Multiple Hash Methods**: bcrypt (htpasswd), SHA-512 (mkpasswd/openssl)
- **📱 Cross-Platform Clipboard**: Auto-detects X11, Wayland, and macOS
- **🎲 Diceware Passphrases**: Generate memorable, strong passphrases
- **🔢 PIN Generation**: Numeric PINs without leading zeros
- **🛡️ Entropy Checking**: Validates system entropy before generation
- **🧹 Memory Cleanup**: Best-effort cleanup of sensitive variables
- **📊 Strength Verification**: Optional password strength analysis via cracklib
- **🚫 No External Dependencies**: Pure bash with common system utilities

## 🚀 Installation

### Quick Install (Recommended)

```bash
curl -fsSL https://raw.githubusercontent.com/geanmartins/geanforge/main/geanforge \
  | sudo tee /usr/local/bin/geanforge > /dev/null
sudo chmod +x /usr/local/bin/geanforge
