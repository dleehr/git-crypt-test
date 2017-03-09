git-crypt-test
==============

This repo is used for exploring git-crypt

git-crypt relies on [gitattributes](https://git-scm.com/docs/gitattributes) to mark files (or file patterns) for processing through a filter on commit, checkout, or diff. It allows the encryption to work transparently

In this repo, the `.gitattributes` file specifies one rule:

```
secret.yml filter=git-crypt diff=git-crypt
```

Anyone may clone the repo, and `secrets.yml` file will be unreadable. To view it, you must `git-crypt unlock` the repo. Doing so requires either:

- The symmetric key for the repo (created by running `git-crypt init` when first configuring the repo for git-crypt)
- A GPG key corresponding to one of the encrypted public keys added to the repo (preferred)

To add a user's GPG key to the repo, use:

```
git-crypt add-gpg-user <key-id>
```

Note, that adding users depends on the key's trust in your local keyring. See https://www.gnupg.org/gph/en/manual/x334.html

GPG keys can only be added to an unlocked repo. Adding a GPG user will result in a commit to the repo with their encrypted key added to the `.git-crypt/keys/` directory.


Notes
=====

Some advantages over ansible-vault:

- Uses GPG keys instead of sharing a password
- encrypt any kind of file, not just ansible structured data
- .gitattributes reduces likelihood of committing sensitive data, also provides local diffs

There are [ways to make ansible-vault work with .gitattributes](https://leucos.github.io/articles/transparent-vault-revisited/), but they're hacky. Vault wasn't built with .gitattributes in mind - it works on files in-place and won't work with stdin/stdout.

One big advantage to ansible vault is that the user must unlock the vault before the playbook runs. And since it won't encrypt arbitrary blobs of data, it won't let you place an encrypted file on a host by accident.
