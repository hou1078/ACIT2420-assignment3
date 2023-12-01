<-------Debian 12 server setup on digitalocean------>
First create a new .ssh directory. 
mkdir .ssh

After you have created the new directory create your new ssh key pair.
ssh-keygen -t ed25519 -f .ssh/do-key -C "your-email-address"

Use the command to get your ssh pulic key
Get-Content C:\Users\user-name\.ssh\do-key.pub | Set-Clipboard (replace user-name). my username is Huawei

Go to DigitalOcean website and go into the project, you need to create one if you do not have.

Create a droplets and add ssh key

Then you will receive a IP address. My example is 147.182.206.210

Typetype command: ssh -i C:\Users\Huawei\.ssh\do-key root@147.182.206.210. For the frist time you need to type yes, when it ask you "Are you sure you want to continue connecting (yes/no/[fingerprint])?"

<-------Create a new regular user------>
sudo useradd <username>     my example is Bowen

<-------User can perform administrative tasks------>
sudo usermod -aG sudo Bowen

<-------User has bash as login shell------>
 usermod --shell /bin/bash Bowen

<-------User can access the server via SSH------>
<-------Prevent the root user from connecting to the server via SSH------>
Goes in to /etc/ssh/
Vim or nano sshd_config
In the config file, change PermitRootLogin to no
Add a line AllowUsers <username>   my exmaple:AllowUsers Bowen 

<-------Install nginx------>
apt install nginx
Do you want to continue? [Y/n] y

<-------Configure nginx to serve a sample website------>
make a dir named my-site in /var/www/ : mkdir /var/www/my-site
Create a html file in /var/www/my-site and add simple html file: vim /var/www/my-site/index.html
my example:
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>2420</title>
    <style>
        body {
            display: flex;
            align-items: center;
            justify-content: center;
            height: 100vh;
            margin: 0;
        }
        h1 {
            text-align: center;
        }
    </style>
</head>
<body>
    <h1>Hello, World</h1>
</body>
</html>

Go to /etc/nginx/sites-available

Delete the defult:rm default

Create my-site.conf, vim my-site.conf and adding the following
Note: Your path need to point to the index.heml file. In here, my path is /var/www/my-site
#server {
#	listen 80;
#	listen [::]:80;
#
#	server_name example.com;
#
#	root /var/www/my-site;
#	index index.html;
#
#	location / {
#		try_files $uri $uri/ =404;
#	}
#}

Next, we need creat a symbolic link and in /etc/nginx/sites-enabled: 
ln -s /etc/nginx/sites-available/my-site.conf /etc/nginx/sites-enabled/

Then, we need to unlink the default link:
sudo unlink /etc/nginx/sites-enabled/default

Then, we can test nginx configurations:
sudo nginx -t

It will show successful.

Then, we can run nginx server: 
sudo systemctl start nginx

We can also double check the status see if it is correctly running:
sudo systemctl status nginx

Then, everything is setup. We can go to browser and type http://143.198.71.159/ (basically http://  and the IP address)
You should see the content in your index.html. For me, it is "Hello, World"

Note: Google browser is not supporting since it automatically redirect from HTTP to HTTPS.



