### Centinel server

The Server used to control Centinel nodes in the wild.

### Install and usage
#### Preparation (all platforms)
Get the maxind geolocation database by running 

    $ mkdir ~/.centinel
    $ curl http://geolite.maxmind.com/download/geoip/database/GeoLite2-Country.mmdb.gz | gunzip -c > ~/.centinel/maxmind.mmdb
    $ curl http://download.maxmind.com/download/geoip/database/asnum/GeoIPASNum.dat.gz | gunzip -c > ~/.centinel/asn-db.dat

Download and install PostgreSQL [here](http://www.postgresql.org/download/).
Create user root:

    $ sudo -u postgres createuser -s root

Change the password for the user postgres since Centinel-server connects to PSQL Database with this password: 

    $ sudo -u postgres psql
    $ postgres=> alter user postgres password 'postgres';

### Install dependencies

All the dependencies have been provided in a requirements.txt file. It is highly recommended that these be installed
in a virtual environment. This can be done by installing the `virtualenv` package for your operating system. Once 
that is installed, the following commands can be run to create the dependencies and install the packages:

```sh
python3 -m venv venv
. ./venv/bin/activate
```

Once the second command above has been run, there should now be a `(venv)` at the beginning of your prompt. This shows
that the virtual environment has been successfully installed. 

Installing the dependencies can then be done by running the following command: 

```sh
(venv)$ pip install -r requirements.txt
```

**Note:** For the rest of this document, if commands that need to be run _outside_ of the virtualenv will be prefaced with `$` 
and commands that do need the virtualenv will be prefaced with `(venv)$`.

### Supported platforms
    * Unix
    * Mac OS X

### Running in your local system
change production = False in config.py so that  the app will try to connect to local postgres sql server.

pass the --adhoc option to python run.py so that the application will not try to load ssl certificate.

    $ . ./venv/bin/activate
	(venv)$ python run.py --adhoc

### **OUTDATED - Will be updated in time** Running with Apache (WSGI) for production environments 
Install mod_wsgi and apache

	$ sudo apt-get install apache2 libapache2-mod-wsgi

Create the directory /opt/centinel-server and clone a copy of the centinel server repo with

	$ sudo mkdir /opt/centinel-server
    $ cd /opt/centinel-server; git clone https://github.com/iclab/centinel-server.git

Add centinel server port to /etc/ports.conf by adding Listen 8082 to /etc/apache2/ports.conf.

Add misc/centinel.conf to /etc/apache2/sites-enabled/

And setup your mod\_wsgi with your virtual env by adding this line to your /etc/apache2/mods_available/wsgi.conf:

        WSGIPythonPath /home/iclab/env/lib/python2.7/site-packages

Edit your WSGI script if necessary at centinel.wsgi



