# Git

## basics

### set username and email

source:

  - [[GitHub] Setting your username in Git](https://docs.github.com/en/get-started/getting-started-with-git/setting-your-username-in-git)
  - [[GitHub] Setting your commit email address](https://docs.github.com/en/account-and-profile/setting-up-and-managing-your-personal-account-on-github/managing-email-preferences/setting-your-commit-email-address)

---

- to setup your username and email globally use the following commands

``` { .sh }
git config --global user.name "Max Mustermann"
git config --global user.email "max.mustermann@muster.de"
```

- to setup your username and email for a specific repository use the following commands

``` { .sh }
cd <path-to-your-repo>/
git config user.name "Max Mustermann"
git config user.email "max.mustermann@muster.de"
```

### store temporarily passwords

source: [[git] git-credential-cache](https://git-scm.com/docs/git-credential-cache)

- use the `git-credential-cache` helper

``` { .sh }
git config credential.helper 'cache --timeout=28800' # 8 hours
```

### rename GitHub branch

source: [[GitHub] Renaming a branch](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-branches-in-your-repository/renaming-a-branch)

---

- if you need to rename your branch execute the following commands:

``` { .sh }
git branch -m OLD-BRANCH-NAME NEW-BRANCH-NAME
git fetch origin
git branch -u origin/NEW-BRANCH-NAME NEW-BRANCH-NAME
git remote set-head origin -a

# remove tracking references to the old branch name
git remote prune origin
```

### git tag

source: [[Atlassian] Git tag](https://www.atlassian.com/git/tutorials/inspecting-a-repository/git-tag)

---

- if you need to create a new tag execute the following commands:

``` { .sh }
# add annotated tag with message
git tag -a v1.4 -m "my version 1.4"

# push tag to remote
git push origin v1.4
```

- if you need to retag or replace old tags execute the following command

``` { .sh }
# map commit "15027957951b64cf874c3557a0f3547bd83b3ff6" to tag v1.4 and override existing content for the v1.4 tag
git tag -a -f v1.4 15027957951b64cf874c3557a0f3547bd83b3ff6
```

### delete commit history

source: [[Xebia] How to Delete Commit History – A Step-by-Step Guide](https://xebia.com/blog/deleting-your-commit-history/)

---

- if you need to delete your commit history do the following steps

``` { .sh }
# checkout to a temp branch
git checkout --orphan temp_branch

# add all files
git add -A

# commit
git commit -m "Initial commit"

# delete old branch
git branch -D main

# rename temp branch
git branch -m main

# push
git push --force origin main
```

## encryption with transcrypt

- below is a description how to encrypt files in Git with [transcrypt](https://github.com/elasticdog/transcrypt)
- for a working example take a look at the [boilerplate](https://github.com/steled/boilerplate/tree/main/git/transcrypt) repo

### initialization

``` { .sh }
$ cd <path-to-your-repo>/
$ transcrypt
```

### full file encryption

``` { .sh }
$ cd <path-to-your-repo>/
$ echo 'sensitive content' >> sensitive_file_full
$ echo 'sensitive_file_full filter=crypt diff=crypt merge=crypt' >> .gitattributes
$ git add .gitattributes sensitive_file_full
$ git commit -m 'Add encrypted version of a sensitive file'
$ transcrypt --list
$ transcrypt --show-raw sensitive_file_full
```

### partial file encryption

- transcrypt can't encrypt files partially
- so I created a workaround based on `sed`
- the file itself is still fully encrypted but a copy of the file will be created which only redacts the sensitive parts

#### preparation

- create a file `bin\create-hook-symlinks.sh`

``` { .sh }
#!/bin/bash

for hook in "$(dirname "$0")/../githooks/"*; do
  ln -s -f "../../githooks/$(basename $hook)" "$(dirname "$0")/../.git/hooks/$(basename $hook)"
  echo -e "\n# run $(basename $hook) script" >> "$(dirname "$0")/../.git/hooks/pre-commit"
  echo "\$(dirname \"\$0\")/$(basename $hook)" >> "$(dirname "$0")/../.git/hooks/pre-commit"
done
```

- create a file `githooks/pre-commit-sed`

``` { .sh }
#!/usr/bin/env bash
# sed pre-commit hook: duplicate decrypted sensitive file and redact sensitive informations via sed

tmp=$(mktemp)
IFS=$'\n'
for secret_file in $(git -c core.quotePath=false ls-files | git -c core.quotePath=false check-attr --stdin filter | awk 'BEGIN { FS = ":" }; /crypt$/{ print $1 }'); do
  # Skip symlinks, they contain the linked target file path not plaintext
  if [[ -L $secret_file ]]; then
    continue
  fi

  # extract filename
  filename="${secret_file##*/}"
  # get file extension
  file_extension="${filename##*.}"
  # get filename without extension
  file="${filename%.*}"
  # extract directory
  dir="$(dirname ${secret_file})"

  # if test -f "${dir}/${file}.sed"; then
  if test -f "${dir}/${filename}.sed"; then
    if [ $file_extension == $file ]; then
      sed -f "${dir}/${filename}.sed" $secret_file > "${dir}/${file}_dec"
    else
      sed -f "${dir}/${filename}.sed" $secret_file > "${dir}/${file}.${file_extension}.dec"
    fi
  fi

done
rm -f "${tmp}"
unset IFS
```

- execute file `bin/create-hook-symlinks.sh`

#### usage

``` { .sh }
$ cd <path-to-your-repo>/
$ echo 'user: max mustermann' >> sensitive_file_partial
$ echo 'password: s3nsitive' >> sensitive_file_partial
$ echo 's/\(password: \).*/\1<REDACTED>/' >> sensitive_file_partial.sed
$ echo 'sensitive_file_partial filter=crypt diff=crypt merge=crypt' >> .gitattributes
$ git add .gitattributes sensitive_file_partial
$ git commit -m 'Add encrypted version of a sensitive file'
$ transcrypt --list
$ transcrypt --show-raw sensitive_file_partial
```

- after commit you should see a file `sensitive_file_partial_dec` where only the password is `<REDACTED>`
