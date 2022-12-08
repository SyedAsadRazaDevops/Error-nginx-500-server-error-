I have been trying to configure my nginx vuejs static frontend. My site consistentlyreturns a 500.
<img width="1680" alt="Screenshot 2022-12-07 at 6 56 28 PM" src="https://user-images.githubusercontent.com/71556060/206403028-db022fa5-65d4-451b-a243-0963505ca702.png">
### i had the same issue with my **Vue project**, I'm 100% sure that my nginx server block is good. 
**Here is an example:**
````
    server {
    listen      80;
    server_name example.com www.example.com;        charset utf-8;
    root    /var/www/myapp/dist;
    index   index.html;
    #Always serve index.html for any request
    location / {
        root /var/www/myapp/dist;
        try_files $uri  /index.html;
    }
    error_log  /var/log/nginx/vue-app-error.log;
    access_log /var/log/nginx/vue-app-access.log;
    }
````
but, I get the error 500. also i assign the execution permission to **dist/** folder and nginx user ownership of file:
````
    chown -R www-data:www-data ./dist/*
````
and 
````
    chmod 755 -R ./dist/*
````


## **SOLUTION**

  -1 check the www-data user has the path/ file permission
````
    root@root:# sudo -u www-data stat /home/myuser/public_html/

````
### **OUTPUT:**
> sudo: /etc/sudo.conf is group writable sudo: /etc/sudo.conf is group
> writable   File: /home/efficonx/public_html/   Size: 4096           
> Blocks: 8          IO Block: 4096   directory Device: fc01h/64513d   
> Inode: 258318      Links: 4 Access: (0775/drwxrwxr-x)  Uid: (    0/   
> root)   Gid: (    0/    root) Access: 2022-12-07 13:48:55.245310115
> +0000 Modify: 2022-12-06 13:13:40.187999036 +0000 Change: 2022-12-07 13:48:33.149826680 +0000 Birth: 2022-12-06 07:34:49.277257817 +0000

  -2 if the www-data have no permission then assign him group/permission in my case i add him to root user group
````
    sudo adduser www-data root
````
and all DONE.

 
