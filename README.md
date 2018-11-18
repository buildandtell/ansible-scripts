# ansible-scripts
Ansible scripts to deploy website, wiki and forum to a server

It should also have a Vagrantfile to test the whole setup on a vm

custom_wiki.yml should take variables from commandline

ansible-playbook server.yml -e env=development

- settig u ntp 
- mastodon will be one server
- discourse and

## Notes
- add a desable PasswordAuthentication to the commons role
