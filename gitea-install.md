setup gitea server
https://linuxize.com/post/how-to-install-gitea-on-ubuntu-20-04/

1. sudo apt update
2. sudo apt install sqlite3
3. sudo apt update
4. sudo apt install git (which should already be installed by default)
5. git --version (to validate)
6. create git user
    sudo adduser \
    > --system \
    > --shell /bin/bash \
    > --gecos 'Git Version Control' \
    > --group \
    > --disabled-password \
    > --home /home/git \
    > git
7. download gitea binary
    visit https://dl.gitea.io/gitea/ and navigate to your latest version
    sudo wget -O /tmp/gitea https://dl.gitea.io/gitea/1.17.2/gitea-1.17.2-linux-amd64
8. move gitea binary to /usr/local/bin
    sudo mv /tmp/gitea /usr/local/bin
9. change permissions on gitea binary to make it executable
    sudo chmod +x /usr/local/bin/gitea
10. create directories and set necessary permissions
    sudo mkdir -p /var/lib/gitea/{custom,data,log}
    sudo chown -R git:git /var/lib/gitea/
    sudo chmod -R 750 /var/lib/gitea/
    sudo mkdir /etc/gitea
    sudo chown root:git /etc/gitea
    sudo chmod 770 /etc/gitea
    setup runner
11. create a systemd unit file - download the sample systemd unit file to the /etc/systemd/system directory
    sudo wget https://raw.githubusercontent.com/go-gitea/gitea/main/contrib/systemd/gitea.service -P /etc/systemd/system/
12. enable and start the gitea service
    sudo systemctl daemon-reload
    sudo systemctl enable --now gitea
13. verify gitea is running
    sudo systemctl status gitea
14. if you are using ufw firewall, you'll need to open the gitea port to allow traffic on port 3000
    sudo ufw allow 3000/tcp
15. navigate to your gitea server using your browser
    http://gitea-server-ip:3000
16. the gitea 'initial configuration' page will appear. make the following changes to the configuration (most are default) and click the 'install gitea' button once complete.
    Database Settings:
    - Database Type: SQLite3
    - Path: Use an absolute path, /var/lib/gitea/data/gitea.db
    Application General Settings:
    - Site Title: Enter your organization name.
    - Repository Root Path: Leave the default var/lib/gitea/data/gitea-repositories.
    - Git LFS Root Path: Leave the default /var/lib/gitea/data/lfs.
    - Run As Username: git
    - SSH Server Domain: Enter your domain or server IP address.
    - SSH Port: 22, change it if SSH is listening on other Port
    - Gitea HTTP Listen Port: 3000
    - Gitea Base URL: Use http and your domain or server IP address.
    - Log Path: Leave the default /var/lib/gitea/log
***NOTE: you can change these settings at any time by editing the gitea configuration file located in /etc/gitea/app.ini
17. once the installation is complete, you'll be redirected to the login page. you'llhave to create a new user. click the 'need an account? register now.' link and follow the prompts.
18. change the permissions of the gitea configuration file to read-only
    sudo chmod 750 /etc/gitea
    sudo chmod 640 /etc/gitea/app.ini
THAT'S IT! GITEA IS INSTALLED!