# Importing GPG Keys

[Source](https://unix.stackexchange.com/questions/184947/how-to-import-secret-gpg-key-copied-from-one-machine-to-another)

To import both public and private GPG keys:

```shell
gpg --import /path/to/publickey.asc
gpg --import /path/to/privatekey.asc
```

## Telling git to use GPG for signing

[Source](https://docs.github.com/en/authentication/managing-commit-signature-verification/telling-git-about-your-signing-key)

First, make sure a general git config is set:

```shell
git config --global user.email "your.email@example.com"
git config --global user.name "yourUsername"
```

Then tell git to use gpg keys for signing commits globally:

```shell
git config --global --unset gpg.format
git config --global user.signingkey 3AA5C34371567BD2 #<- replace with actual key id
git config --global commit.gpgsign true
git config --global tag.gpgSign true
```
