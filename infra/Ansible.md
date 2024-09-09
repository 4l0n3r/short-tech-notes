file config:
    default
    default_file 2
    override file
    override values

Inventories in INI or YAML format
    for small no of nodes -> INI
    for many -> YAML

Verify your inventory:
    ansible-inventory -i inventory.ini --list

Ping the group:
    ansible myhosts -m ping -i inventory.ini

metagroups

create variables:
    webservers:
        vars:
            ansible_user: my_server_user