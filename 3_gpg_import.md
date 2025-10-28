# Importing GPG Keys

To import both public and private GPG keys:

```shell
gpg --import /path/to/publickey.asc
gpg --import /path/to/privatekey.asc
```

## Telling git to use GPG for signing

```shell
git config --global user.signingkey YOUR_KEY_ID
git config --global commit.gpgsign true
git config --global gpg.program gpg
git config --global tag.gpgsign true
```
