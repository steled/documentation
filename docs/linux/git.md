# Git

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
