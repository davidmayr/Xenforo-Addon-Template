# Xenforo Addon Template

This is a basic Xenforo Addon Template. It uses Docker to serve a Xenforo install.
This was made since it was pretty hard for me to manage my addons(for https://cafestu.be) with git and maintain an acceptable
folder structure.

# Installation Guide

### Step 1: Downloading Xenforo
First you need a Xenforo Licence and you need to download the package from the official xenforo website.
Make sure you download all elements. By default, the xenforo website only gives you an upgrade package.
So make sure the checkbox is turned off.

After you have downloaded the zip file you need to create a new directory in your folder structure called "xenforo".
Drag the contents of the "upload" directory from the zip file in your new "xenforo" folder.

This folder is added in your .gitignore, so it will not be committed, and you can safely work on your addons.

### Step 2: Starting the Containers

Start the docker container by running "docker-compose up -d" in your command line.
Wait until it is running. It might take a while when executing it the first time.

### Step 3: Configuring Xenforo

Navigate to "localhost" in your browser. It starts on port 80 by default, so you don't need to provide a port.
Begin the installation. For the database enter these values:

```
MySQL server: xenforo-db
MySQL Port: 3306
MySQL username: xenforo
MySQL password: password
MySQL database name: xenforo
```

Start the installation. This can take a while...

### Step 4: Enabling development mode. 

For this next step and for generally having a good development experience(and use the _output folder) you need development mode enabled.
This allows you to use all the development features of xenforo which you will most likely need. 

Edit the file: xenforo/src/config.php and add the following line:

```
$config['development']['enabled'] = true;
```

You can <b>optionally</b> add this too. This will set your addon as the default option in development menus.
```
$config['development']['defaultAddOn'] = 'YourGroup/YourAddon';
```

### Step 5: Installing the addon/importing the data.

If you are running xenforo for the first time you need to install the addon via the command line, so it will be imported correctly.
If you have installed the addon before and need to update the data because of manual changes/a new commit you need to update the data.
Both of these actions need to be done with the command-line. 

To run a php command you need to connect to the container. Run "docker ps" in your terminal.

It will look somewhat like this:
```
CONTAINER ID   IMAGE                                   COMMAND                  CREATED          STATUS                    PORTS                    NAMES
807317e46be5   nginx:1.19-alpine                       "/docker-entrypoint.…"   17 minutes ago   Up 17 minutes (healthy)   0.0.0.0:80->80/tcp       xenforo-nginx
d64217844cbc   xenforo-addon-template_xenforo_server   "docker-php-entrypoi…"   17 minutes ago   Up 17 minutes (healthy)   9000/tcp                 xenforo
c4dca548fb4d   mariadb:10.3                            "docker-entrypoint.s…"   17 minutes ago   Up 17 minutes (healthy)   0.0.0.0:3306->3306/tcp   xenforo-db
```

Select the one that ends with _xenforo_server. And copy the Container ID

Then run this to connect to the container.
```
docker exec -it <ContainerId> /bin/sh
```

---

Execute this command if you want to <b>install</b> the addon.
```
php cmd.php xf:addon-install <AddOnId>
```

or execute this command to <b>update</b> the addon data:
```
php cmd.php xf-dev:import --addon <AddOnId>
```

Now you are done.

If you want to stop your xenforo run "docker-compose stop" and if you want to start it again run "docker-compose up -d".

# Changing the Addon Group or Name

To change the Addon Name/Group uninstall the addon in the Control Panel first. Then shutdown your instance.
Rename the group and addon folder and change the namespace in the PHP files and modify your addon.json.

Before you can run your container again you need to change the group name in the .env file. 
Start xenforo again and run Step 5(installing the addon) from the Installation Guide.