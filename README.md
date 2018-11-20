# git-remote-crypt

git-remote-crypt is a [git remote helper](https://git-scm.com/docs/git-remote-helpers), that is used to encrypt/decrypt git repository.

## Installation

requires:

```
bash version >= 4.0 is required
```

linux:

```
wget https://raw.github.com/xnil/git-remote-crypt/master/git-remote-crypt -O /usr/local/bin/git-remote-crypt
chmod +x /usr/local/bin/git-remote-crypt
```

windows (assume MinGW installed):

```
curl https://raw.github.com/xnil/git-remote-crypt/master/git-remote-crypt -O ~/bin/git-remote-crypt
chmod +x ~/bin/git-remote-crypt
```

## Quick start

1. echo "your secret key" > ~/.git-crypt-key
2. cd /path/to/exist/local/repository
3. git remote add backup crypt::https://github.com/your-account/your-secret-repository.git
4. git push backup master

## Customize encryptor/decryptor

By default git-remote-crypt uses aes to encrypt/decrypt data:

```
_default_encryptor() {
	openssl enc -aes-256-cbc -pass pass:$GIT_CRYPT_KEY
}

_default_decryptor() {
	openssl enc -aes-256-cbc -d -pass pass:$GIT_CRYPT_KEY
}
```

You can choose another, like:

```
  cat << EOF > ~/my-git-crypt-encryptor
#!/bin/bash
openssl enc -des-cfb -pass pass:mysecret
EOF
  chmod +x ~/my-git-crypt-encryptor

  cat << EOF > ~/my-git-crypt-decryptor
#!/bin/bash
openssl enc -des-cfb -d -pass pass:mysecret
EOF
  chmod +x ~/my-git-crypt-decryptor

  export GIT_CRYPT_ENCRYPTOR=~/my-git-crypt-encryptor
  export GIT_CRYPT_DECRYPTOR=~/my-git-crypt-decryptor
```

## Environment variables

| name                | desc            |
| ------------------- | --------------- |
| GIT_CRYPT_KEY       | secret key      |
| GIT_CRYPT_KEY_FILE  | secret key file |
| GIT_CRYPT_ENCRYPTOR | encrypt program |
| GIT_CRYPT_DECRYPTOR | decrypt program |

## License

Copyright (c) 2018 git-remote-crypt author (bpskdc@gmail.com). See [LICENSE](LICENSE) for details.
