Aqui está o README.md profissional completo para o `geanforge`, pronto para ser publicado no GitHub:

## 📄 README.md

```markdown
<p align="center">
  <img src="https://img.shields.io/badge/geanforge-v1.0.0-blue?style=flat-square" alt="Version">
  <img src="https://img.shields.io/badge/license-MIT-green?style=flat-square" alt="License">
  <img src="https://img.shields.io/badge/platform-Linux%20%7C%20macOS-lightgrey?style=flat-square" alt="Platform">
  <img src="https://img.shields.io/badge/bash-4.0%2B-orange?style=flat-square" alt="Bash">
  <img src="https://img.shields.io/github/last-commit/g3anmart1ns/geanforge?style=flat-square" alt="Last Commit">
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
curl -fsSL https://raw.githubusercontent.com/g3anmart1ns/geanforge/main/geanforge \
  | sudo tee /usr/local/bin/geanforge > /dev/null
sudo chmod +x /usr/local/bin/geanforge
```

### Package Managers

**Debian/Ubuntu:**
```bash
# Install dependencies
sudo apt install apache2-utils whois

# Install geanforge
sudo curl -fsSL https://raw.githubusercontent.com/g3anmart1ns/geanforge/main/geanforge \
  -o /usr/local/bin/geanforge
sudo chmod +x /usr/local/bin/geanforge
```

**RHEL/CentOS/Fedora:**
```bash
# Install dependencies
sudo dnf install httpd-tools whois

# Install geanforge
sudo curl -fsSL https://raw.githubusercontent.com/g3anmart1ns/geanforge/main/geanforge \
  -o /usr/local/bin/geanforge
sudo chmod +x /usr/local/bin/geanforge
```

**macOS (Homebrew):**
```bash
# Install dependencies
brew install httpd coreutils

# Install geanforge
curl -fsSL https://raw.githubusercontent.com/g3anmart1ns/geanforge/main/geanforge \
  | sudo tee /usr/local/bin/geanforge > /dev/null
sudo chmod +x /usr/local/bin/geanforge
```

### Manual Installation

```bash
git clone https://github.com/g3anmart1ns/geanforge.git
cd geanforge
sudo install -m 755 geanforge /usr/local/bin/
```

## ⚡ Quick Start

```bash
# Generate a 20-character secure password
geanforge

# Generate a 32-character password with base64 encoding (K8s secrets)
geanforge -l 32 -b

# Generate a bcrypt hash with cost 12
geanforge -m bcrypt --cost 12 -S

# Generate a 6-digit PIN
geanforge -p 6

# Generate a 5-word Diceware passphrase
geanforge -w 5

# Generate 10 passwords and save to file
geanforge -n 10 -l 24 -f passwords.txt
```

## 📖 Usage

```
geanforge [OPTIONS]
```

### Main Options

| Flag | Long | Description | Default |
|------|------|-------------|---------|
| `-l` | `--length N` | Password length | `20` |
| `-n` | `--count N` | Number of passwords | `1` |
| `-m` | `--method METHOD` | Generation method | `random` |

**Available methods:** `random`, `openssl-rand`, `mkpasswd`, `openssl`, `bcrypt`

### Special Modes

| Flag | Long | Description |
|------|------|-------------|
| `-p` | `--pin N` | Generate numeric PIN (1-12 digits) |
| `-w` | `--words N` | Generate Diceware passphrase |
| `-W` | `--word-sep CHAR` | Passphrase separator | `-` |
| `-b` | `--base64` | Encode output as base64 |

### Character Configuration

| Flag | Long | Description |
|------|------|-------------|
| `-s` | `--no-symbols` | Alphanumeric only |
| `-a` | `--no-ambiguous` | Remove `0,O,l,1,I` |
| `-x` | `--hexadecimal` | Hexadecimal only (`0-9,A-F`) |
| `-e` | `--entropy` | Check system entropy |

### Output Options

