# Ansible Configuration for eduVPN

## Initial Setup

This folder contains configuration files for eduVPN deployment. 

**IMPORTANT**: Files already contain placeholder values. Replace them with your actual data before deployment.

### 1. Edit configuration files

**`inventories/single/hosts.ini`** - Specify target server:
```ini
[single]
vpn.example.org ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/id_rsa
```

**`group_vars/all/vars.yml`** - Deployment variables:
- `eduvpn_fqdn`: Your domain (e.g., `vpn.example.org`)
- `server_ipv4`: Server's public IP
- `public_interface_name`: Network interface (e.g., `eth0`, `ens3`)
- `certbot_admin_email`: Your email for Let's Encrypt
- `auth_module`: Authentication method (DbAuthModule, OidcAuthModule, LdapAuthModule)

**`group_vars/all/vault.yml`** - Generate your secrets:

```bash
# Node Key
openssl rand -hex 32

# WireGuard Keys
wg genkey | tee /tmp/wg.key | wg pubkey

# OIDC Secrets (if using OidcAuthModule)
pwgen -s 32 1
```

Insert generated values into the `vault.yml` file.

### 2. Encrypt the vault

```bash
ansible-vault encrypt group_vars/all/vault.yml
```

You'll be prompted for a password - remember it for future deployments!

### 3. Run the deployment

```bash
ansible-playbook playbook.yml --ask-vault-pass
```

## File Structure

```
ansible/
├── playbook.yml                     # Main playbook
├── requirements.txt                 # Python dependencies
├── requirements.yml                 # Ansible collections
├── ansible.cfg                      # Ansible configuration
│
├── inventories/
│   └── single/
│       └── hosts.ini               # Target server (edit with your data)
│
├── group_vars/
│   └── all/
│       ├── vars.yml                # General variables (edit with your data)
│       └── vault.yml               # Secrets with placeholders (generate and encrypt)
│
├── files/                           # Static files
│   ├── credentials.conf
│   └── vpn-daemon
│
└── templates/                       # Jinja2 templates
    ├── apache2/
    ├── eduvpn/
    ├── iptables/
    ├── keys/
    └── memcached/
```

## Files NOT to Commit

The following files contain sensitive data and **MUST NOT be committed** to the repository:

- `group_vars/all/vars.yml` (contains IP addresses and specific configurations)
- `group_vars/all/vault.yml` (contains secrets even if encrypted)
- `inventories/single/hosts.ini` (contains servers and credentials)

These files are already excluded by `.gitignore`.

## Useful Scripts

### Encrypt/Decrypt vault

```bash
# Encrypt vault
make encrypt
# or
ansible-vault encrypt group_vars/all/vault.yml

# Decrypt vault (to edit)
make decrypt
# or
ansible-vault decrypt group_vars/all/vault.yml

# Edit vault (automatically encrypts after)
ansible-vault edit group_vars/all/vault.yml
```

## Subsequent Deployments

After the initial deployment, you can update only specific components:

```bash
# Update only configurations
ansible-playbook playbook.yml --ask-vault-pass --tags update_config

# Update only firewall rules
ansible-playbook playbook.yml --ask-vault-pass --tags iptables

# Redistribute WireGuard keys
ansible-playbook playbook.yml --ask-vault-pass --tags wg
```

## Support

For more information, see the [main README](../README.md) or the [official eduVPN documentation](https://docs.eduvpn.org/server/v3/).
