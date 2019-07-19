# Provision LDAP mail users and distribution groups
The LDAP mail roles in this Playbook provision both LDAP mail users and distribution groups. 

### Prepare
Create the ldap virtualenv.

`mkvirtualenv ldap; pip install -r requirements.txt`

### LDAP Mail accounts
**Provision LDAP mail account and send welcome email**

`ansible-playbook --check --diff --ask-vault-pass main.yml --tags "mailuser,email"`

**Run Playbook but do not include welcome email**

`ansible-playbook --check --diff --ask-vault-pass main.yml --tags "mailuser"`

**Run Playbook but only send welcome email**

`ansible-playbook --check --diff main.yml --tags "email,welcome"`

**Pass basic info with variables**

`ansible-playbook ... --extra-vars "firstname=Jon lastname=Doe ldap_mail_username=jdoe ldap_mail_userdomain=example.email"`

### LDAP Mail distribution groups
**Provision LDAP mail group**

`ansible-playbook --check --diff main.yml --ask-vault-pass --tags "mailgroup" --extra-vars "mailgroup=mygroup groupdomain=example.email"`

Mail distribution group members are defined by the mail user's email address within the `mailGroupMember` attribute, respectively. Mail distribution groups can also be nested group members.

### LDAP
This Playbook makes use of Ansible Vault to encrypt and store the LDAP admin password string in a variable named `ldap_admin_password`

`ansible-vault encrypt_string 'secretpw' --name 'ldap_admin_password'`

The supported LDAP mail attributes provisioned with this Playbook require [postfix-book.schema](https://github.com/variablenix/ldap-mail-schema/blob/master/postfix-book.schema) to be loaded with an LDAP backend server such as OpenLDAP.

|       objectClass      | attributes               |
|:----------------------:|--------------------------|
| PostfixBookMailAccount | `mail`                   |
| PostfixBookMailForward | `mailAlias`              |
|                        | `mailHomeDirectory`      |
|                        | `mailStorageDirectory`   |
|                        | `mailUidNumber`          |
|                        | `mailGidNumber`          |
|                        | `mailEnabled`            |
|                        | `mailGroupMember`        |
|                        | `mailQuota`              |
|                        | `mailSieveRuleSource`    |
|                        | `mailForwardingAddress`  |
