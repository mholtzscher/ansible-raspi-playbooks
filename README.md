# Ansible Playbooks for Raspberry Pi setup

Some simple playbooks, templates and tasks for setup and configuration of my assorted raspberry pi boards and projects. I have found the Ansible [lineinfile](http://docs.ansible.com/ansible/lineinfile_module.html) module is perfect for automating Raspberry Pi setup since most of the configuration is done via text files.

The goal is to do as little manual setup of my raspberry pi projects as possible as well as creating a record of the setup and configuration that could be run again if there was a problem with the SD card, or if I build another copy of the project. 

All of the setup can be done remotely over the network without login, I found that using ethernet was the best way to set up new projects. For Pi Zero and A+ projects without an ethernet port I use a USB ethernet adaptor to do the initial setup. One of the best parts of using ansible for setups is that I can avoid the need to connect a monitor or keyboard for everything but Raspbian Pixel based projects. I have used prompts to collect sensitive data, so it is not stored in the repository.

So far I have found an easy way in Ansible to complete every step of the setup for my projects. There are a ton of well documented modules in ansible and I was able to easily handle apt, git boot/config.txt settings, making reusable tasks and handlers, setting up wifi with a template, installing the SSH key and collecting sensitive inputs with prompts.

### SSH Keys for login
A SSH key is used to automatically login to each device being managed. 

Set up a public/private keypair on the controller computer you will be running ansible from.

    ssh-keygen -t rsa -b 4096 -f ~/.ssh/id_rsa
    cat ~/.ssh/id_rsa.pub

On each device the initial setup script will manage the SSH key for you.

### Ansible hosts file
I have an ansible host file ~/ansible/hosts set up with all of my device hostnames.

    # Hostnames for devices 

    [defaultdevices] # Default hostnames for raspbian and retropie devices 
    retropie
    raspberrypi
    [defaultdevices:vars]
    ansible_user=pi
    ansible_ssh_pass=raspberry

    [picades]    
    elsies-arcade
    markos-arcade
    [picades:vars]
    ansible_user=pi

    [pocketpigrrls]
    elsie-pocket-pigrrl
    violet-pocket-pigrrl
    [pocketpigrrls:vars]
    ansible_user=pi

    [nesclassics]
    vh-pi-nes
    plumis-pi-nes
    millet-pi-nes
    [nesclassics:vars]
    ansible_user=pi

    [projects]
    astro-pi
    alexa-pi-vending
    pi-camera-motion
    pi-camera-timelapse
    pix-e-gif-cam
    [projects:vars]
    ansible_user=pi

    [clusternodes]
    p[1:4]
    [clusternodes:vars]
    ansible_user=pi 

Once the hosts file is in place the ansible controller is all set up and ready to use.