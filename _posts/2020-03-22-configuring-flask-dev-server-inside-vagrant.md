---
layout: post
title:  "Configuring Flask dev server inside Vagrant"
date:   2020-03-22 12:36:00 +0200
categories: programming
tags: [vagrant, flask, nginx]
---

In the [previous post]({% post_url 2020-03-21-learning-vagrant %}) I set up a Vagrant box. This follow-up post documents how to create a simple Flask development server inside the Vagrant box. For reference, I read instructions from
[Vultr](https://www.vultr.com/docs/setup-django-on-debian-8) and
[Digitalocean](https://www.digitalocean.com/community/tutorials/how-to-serve-flask-applications-with-gunicorn-and-nginx-on-ubuntu-18-04) to get things rolling.

This is not overly complicated but there are quite a few moving parts. So I document this in sections, giving references that helped me understand how things work. I am starting from inside out:

Flask APP --> Gunicorn server --> (local proxy) --> Nginx server --> (expose to host computer).

##### Install some prerequisites

At minimum, install Python virtual environment module and Nginx server.

```
sudo apt-get install python3-venv nginx

# Additionally also database to actually do stuff:
sudo apt-get install postgresql
```


##### Create Flask app

Let's start small by creating a Flask App in a Python virtual environment first. I'll skip the normal version control parts here. In guest:
```
# In /home/vagrant/webapp
# Create and enter virtual environment using Python's own module
python3 -m venv venv
source venv/bin/activate

# Install Flask
pip install Flask
```

Create a very simple Flask app in file **myapp.py**:
```
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello_world():
    return 'Hello from Vagrant box!'
```

Set Flask environment variables and check that the app runs

```
export FLASK_APP=myapp.py
export FLASK_ENV=development
flask run
```

##### Use Gunicorn to serve the Flask app

We actually need two server components here. Nginx is going to face the outside world, to serve static content and request dynamic content from the Flask app. For this dynamic part, we also need Gunicorn to bridge Nginx and Flask. Better explanation is available in 
[this excellent serverfault thread](https://serverfault.com/questions/331256/why-do-i-need-nginx-and-something-like-gunicorn). Gunicorn configuration instructions are also available from [Flask documentation](https://flask.palletsprojects.com/en/1.1.x/deploying/wsgi-standalone/#uwsgi).

Create an entry point for Gunicorn in file **wsgi.app**:

```
from myapp import app

if __name__ == "__main__":
    app.run()
```

Install Gunicorn:

```
# Install Gunicorn
pip install gunicorn
```

Create Gunicorn configuration in **/etc/systemd/system/myapp.service** and bind the Gunicorn to local port 8000.

```
[Unit]
Description=Gunicorn instance to serve myapp
After=network.target

[Service]
User=vagrant
Group=vagrant
WorkingDirectory=/home/vagrant/webapp
Environment="PATH=/home/vagrant/webapp/venv/bin"
ExecStart=/home/vagrant/webapp/venv/bin/gunicorn --workers 3 --bind 127.0.0.1:8000 wsgi:app

[Install]
WantedBy=multi-user.target
```

Enable and start the Gunicorn service:

```
sudo systemctl enable myapp
sydo systemctl start myapp
```

Ok. Now Gunicorn is used to handle the Flask app communication to address 127.0.0.1:8000

##### Setup Nginx

By default, Vagrant only exposes ssh port 22 on guest machine and maps that to host 2222. To map the http port 80 to 8080 on the host, uncomment this in the **Vagrantfile** and restart the guest:

```
config.vm.network "forwarded_port", guest: 80, host: 8080
```

Create configuration for Nginx in **/etc/nginx/sites-available/myapp**:

```
server {
        listen 80;
        server_name localhost;
        access_log off;

# This would be our static file directory, but not enabled here yeat
#        location /static/ {
#                alias /home/vagrant/webapp/static/;
#        }

        location / {
                proxy_pass http://127.0.0.1:8000;
        }
}
```

Symlink this to enabled services:

```
ln -s /etc/nginx/sites-available/myapp /etc/nginx/sites-enabled/myapp
```

Enable and start Nginx:

```
sudo systemctl enable nginx
sudo systemctl start nginx
```

##### Ensure it's working

```
sudo systemctl status nginx
```

Now the server content can be accessed from host computer's address localhost:8080

It might be necessary to restart Gunicorn/Nginx right after this installation to get it working but everything should be smooth after that.

-----------------

##### Edit 1.4.2020: Set up remote editing and a small fix:

**To enable remote editing**:

By default, Vagrant mounts host root directory in guest /vagrant mountpoint. To move app there and create a symbolic link in guest:

```
mv /home/vagrant/webapp /vagrant
ln -s /vagrant/webapp /home/vagrant/webapp
```

And for the development environment, it's a good idea to make Gunicorn observe file changes and reload on each change. This can be done by adding **--reload** parameter to gunicorn startup command in guest /etc/

```
...
ExecStart=/home/vagrant/webapp/venv/bin/gunicorn --reload --workers 3 --bind 127.0.0.1:8000 wsgi:app
...
```

However, the mounting of /vagrant happens **after** Gunicorn startup so there is nothing to be found for Gunicorn to serve and app won't work. Add a simple trigger to the end of Vagrantfile:

```
config.trigger.after :up do |trigger|
    trigger.name = "Gunicorn restarter trigger"
    trigger.info = "Restarting Gunicorn service after mountpoints attached."
    trigger.run_remote = {inline: "systemctl restart myapp"}
end
```

Now the webapp is exposed to the host system in the vagrant root directory and any edits will trigger reload and show on http://127.0.0.1

**Also: remove Nginx default profile**

I am getting inconsistent behaviour, but it might be necessary to remove the Nginx default profile by unlinking the file in */etc/nginx/sites-enabled/default*. This is a good idea anyway so that two URLs don't conflict. For some weird reason it worked fine for a while.

