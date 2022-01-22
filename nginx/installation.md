# Nginx Installation

Official docs: https://nginx.org/en/linux_packages.html

## Ubuntu

- Install the prerequisites:
  ```bash
  sudo apt install curl gnupg2 ca-certificates lsb-release ubuntu-keyring
  ```
- Import an official nginx signing key so apt could verify the packages authenticity. Fetch the key:
  ```bash
  curl https://nginx.org/keys/nginx_signing.key | gpg --dearmor \
    | sudo tee /usr/share/keyrings/nginx-archive-keyring.gpg >/dev/null
  ```
- Set up the apt repository for stable nginx packages:
  ```bash
  echo "deb [signed-by=/usr/share/keyrings/nginx-archive-keyring.gpg] \
  http://nginx.org/packages/ubuntu `lsb_release -cs` nginx" \
      | sudo tee /etc/apt/sources.list.d/nginx.list
  ```
- Set up repository pinning to prefer our packages over distribution-provided ones:
  ```bash
  echo -e "Package: *\nPin: origin nginx.org\nPin: release o=nginx\nPin-Priority: 900\n" \
    | sudo tee /etc/apt/preferences.d/99nginx
  ```
- Install nginx:
  ```bash
  sudo apt update
  sudo apt install nginx
  ```
