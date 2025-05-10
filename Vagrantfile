# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "debian/bookworm64"
  
  # Enable X11 forwarding so we can interact with GUI apps.
  config.ssh.forward_agent = true
  config.ssh.forward_x11 = true

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Disable the default share of the current code directory. Doing this
  # provides improved isolation between the vagrant box and your host
  # by making sure your Vagrantfile isn't accessible to the vagrant box.
  # If you use this you may want to enable additional shared subfolders as
  # shown above.
  # config.vm.synced_folder ".", "/vagrant", disabled: true

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
  #
  # View the documentation for the provider you are using for more
  # information on available options.
  config.vm.provider "virtualbox" do |vb|
    # Enable USB.
    vb.customize ["modifyvm", :id, "--usb-xhci", "on"]

    # Make USB devices available on VM. Get the values for `--vendorid` and
    # `--productid` using `vboxmanage list usbhost` from the host machine
    # with the UB device plugged in.
    # `--vendorid` value is the hex string prefixed with '0x'.
    # `--productid` value is the value displayed inside the parentheses (no '0x` prefix).
    # ESP32 DevKitC vendorid=0x10c4, productid=EA60
    vb.customize ["usbfilter", "add", "0", "--target", :id, "--name", "ESP Dev Board", "--vendorid", "0x10c4", "--productid", "EA60"]
  end

  # Enable provisioning with a shell script. Additional provisioners such as
  # Ansible, Chef, Docker, Puppet and Salt are also available. Please see the
  # documentation for more information about their specific syntax and use.
  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    # ESP-IDF requirements
    apt-get install -y \
      bison \
      ccache \
      cmake \
      dfu-util \
      flex \
      git \
      libffi-dev \
      libssl-dev \
      libusb-1.0-0 \
      ninja-build \
      python3 \
      python3-pip \
      python3-venv \
      wget 
    # Other packages
    apt-get install -y \
      build-essential \
      python3-ipython \
      vim 
  SHELL

  # Install ESP-IDF.
  config.vm.provision "shell", privileged: false, inline: <<-SHELL
    espdir=${HOME}/esp
    if [ ! -d ${espdir} ]
    then
      mkdir -p ${espdir}
      cd ${espdir}
      git clone -b v5.4.1 --recursive https://github.com/espressif/esp-idf.git

      # Install target hardware.
      # use `./install.sh all` for all ESP32 targets, otherwise comma-separated 
      # list of devices. e.g. `./install.sh esp32,esp32s2`
      cd ${espdir}/esp-idf
      ./install.sh esp32  

      # Activate with `source ~/esp/esp-idf/export.sh`
    fi
  SHELL
end
