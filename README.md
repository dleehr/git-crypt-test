git-crypt-test
==============

This repo is used for exploring git-crypt

The `secret.yml` file is encrypted with git-crypt. To unlock this file, a GPG key must be added to the repo:

```
git-crypt add-gpg-user --trusted <key-id>
```

The repo can be cloned as usual, but the contents of `secret.yml` will not be visible until running

```
git-crypt unlock
```

And allowing your GPG key to decrypt the file.

The `.gitattributes` file is what specifies which files are encrypted. This guarantees that plaintext versions are not checked into git.


