# Laravel Dokku Deployment (Debian 9)
## Installing fresh VPS or VM instance
- GCP
- AWS EC2
- Vultr VPS

## Installing dokku

Install Dokku
```
wget https://raw.githubusercontent.com/dokku/dokku/v0.12.13/bootstrap.sh
sudo DOKKU_TAG=v0.12.13 bash bootstrap.sh
```
Copy your public key usually starts with 'ssh-rsa ..' to clipboard.
Open up browser and navigate to your VPS or VM Instance public ip.
Put your public key in the form to complete installation.

## Deployment

###### Create app in dokku

```
# on the Dokku host
dokku apps:create laravel_app
```

###### Install Dokku MySQL plugin and link to app

```
# on the Dokku host
sudo dokku plugin:install https://github.com/dokku/dokku-mysql.git mysql

dokku mysql:create laravel_db
dokku mysql:link laravel_db laravel_app
```

###### Set ENV variable

```
# on the Dokku host
dokku config:set laravel_app APP_ENV=production APP_KEY=yoursecretkeyinserthere DB_CONNECTION=mysql DB_HOST=laravel_db DB_DATABASE=laravel DB_USERNAME=root DB_PASSWORD=yoursecretpasswordinserthere
```

###### Setup PHP Buildpack

```
# on the Dokku host
dokku config:set laravel_app BUILDPACK_URL=https://github.com/heroku/heroku-buildpack-php
```

###### Clone your app from github / gitlab / bitbucket

```
# from your local machine
# SSH access to github must be enabled on this host
git clone git@github.com:heroku/laravel-app-sample.git
```

###### add remote repo

```
cd laravel-app-sample
git remote add dokku dokku@your_instance_ip:laravel-app-sample
git push dokku master
```
