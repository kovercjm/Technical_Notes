# 1 About Leanote
>Leanote, not just a notpad!

Leanote is a brand-new tool that provides everything *Evernote* has offered and plus a bunch of new features, such as *Markdown* editor.

Official documentation refers to [GitHub](https://github.com/leanote/leanote).

# 2 About Server
The whole process ran under Ubuntu 18.04. The VPS, which can support the load of Leanote server, has 1 CPU core and 1G RAM.

# 3 Installation
## 3.1 Overview
To use the new features sooner, **Leanote** server is going to be installed with its running environment and source code.

>1. Install the **Golang** execution environment.
2. Fetch the source code of **Leanote**.
3. Install **MongoDB** database.
4. Import initial data to **MongoDB**.
5. Configure **Leanote**.
6. Run **Leanote** by **revel**.

## 3.2 Install the Golang environment
Download the latest [Golang](https://golang.org/dl/) corresponding to the system, and extract the file.
```
$ wget https://dl.google.com/go/go1.12.linux-amd64.tar.gz
$ tar -xzvf go1.12.linux-amd64.tar.gz
```
Make a new directory to store *Go* packages, in this case, the source code of *Leanote*.
```
$ mkdir gopackage
```
Configure the environment variables.
```
$ sudo vim /etc/profile
```
Add the following lines:
```
export GOROOT=/root/go
export GOPATH=/root/gopackage
export PATH=$PATH:$GOROOT/bin:$GOPATH/bin
```
Execute to make changes take effect.
```
$ source /etc/profile
```
Check the *Go* installation.
```
$ go version
```
If success, the terminal shall print as follow:
```
go version go1.12 linux/amd64
```

## 3.3 Fetch Leanote source code
Download the source code of *Leanote*, extract the files and move the *src* directory to *Go* packages directory.
```
$ wget https://github.com/leanote/leanote-all/archive/master.zip
$ unzip master.zip
$ cp -r ./leanote-all-master/src /root/gopackage
```

## 3.4 Install MongoDB database
The MongoDB installation follows the [official tutorial](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/). 
Import the *MongoDB* public GPG key used by the Ubuntu package management system.
```
$ apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 9DA31620334BD75D9DCB49F368818C72E52529D4
```
Create a list file for Ubuntu 18.04.
```
$ echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu bionic/mongodb-org/4.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.0.list
```
Reload the local package database and install the *MongoDB*.
```
$ sudo apt-get update
$ sudo apt-get install -y mongodb-org
```
Start running *MongoDB*.
```
$ systemctl start mongod
$ systemctl enable mongod
```

## 3.5 Import initial data
Import the initial data to *MongoDB*, and enter the *MongoDB* terminal.
```
$ mongorestore -h localhost -d leanote --dir $GOPATH/src/github.com/leanote/leanote/mongodb_backup/leanote_install_data/
$ mongo
```
In *MongodB* terminal, check the *Leanote* database and put it into use.
```
> mongorestore -h localhost -d leanote --dir $GOPATH/src/github.com/leanote/leanote/mongodb_backup/leanote_install_data/
> use leanote
```

## 3.6 Configure Leanote
The configuration of *Leanote* is controlled by 'app.conf' file. In which, one setting that is **strongly resuggested** to modify is 'app.secret'. Please change arbitrary number of digits of the string to something different, but keeping the string length unchanged. This is to avoid potential security issues.
```
$ vim /root/gopackage/src/github.com/leanote/leanote/conf/app.conf
```

## 3.7 Run Leanote server
Generate **revel** to run *Leanote*.
```
$ go install github.com/revel/cmd/revel
```
Make sure the *MongoDB* is still up and running, and the default **9000** port is open. Then execute *Leanote* in background.
```
$ nohup revel run github.com/leanote/leanote 2>&1 &
```
The running logs will be stored in 'nohup.out' file.
Now you can access **Leanote** through website or client applications via **[server ip]:9000**.

## 3.8 [**TODO**] Auto start up

# 4 Optimization
## 4.1 Secure MongoDB and Leanote
Set a root user for leanote database. Run the following command in *MongoDB* terminal.
```
> db.createUser({user:'root', pwd:'kover', roles:[{role:'dbOwner', db:'leanote'}]})
```
Modify the *Leanote* configuration as well.

## 4.2 Install plugin to export PDF notes
The **wkhtmltopdf** plugin is used in Web version Leanote supporting the export-PDF-notes feature.
Download the plugin, extract the files and move the *wkhtmltopdf* to /usr/local/bin for further usage.
```
$ wget https://github.com/wkhtmltopdf/wkhtmltopdf/releases/download/0.12.4/wkhtmltox-0.12.4_linux-generic-amd64.tar.xz
$ tar -xvf wkhtmltox-0.12.4_linux-generic-amd64.tar.xz
$ cd wkhtmltopdf/bin
$ chmod +x wkhtmltopdf
$ mv wkhtmltopdf /usr/local/bin
```

## 4.3 [**TODO**]Transmit Leanote service
The **OpenResty** is used to transmit the *Leanote* service to port 80. The installation process is following the [official website](http://openresty.org/cn/linux-packages.html).
Import the GPG public key, add the OpenResty API repository and install *Openresty* as follow.
```
$ wget -qO - https://openresty.org/package/pubkey.gpg | sudo apt-key add -
$ sudo apt-get -y install software-properties-common
$ sudo add-apt-repository -y "deb http://openresty.org/package/ubuntu $(lsb_release -sc) main"
$ sudo apt-get update
$ sudo apt-get install openresty
```

## 4.4 [**TODO**] HTTPS Support

## 4.5 Remove installation packages
```
$ rm go1.12.linux-amd64.tar.gz # Golang
$ rm master.zip # Leanote source code
$ rm -r leanote-all-master/ # Extracted source file
$ rm wkhtmltox-0.12.4_linux-generic-amd64.tar.xz # wkhtmltox plugin
$ rm -r wkhtmltopdf/ # Extracted plugin file
```