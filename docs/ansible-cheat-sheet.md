# Ansible Cheat Sheet

## Running playbooks

### Running all playbooks for configured hosts

```sh
ansible-playbook {playbook}.yml -b -K
```

- `{playbook}.yml`: runs specified playbook on configured hosts (e.g. `nas.yml`)
- `-b`, `--become`: (Privilege escalation option) run operations with become (does not imply password prompting)
- `-K`, `--ask-become-pass`: (Privilege escalation option) ask for privilege escalation password

### Running tagged playbooks for configured hosts

```sh
ansible-playbook {playbook}.yml -b -K --tags {tag}
```

- `{tag}`: runs specific tasks based on `{playbook}.yml`'s role tags

### Dry-run (incl. most commonly used options)

```sh
ansible-playbook {playbook}.yml -b -K -C -D -v
```

- `-C`, `--check`: don't make any changes; instead, try to predict some of the changes that may occur
- `-D`, `--diff`: when changing (small) files and templates, show the differences in those files; works great with --check
- `-v[vvvvv]`: causes Ansible to print more debug messages (adding multiple `-v` will increase verbosity up to 6 `v`'s max)

## Encrypting/decryping vault secrets

> [!NOTE]
> Make sure that you have your vault password specified correctly!
> By default, if you specify a `vault_password_file` in `ansible.cfg`, it will use it by default.

### Encrypting a string

```sh
ansible-vault encrypt_string '{string_to_encrypt}' --name {name_of_secret}
```

### Decrypting a secret

```sh
echo -e '{encrypted_string}' | ansible-vault decrypt
```

> [!WARNING]
> Formatting needs to be `$ANSIBLE_VAULT;1.1;AES256\n...\n...\n...` where the newlines are included as `\n`.
