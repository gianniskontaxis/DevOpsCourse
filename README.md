# Vagrant DevOps Environment - CentOS Stream 9 with Apache Web Server

This project provides a reproducible development environment using Vagrant. It sets up a CentOS Stream 9 virtual machine with the Apache web server (httpd) installed, configured, and running.  It also downloads and deploys a website template.

## Overview

This Vagrant environment allows you to:

*   Quickly provision a CentOS Stream 9 virtual machine.
*   Install and configure the Apache web server.
*   Access a sample website deployed on the server.
*   Experiment with web server configuration and deployment in a controlled environment.

## Prerequisites

Before you begin, ensure you have the following installed:

*   [Vagrant](https://www.vagrantup.com/downloads.html) (version 2.0 or later)
*   [VirtualBox](https://www.virtualbox.org/wiki/Downloads) (or another Vagrant-supported provider like VMware, but this README assumes VirtualBox).

## Getting Started

1.  **Clone this repository:**

    ```bash
    git clone [repository URL]
    cd [repository directory]
    ```

2.  **Start the Vagrant environment:**

    ```bash
    vagrant up
    ```

    This command will download the `eurolinux-vagrant/centos-stream-9` base box (if you don't have it already), create the virtual machine, and provision it according to the settings in the `Vagrantfile`. This process may take some time, as it installs software and downloads website content.

3.  **Access the VM:**

    ```bash
    vagrant ssh
    ```

    This command will connect you to the virtual machine via SSH. You'll be logged in as the `vagrant` user.

## Usage

*   **Accessing the Web Server:** Once the VM is up and provisioned, you can access the website in your web browser using the following methods:

    *   **Public Network:** If your `Vagrantfile`'s `public_network` line is uncommented *and* your network configuration allows it, the VM will obtain an IP address from your network's DHCP server. You can determine this IP address by running `ip addr` inside the VM after you've SSH'd in, and then access the website using that IP address.  Note that this will make your VM visible to other devices on your network.

    *   **Private Network:** The `Vagrantfile` configures a private network. To access the web server via the private network, you'll need to determine the IP address assigned to the VM on that network. After you SSH into the VM (`vagrant ssh`), run the command `ip addr` (or `ifconfig` on older systems) to list the network interfaces and their IP addresses. Look for the interface associated with the private network (it might be named something like `eth1` or `enp0s8`) and its corresponding IP address. Then, use that IP address in your web browser. This approach isolates the VM's network traffic to your host machine.

*   **Exploring the Website Files:** After SSH'ing into the VM, you'll find the website files in the `/var/www/html/` directory. You can modify these files to experiment with web development.

*   **Apache Configuration:** The Apache web server configuration files are located in `/etc/httpd/conf/` and `/etc/httpd/conf.d/`. You can modify these files to customize the web server's behavior. Remember to restart Apache after making changes (`sudo systemctl restart httpd`).

## Vagrantfile Configuration

The `Vagrantfile` is configured to:

*   Use the `eurolinux-vagrant/centos-stream-9` base box. This provides a minimal CentOS Stream 9 installation.
*   Set up a private network, allowing you to access the VM from your host machine.  **Note:** The specific IP address for this network is configured within the `Vagrantfile`, but you should refer to the `ip addr` command within the VM to confirm the active IP address.
*   Configure a public network (bridged adapter), allowing the VM to obtain an IP address from your network.
*   Allocate 1024 MB of memory to the virtual machine.
*   Use a shell script to provision the VM, performing the following actions:
    *   Installs `httpd`, `wget`, `unzip`, and `vim` using `yum`.
    *   Starts and enables the `httpd` service to run on boot.
    *   Downloads, unzips, and deploys the "Barista Cafe" website template from Tooplate to the `/var/www/html/` directory.
    *   Restarts the `httpd` service to serve the new website content.
    *   Cleans up the temporary directory `/tmp/cafe`.

## Troubleshooting

*   **"Vagrant box not found":** This likely means that you don't have the `eurolinux-vagrant/centos-stream-9` box downloaded yet. The `vagrant up` command should download it automatically, but if it fails, you can manually add the box using `vagrant box add eurolinux-vagrant/centos-stream-9`.
*   **"The guest machine exited unexpectedly during the first boot":** This can be caused by insufficient memory allocated to the VM. While the Vagrantfile sets `vb.memory = "1024"`, your system might still be struggling. Try closing other applications on your host machine to free up memory, or increase the memory allocation in the `Vagrantfile` (if your system has enough RAM).
*   **Web server not accessible:**
    *   Make sure the VM is running (`vagrant status` should show "running").
    *   Check that the Apache web server is running inside the VM (`sudo systemctl status httpd`). If it's not running, start it with `sudo systemctl start httpd`.
    *   Verify that your firewall is not blocking access to port 80 or 8080 (if you're using port forwarding).
*   **Public network issues:** If you have problems with the public network, ensure that your network adapter is properly configured and that your firewall allows bridged connections. You might need to configure your network adapter in VirtualBox settings to allow bridging.
*   **`vagrant up` failing:** Look closely at the output of `vagrant up`. It will often provide error messages that can help diagnose the problem (e.g., network issues, permission problems, etc.).