| Flag | Long | Description |
|------|------|-------------|
| `-S` | `--show-password` | Show plaintext with hash methods |
| `-C` | `--clipboard` | Copy to clipboard (auto-clears in 30s) |
| `-f` | `--file FILE` | Save to file (perm 600) |
| `--cost N` | Bcrypt cost factor | `10` |

### Security Options

| Flag | Long | Description |
|------|------|-------------|
| `-F` | `--strength` | Verify password strength |
| `--force-low-entropy` | Skip entropy check |

## 💡 Examples

### Basic Usage

```bash
# Default 20-character password
$ geanforge
[INFO] Generation via /dev/urandom...
k9#mP2$vL8@nQ5*wR3!x

# Strong password without ambiguous characters
$ geanforge -l 24 -a -F
[INFO] Generation via /dev/urandom...
abcdefghkmnpqrstuvwxyz2345
[WARN] Password lacks character variety - Medium

# Generate 5 passwords at once
$ geanforge -n 5 -l 16
[INFO] Generation via /dev/urandom...
--- Item 1 ---
pL7$mK2#nQ9@vR5*
--- Item 2 ---
wT8!bN4^xJ6&hM3%
...
```

### Kubernetes Integration

```bash
# Create K8s secret with base64-encoded password
kubectl create secret generic db-credentials \
  --from-literal=username=admin \
  --from-literal=password="$(geanforge -l 32 -b)"

# Generate multiple secrets and save to file
geanforge -n 5 -l 32 -b -f k8s-secrets.txt

# Create secret with literal password (not base64)
kubectl create secret generic app-secret \
  --from-literal=api-key="$(geanforge -l 40 -s)"
```

### Web Applications

```bash
# Bcrypt hash for user registration (cost 12)
$ geanforge -m bcrypt --cost 12 -S
[INFO] Bcrypt hash (cost: 12)...
Password: mK9#pL2$vN8@qR5*
$2y$12$Lxv3k9Jp2mN8qR5vT7wX9OeYzA1B2C3D4E5F6G7H8I9J0K1L2M3N4

# SHA-512 hash for /etc/shadow
$ geanforge -m mkpasswd
[INFO] SHA-512 hash (whois)...
$6$rounds=5000$salt$hash...

# htpasswd format for nginx/Apache basic auth
$ geanforge -m bcrypt --cost 10 -S | awk '{print "admin:"$1}' >> .htpasswd
```

### Passphrases & PINs

```bash
# Diceware passphrase with default separator
$ geanforge -w 5
[INFO] Generating Diceware passphrase (5 words)...
correct-horse-battery-staple-unicorn

# Passphrase with custom separator
$ geanforge -w 6 -W "_"
correct_horse_battery_staple_unicorn_dragon

# Base64-encoded passphrase for config files
$ geanforge -w 4 -b
Y29ycmVjdC1ob3JzZS1iYXR0ZXJ5LXN0YXBsZQ==

# 6-digit PIN for 2FA/banking
$ geanforge -p 6
[INFO] Generating PIN of 6 digits...
739284

# 4-digit PIN
$ geanforge -p 4
[INFO] Generating PIN of 4 digits...
8291
```

### Advanced Usage

```bash
# Generate with clipboard copy and strength check
geanforge -l 32 -a -F -C

# Save to file with metadata header
geanforge -n 20 -l 24 -f production-passwords.txt

# Hexadecimal-only password
geanforge -l 40 -x

# OpenSSL generation with bias reduction
geanforge -m openssl-rand -l 32

# Force low entropy mode (for CI/CD)
FORCE_LOW_ENTROPY=1 geanforge -l 32
```

## 🔒 Security

### Cryptographic Foundation

- **Primary Source**: `/dev/urandom` (CSPRNG, non-blocking, cryptographically secure)
- **Fallback**: `openssl rand` with base64 decoding and bias reduction
- **Character Filtering**: Uses `LC_ALL=C tr` to avoid locale-related multibyte issues
- **PIN Generation**: Avoids leading zeros to prevent octal interpretation in legacy systems

