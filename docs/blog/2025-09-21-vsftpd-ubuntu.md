# SFTP setup on Ubuntu

Installation and setup of VSDTPD in Ubuntu Server 24.04.3 LTS with:

- dedicated user for FTP (replace _USERNAME_ placeholder),
- and SSH user has limited access to server (specific directory and only SFTP/FTP).

## SSH setup - limitation of access for SFTP and directories (sshd_config)

Using command `sudo vi /etc/ssh/sshd_config` edit configuration - add this:

```
Match User USERNAME
        AllowTcpForwarding no
        X11Forwarding no
        ChrootDirectory /srv/ftp/uploads
        ForceCommand internal-sftp
```

A **ChrootDirectory** limits access for SSH to specific directory.

Notes:

- **IMPORTANT:** `/srv/ftp/uploads` folder needs to be "owned" by root
    - `sudo chown root:root /srv/ftp/uploads`
    - create folder with USER access in `uploads` folder
- Access is limited to SFTP via **ForceCommand**
- **Restart SSH service** after changes in configuration.

## Users (Linux/SSH)

Create user:

`sudo useradd -d /srv/ftp/uploads USERNAME`

## vsftpd

Install VSFTPD (Very Secure FTP Deamon):

`sudo apt install vsftpd`

Edit configuration (vsftpd.confg):

`sudo vi /etc/vsftpd.conf`

Add (uncomment) to vstftp.config:

`chroot_local_user=YES`

Restart a VSFTPD service:

`sudo systemctl restart vsftpd`
