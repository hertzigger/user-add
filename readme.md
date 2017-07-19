# Install Monit

## Description

This is an ansible project built as a flexible way to add a user from a playbook or another role.

## Dependency's

None

## Supported OS

centos

## Example Usage

The idea behind the role is to be able to add users to hte system in a variety of way.

I advise creating a new main.yml in vars/ and copying defaults from defaults/main.yml. This way you can maintain public/private keys for your developers/sservices in one place.

### Playbook

Add a user as a sudo user to only specific server or server groups, This will also add their public key to authorised_keys
```yaml
- hosts: web
  become: true
  vars:
    user_add_public_keys:
      jonathan: "public-key"
    username: jonathan
    user_add_user_groups: wheel
    user_add_generate_ssh_key: no
    authorised_keys:
      - jonathan
  roles:
    - role: user-add
```

Add a user and generate a new key
```yaml
- hosts: web
  become: true
  vars:
    user_add_public_keys:
      jonathan: "public-key"
    username: jon
    user_add_user_groups: wheel
    user_add_generate_ssh_key: yes
    authorised_keys:
      - jonathan
  roles:
    - role: user-add
```

Add a user and copy their private and public key to the server
```yaml
- hosts: web
  become: true
  vars:
    user_add_public_keys:
      jonathan: "public-key"
    user_add_private_keys:
      jonathan: "private-key"
    username: jon
    user_add_user_groups: wheel
    user_add_generate_ssh_key: no
    user_add_user_ssh_key: jonathan
    authorised_keys:
      - jonathan
  roles:
    - role: user-add
```

### Include Role

```yaml
- include_role:
    name: user-add
  vars:
    user_add_username: nginx
    user_add_user_groups: nginx
    user_add_generate_ssh_key: no
```