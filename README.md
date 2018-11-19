#### Deployment scripts for buildandtell online services
In the `hosts` file, there two sets of server groups:
- `[mastodon_servers]`
- `[main_servers]`

A set of playbooks to deploy the following services:
- Mastodon `[mastodon_servers]`
- Discourse `[main_servers]`
- Official wiki (DockuWiki) `[main_servers]`
- Homepage `[main_servers]`
- Custom Wikis (DokuWiki) `[main_servers]`
- Monitoring services `[main_servers]`

We plan to run our services free of charge,
With AWS Free tire, we can run one `t2.micro` EC2 instance`(30GB SSD+1GB Memory)` for one year.
Because the 1GB memory is not enough to run all of our services we need to run **two** servers
and we switch to a new AWS Free tire plan every 6th month. With these playbooks and backup scripts
that should not be much of a hassle.

### Roles installation
```
$ ansible-playbook install -r requirements.yml
```

### Playbook usage
Three primary playbooks:
- `./custom_wiki.yml` : for deploying custom wikis for educational orgs.
- `./mastodon.yml` : for deploying the mastodon instance
- `./server.yml`: for deploying all other services

**Examples:**
```
$ ansible-playbook mastodon.yml
```
- in mastodon, add the .env.production file as a file in ansible and run the compile rake command from ansible
- also add the letsencrypt process in it so that there's 0 manual work
- also add proper notify handlers
- use treliss handler technique to get handlers from othr roles

## Backup
As we do not have infrastructure for a dedicated backup server, I wrote a few
scripts to rsync databases and media files. It sets up cron service in your local machine
that does daily backups.

> **Note:** You will want to make sure that Mastodon is not running during the backup/restore process. if still running and the database might have changes which haven’t been written to disk yet. Therefore we quickly stop the container.

https://wiki.postgresql.org/wiki/Automated_Backup_on_Linux

On any supported machine, run the scripts individually and it will start backing up automatically.

### Items to backup
- Mastodon
    - PostgreSQL database
    - User generated content (images, avatars, headers)
    - Mastodon application secrets ( one time )
- Discourse 
- DokuWiki 

### Backup restoration
- Mastodon
- Discourse
- DokuWiki

---

### Need a new service / found a bug / have a better way of doing things?
Feel free to create issues and send PRs.



## Notes
- add a desable PasswordAuthentication to the commons role
- settig u ntp 
