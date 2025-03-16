# Ansible Role: certbot_cloudflare

![Ansible](https://img.shields.io/badge/ansible-ready-blue.svg)
![Platform](https://img.shields.io/badge/platform-Debian%2FUbuntu-lightgrey)
![License](https://img.shields.io/badge/license-MIT-green)

## Description

This Ansible role obtains and renews **Let's Encrypt SSL certificates** using **Certbot** and the **Cloudflare DNS challenge**.  
It is designed to be **webserver-agnostic**, making it usable with **nginx**, **apache2**, or any custom web server setup.

### Features:
- Fully supports **Debian-family systems** (Ubuntu, Debian)
- Webserver-agnostic: Works with **nginx**, **apache2**, or custom servers
- Uses **DNS-01 Challenge via Cloudflare** (no need for port 80/443 open during issuance)
- Automatic certificate renewal via **cronjob**
- **Deploy Hook** configurable to reload your web server
- Includes **Sanity Checks** to verify certificate presence

---

## Supported Platforms

Any APT-based Debian-family system (Ubuntu, Debian).

---

## Role Variables

| Variable                                              | Default Value                     | Description                                                                 |
|-------------------------------------------------------|-----------------------------------|-----------------------------------------------------------------------------|
| `ansible_role_certbot_cloudflare_domain`              | `example.com`                     | Primary domain name for the certificate                                      |
| `ansible_role_certbot_cloudflare_email`               | `admin@example.com`               | Email address for Let's Encrypt registration                                 |
| `ansible_role_certbot_cloudflare_api_token`           | `YOUR_CLOUDFLARE_API_TOKEN`        | Cloudflare API token (should be encrypted with Ansible Vault)                |
| `ansible_role_certbot_cloudflare_deploy_hook_command` | `systemctl reload nginx`          | Command to reload/restart web server after renewal                           |
| `ansible_role_certbot_cloudflare_credentials_file`    | `/etc/letsencrypt/cloudflare.ini` | Path to store Cloudflare API token credentials file                          |

> **Note:**  
> The role is strictly webserver-neutral. Only Certbot + Cloudflare + certificate management handled.

---

## Usage Example

```yaml
- name: Obtain SSL certificates via Certbot & Cloudflare
  hosts: all
  become: true
  roles:
    - role: ansible_role_certbot_cloudflare
      vars:
        ansible_role_certbot_cloudflare_domain: "yourdomain.com"
        ansible_role_certbot_cloudflare_email: "admin@yourdomain.com"
        ansible_role_certbot_cloudflare_api_token: "{{ vault_cloudflare_api_token }}"
        ansible_role_certbot_cloudflare_deploy_hook_command: "systemctl reload apache2"
```

---

## Tags

| Tag          | Purpose                                |
|--------------|----------------------------------------|
| certbot      | Install and configure Certbot          |
| cloudflare   | DNS-01 Challenge via Cloudflare        |
| ssl          | Manage SSL/TLS certificates            |
| letsencrypt  | Obtain free Let's Encrypt certificates |

---

## Sanity Checks

The role performs:

- Check if certificate exists at `/etc/letsencrypt/live/{{ domain }}/fullchain.pem`
- Verifies renewal cronjob presence
- Ensures Cloudflare credentials file is correctly created

---

## License

MIT

---

## Author Information

Role maintained by **Max Bergmann**.  
Feel free to contribute, suggest improvements, or raise issues!
