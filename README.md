This is a collection of code snippets

### Installing Oh-My-Zsh
```
sudo apt-get install zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
chsh -s /bin/zsh
```


### Installing a .deb file via terminal
 ```
 sudo dpkg -i path/to/the/file
 ```
### Install node js
```
sudo apt-get install nodejs-legacy
sudo apt-get install npm
```

### Uninstalling a package
####  This removes  all but config files
```
sudo apt-get remove <package-name>
```
#### This removes everything
```
sudo apt-get purge <package-name>
```

### Getting started with vue.js
#### Using the CDN
```
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.13/dist/vue.js"></script>
```



### Start your first react app
```
npm install create-react-app
create-react-app app-name
cd app-name
npm start
```



### Setup mongodb on ubuntu
```
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv EA312927

echo "deb http://repo.mongodb.org/apt/ubuntu "$(lsb_release -sc)"/mongodb-org/3.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.2.list

sudo apt-get update

sudo apt-get install -y mongodb-org
```

We have to create a new mongodb systemd service file in the `/lib/systemd/system` directory. Go to that directory and create the new mongodb service file `mongod.service` with nano.

```
sudo nano /etc/systemd/system/mongodb.service
```
Paste the script below
```
    [Unit]
    Description=High-performance, schema-free document-oriented database
    After=network.target

    [Service]
    User=mongodb
    ExecStart=/usr/bin/mongod --quiet --config /etc/mongod.conf

    [Install]
    WantedBy=multi-user.target
```
Now update the systemd service and start mongodb:

```
systemctl daemon-reload
systemctl start mongod
systemctl enable mongod
```


### Setting up nginx
```
sudo apt-get install nginx nginx-full nginx-common
```
then reboot system
Check the firewall and allow nginx HTTP
```
sudo ufw app list
sudo ufw allow 'Nginx HTTP'
sudo ufw status
```
If ufw is inactive enable it
```
sudo ufw enable
```

Whenever you create a new server, just add them into the conf.d directory `/etc/nginx/conf.d/`
```
sudo nano /etc/nginx/conf.d/myWebsite.d
```
Put a specific server config in that file
```
server {
    listen 8080;
    server_name localhost;
    location / {
        root    /path/to/the/folder/;
        index   index.html index.htm;
    }

    #redirect server error pages to the static pages /50x.html
    error_page 500 502 503 504  /50x.html;
    location = /50x.html{
        root html;
    }
}
```

### Setting up ruby on rails
#### Install rvm and ruby first
```
sudo apt-get install software-properties-common
sudo apt-add-repository -y ppa:rael-gc/rvm
sudo apt-get update
sudo apt-get install rvm
rvm install ruby
```
#### Install rails using gem 
```
gem install rails
```
#### Create an app 
```
rails new <app-name>
```


### Deploying a python telegram bot in heroku
`https://hackernoon.com/how-to-create-and-deploy-a-telegram-bot-2addd8aec6b4`
this is a very nice article about deployment

navigate to the project folder
```
pip freeze > requirements.txt
```
now create a `Procfile`  ( but for windows it is `Procfile.windows`)
it should contain 
```
web: python my_bot_name.py
```
Also add an __init__.py to the project folder. It can be empty
Push the project to a git repository
Install Heroku CLI
```
npm install heroku -g
```
To verify the installation, try `heroku --version`
Create an account in `heroku.com`
Now follow these steps
```
heroku login
heroku create
git push heroku master
heroku ps:scale web=1
heroku open
```
Now your project runs on heroku server
To check the logs
```
heroku logs --trail
```



### port forwarding using nginx
Create a new config file inside `/etc/nginx/sites-available/`
if the name of th config file is `app`
Then the config file will be
`/etc/nginx/sites-available/app`
```
server{
    listen 80;
    server_name domain.com
    location / {
        proxy_pass "http://0.0.0.0:8080"
    }
}
```
Also create a soft link to `/etc/nginx/sites-enabled/` like:
```
sudo ln /etc/nginx/sites-available/app /etc/nginx/sites-enabled/app
```
Delete any unwanted files inside the `sites-available` and `sites-enabled` folders


If you want to use the forwarded hostname as the proxy header
Add this inside the `location / { .... }`
```
proxy_redirect http://kutt.com/ http://0.0.0.0:8090;
proxy_set_header Host $host;
```

### Backup and Restore postgres database
#### Backup
```
pg_dump <db-name> > output-file.bak
```
If it is from docker container
```
sudo docker container ls    // to get container id
sudo docker exec -it <container-id> pg_dump -U <db-user> <db-name> > bkup.bak
```
#### Restore
Drop the database if it exists and create a new one
```
psql <db-name> < output-file.bak
```