### Entropy Validation

On Linux systems, `geanforge` checks `/proc/sys/kernel/random/entropy_avail`:

```bash
# Enable entropy check
geanforge -l 32 -e

# Skip entropy check in CI/CD
FORCE_LOW_ENTROPY=1 geanforge -l 32
```

> **Note**: Modern Linux kernels (≥5.6) use ChaCha20-based CSPRNG, making `entropy_avail` less critical. The check is informational for older systems or freshly booted VMs.

### Memory Safety

- Sensitive variables are unset on exit via `trap`
- Bash history is cleared in interactive sessions
- Clipboard is auto-cleared after 30 seconds (best-effort)
- File permissions set to `600` (owner read/write only)

### Known Limitations

- **Memory Cleanup**: Bash cannot securely wipe memory; variables may persist in memory pages until GC
- **Clipboard Clearing**: Background cleanup may fail if terminal closes (SIGHUP)
- **Bcrypt Format**: Uses `$2y$` (Apache-compatible), not `$2b$`. Both are widely supported

## 🔧 Dependencies

### Required

- `bash` ≥ 4.0
- `coreutils` (`tr`, `head`, `wc`, `cut`, `paste`, `shuf`)
- `openssl`

### Optional (for specific methods)

| Tool | Package (Debian) | Package (RHEL) | Purpose |
|------|------------------|----------------|---------|
| `htpasswd` | `apache2-utils` | `httpd-tools` | Bcrypt hashing |
| `mkpasswd` | `whois` | `expect` | SHA-512 hashing |
| `cracklib-check` | `libcrack2` | `cracklib` | Strength verification |
| `xclip` | `xclip` | `xclip` | X11 clipboard |
| `wl-clipboard` | `wl-clipboard` | `wl-clipboard` | Wayland clipboard |
| `haveged` | `haveged` | `haveged` | Entropy daemon |

## 🧪 Testing

```bash
# Run basic tests
bash tests/test_basic.sh

# Run all tests
bash tests/test_all.sh

# Test specific functionality
geanforge -l 32 -b | base64 -d | wc -c  # Should output 32
```

## 🤝 Contributing

Contributions are welcome! Here's how to get started:

1. **Fork** the repository
2. **Create** a feature branch: `git checkout -b feature/amazing-feature`
3. **Commit** your changes: `git commit -m 'Add amazing feature'`
4. **Push** to the branch: `git push origin feature/amazing-feature`
5. **Open** a Pull Request

### Development Setup

```bash
git clone https://github.com/g3anmart1ns/geanforge.git
cd geanforge
./geanforge --help
```

### Code Style

- Use `shellcheck` for linting: `shellcheck geanforge`
- Follow [Google Shell Style Guide](https://google.github.io/styleguide/shellguide.html)
- Add comments for complex logic
- Keep functions small and focused

## 📊 Roadmap

- [ ] Package for Homebrew core
- [ ] Package for Debian/Ubuntu repositories
- [ ] Docker image with all dependencies
- [ ] Windows support via WSL2/Cygwin
- [ ] GUI wrapper (maybe?)
- [ ] Zsh/Fish completions
- [ ] Man page

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

```
MIT License

Copyright (c) 2026 Gean Martins

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

## 👤 Author

**Gean Martins**

- GitHub: [@g3anmart1ns](https://github.com/geanmartins)
- Project: [geanforge](https://github.com/g3anmart1ns/geanforge)

## 🙏 Acknowledgments

- Inspired by `pwgen`, `apg`, and `openssl rand`
- Diceware passphrase concept by Arnold Reinhold
- The open-source security community

---

<p align="center">
  Made with 🔒 by <a href="https://github.com/g3anmart1ns">Gean Martins</a><br>
  <sub>If this project helped you, consider giving it a ⭐️ on GitHub!</sub>
</p>
```
