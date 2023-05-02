# Deploy with github action to VPS

## Server
### Access to VPS server
```cmd
ssh your_vps_username@your_vps_ip_address
cd ~
```

### Create authorized_keys and copy public key allow access by ssh
```cmd
mkdir -p ~/.ssh
touch ~/.ssh/authorized_keys
cat id_rsa.pub >> authorized_keys
```

## Github repository
### Copy private key to secrets

```env
VPS_USERNAME=ssh_user
VPS_PRIVATE_KEY=id_rsa
VPS_HOST=your_host
```

## Example:

```yml
name: Deploy to VPS

on:
  push:
    branches: ['main']
  pull_request:
    branches:
      - main
jobs:
  deploy:
    runs-on: ubuntu-latest
    timeout-minutes: 3
    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Deploy code
        uses: appleboy/ssh-action@master
        with:
          host: ${{ secrets.VPS_HOST }}
          username: ${{ secrets.VPS_USERNAME }}
          key: ${{ secrets.VPS_PRIVATE_KEY }}
          port: ${{ secrets.VPS_SSH_PORT }}
          script: whoami
```
