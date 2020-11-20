# Linux
### Environmental Variables
#### Article
- https://www.digitalocean.com/community/tutorials/how-to-read-and-set-environmental-and-shell-variables-on-linux
- https://stackoverflow.com/questions/39296472/how-to-check-if-an-environment-variable-exists-and-get-its-value
- https://unix.stackexchange.com/questions/182032/zip-the-contents-of-a-folder-without-including-the-folder-itself

##### Create Environmental Variables
```
export NEW_VAR="Testing export"
```

##### Check if an environmental variable exists and get its value
```
if [[ -z "${DEPLOY_ENV}" ]]; then
  MY_SCRIPT_VARIABLE="Some default value because DEPLOY_ENV is undefined"
else
  MY_SCRIPT_VARIABLE="${DEPLOY_ENV}"
fi

# or using a short-hand version

[[ -z "${DEPLOY_ENV}" ]] && MyVar='default' || MyVar="${DEPLOY_ENV}"

# or even shorter use

MyVar="${DEPLOY_ENV:-default_value}"
```

##### Zip the content of a folder without including the folder itself
The most effective way is to go inside the folder to do the zip
```
cd folder; zip -r zipFile.zip ./*
```

# GoCD
### Execute a shell script in Go CD

For windows, in git we need to mark it as executable
```
git update-index --chmod=+x foo.sh
```
In GoCD
```
some_name: &some_name
  - exec:
      command: "sh"
      arguments:
        - "-c"
        - "etc/script/foo.sh"
```
