# Documentation

These instructions were created for Ubuntu Linux, but should be the same for most Linux distributions.
Technically, this repository should work on Windows and Mac but the instructions are not outlined here.

## Setup

* Install vagrant using your distribution's package manager (recommended: 1.6.5+)
* Install ansible on the host machine.  This can be obtained through your distribution's package manager or by using ```pip```
    - Installing on Ubuntu versions < 10

        ```sh
        $ sudo apt-get install python-setuptools python-dev build-essential
        $ sudo easy_install pip
        $ sudo pip install --upgrade ansible
        ```

    - Installing on Ubuntu versions > 10

        ```sh
        $ sudo apt-get install python-pip python-dev build-essential
        $ sudo pip install --upgrade ansible
        ```

* Clone this repository
* Clone [Electric Boogaloo](https://github.com/neoncrisis/electric-boogaloo) if you haven't already
* Install the ```nugrant``` plugin.  This is used for configuring the Vagrantfile

    ```sh
    $ vagrant plugin install nugrant
    ```

* Copy the .vagrantuser.example file to .vagrantuser.  Follow the comments in the file to add the required details.

    ```sh
    $ cp .vagrantuser.example .vagrantuser
    $ nano .vagrantuser
    ```

## Run on Virtualbox
* Install virtualbox using your distribution's package manager
* Install the ```vagrant-vbguest``` plugin.  This ensures the matching version of virtualbox guest modules.

    ```sh
    $ vagrant plugin install vagrant-vbguest
    ```

* Start the environment from the project directory.

    ```sh
    # Virtualbox is the default provider, no --provider flag is needed
    $ vagrant up
    ```

* Launch the webserver

    ```sh
    $ vagrant ssh
    $ cd /code/electric_boogaloo
    $ DJANGO_SETTINGS_MODULE=electric_boogaloo.settings.loc python manage.py runserver 0.0.0.0:8080
    ```

## Run on Digital Ocean
* Install the ```vagrant-digitalocean``` plugin.  This provider allows using vagrant to create a DO droplet.

    ```sh
    $ vagrant plugin install vagrant-digitalocean
    ```

* Start the environment from the project directory

    ```sh
    # The digital ocean provider is used automatically if virtualbox is not installed.
    # To explicitly run this provider use the --provider flag
    $ vagrant up --provider=digital_ocean
    ```

* Launch the webserver

    ```sh
    $ vagrant ssh
    $ cd /code/electric_boogaloo
    $ DJANGO_SETTINGS_MODULE=electric_boogaloo.settings.loc python manage.py runserver 0.0.0.0:8080
    ```

## Tips and Tricks
* The Vagrantfile automatically sources a DevVagrantfile if it can find one.  Use it for any vagrant customizations.
* If you're using the virtualbox provider, install the ```vagrant-vbox-snapshot``` plugin to use virtualbox snapshotting features.
