# Week 6 – NGINX AS A REVERSE PROXY

1.	Cd into **/home/ubuntu** – this is where the app

2.	Node js apps need npm, ejs, mongoose, express (find the packages which are needed for the app) – **sudo npm install ejs, mongoose, express**

3.	Seed the database by doing **node seeds/seed.js** (connects the app machine to the db machine and populated the data, database is ready)

4.	**node app.js** to start the app.

5.	check the url in this case (development. local:3000)

6.	if you go on to port 80 goes to nginx

## **nginx as a reverse proxy** (ngix is a web server just as apache is and IIS )

-  reverse proxy protects the webserver. It is something that stands in front of a webserver to avoid risks of the internet. This way no one can see your web server, dummy webserver/proxy.

- lightweight with a small footprint

- embeddable

## Web servers

- A web servers’ job is to accept and fulfil requests for static content for websites (HTTP)

- Any big company has many many web servers as loads of people are visiting their website so that’s why they have many web servers scattered globally, but by having these many servers you are more likely to get hacked although you have many customers, it is more accessible to hackers.

## How can you manually use nginx as a reverse proxy?

1.	You need to go into the config file
**Cd etc/ nginx/ sites enabled**
-	There can be a lot of sites available but all of them will not be enabled.

2.	**Cat default**

3.	**Sudo vi default**

4.	Scroll to the location to insert:
   location / {
                 proxy_pass http://localhost:3000;

5.	You need to restart the system and then start it:
- **sudo systemctl stop nginx**
- **sudo systemctl start nginx**

6.	**sudo nginx -t** (test to see if it was successful)

7.	check the status – **sudo systemctl status nginx**

8.	**cd  /home/ubuntu/app**

9.	start the app - **node app.js**

10.	check the url without the port


## How to Automate a reverse proxy

- Inside of environment/ app create a folder like config and then in the provision file rm and link what you just made. And make sure you restart

1.	make sure the root is synced on the vagrant file with environment

2.	cd into **/etc/nginx/sites-enabled/default**

3.	change the location as you did manually, and copy this.

4.	In your atom go to the environment/ app and create a default file and paste this there and save.

5.	In the provision file in the same location you want to rm the file on the vm :
**sudo rm /etc/nginx/sites-enabled/default**

6.	Then you want to make a symbolic link with your home and vm to replace the default file you have just made: (also add this in the app provision)

**sudo ln -s /home/ubuntu/environment/app/default /etc/nginx/sites-enabled/default**

7.	Then you want to add system stop nginx and system start nginx

8.	Make sure you vagrant destroy and then start it again.


## How to add images with the proxy

1.	Find where the images are stored in you home (atom .)
In this case it is app/public/

2.	In the default file you have created you want to add:
````
# serve static files
location ~ ^/(images|javascript|js|css|flash|media|static)/  {
root    /var/www/virtual/big.server.com/htdocs;
expires 30d;
}

````

3.	Place the root with where the images are stored on your home:
/home/ubuntu/app/public;

4.	Vagrant destroy and cd into app and run it.
