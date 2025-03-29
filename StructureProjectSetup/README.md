
---

## **🔥 1. WHAT IS A VAGRANTFILE?**  
A **Vagrantfile** is a Ruby config file that defines:  
- **Virtual Machine (VM) settings** (CPU, RAM, OS)  
- **Networking** (ports, IPs)  
- **Folder sharing** between host/guest  
- **Automated setup** (install software, configs)  

---

## **⚡ 2. BASIC STRUCTURE**  
### **MINIMAL VAGRANTFILE**  
```ruby
Vagrant.configure("2") do |config|  
  config.vm.box = "ubuntu/focal64"  
end  
```

---

## **🚀 3. FULL CONFIG BREAKDOWN**  

### **📦 BOX CONFIG (OS IMAGE)**  
```ruby
config.vm.box = "ubuntu/jammy64"          # Official Ubuntu 22.04  
config.vm.box_version = "202303.15.0"      # Specific version  
config.vm.box_url = "https://example.com/boxes/ubuntu.box"  # Custom box  
config.vm.hostname = "my-dev-vm"           # Set VM hostname  
```

### **🌐 NETWORKING**  
```ruby
# Port forwarding (host:8080 → guest:80)  
config.vm.network "forwarded_port", guest: 80, host: 8080  

# Private network (static IP)  
config.vm.network "private_network", ip: "192.168.33.10"  

# Public network (bridged - gets IP from router)  
config.vm.network "public_network"  
```

### **📂 SYNCED FOLDERS (HOST ↔ GUEST)**  
```ruby
# Basic sync  
config.vm.synced_folder "./code", "/home/vagrant/code"  

# Advanced options  
config.vm.synced_folder "./data", "/mnt/data",  
  owner: "vagrant",  
  group: "vagrant",  
  mount_options: ["dmode=755", "fmode=644"]  
```

### **🛠️ PROVISIONING (AUTOMATE SETUP)**  
```ruby
# Run shell commands  
config.vm.provision "shell", inline: <<-SHELL  
  apt update  
  apt install -y nginx git  
SHELL  

# Copy files  
config.vm.provision "file", source: "~/configs/app.conf", destination: "/etc/app.conf"  

# Use Ansible  
config.vm.provision "ansible" do |ansible|  
  ansible.playbook = "setup.yml"  
end  
```

### **⚙️ PROVIDER-SPECIFIC SETTINGS**  
```ruby
# VirtualBox (most common)  
config.vm.provider "virtualbox" do |vb|  
  vb.memory = 4096       # 4GB RAM  
  vb.cpus = 2            # 2 CPU cores  
  vb.name = "my-vm"      # VM name in VirtualBox  
  vb.gui = false         # Disable GUI (headless)  
end  

# VMware  
config.vm.provider "vmware_desktop" do |vmw|  
  vmw.memory = 8192      # 8GB RAM  
  vmw.cpus = 4  
end  
```

### **🐳 MULTI-MACHINE SETUP (CLUSTER)**  
```ruby
Vagrant.configure("2") do |config|  
  # Web server  
  config.vm.define "web" do |web|  
    web.vm.box = "ubuntu/focal64"  
    web.vm.network "private_network", ip: "192.168.33.10"  
  end  

  # Database server  
  config.vm.define "db" do |db|  
    db.vm.box = "centos/7"  
    db.vm.network "private_network", ip: "192.168.33.11"  
  end  
end  
```

---

## **💻 4. EXAMPLE VAGRANTFILES**  

### **✅ BASIC SINGLE-MACHINE**  
```ruby
Vagrant.configure("2") do |config|  
  config.vm.box = "ubuntu/jammy64"  
  config.vm.network "forwarded_port", guest: 80, host: 8080  
  config.vm.provision "shell", inline: "apt update && apt install -y nginx"  
end  
```

### **🚀 ADVANCED MULTI-MACHINE**  
```ruby
Vagrant.configure("2") do |config|  
  # WEB SERVER  
  config.vm.define "web" do |web|  
    web.vm.box = "ubuntu/focal64"  
    web.vm.network "private_network", ip: "192.168.33.10"  
    web.vm.provision "shell", path: "scripts/web.sh"  
    web.vm.provider "virtualbox" do |vb|  
      vb.memory = 2048  
      vb.cpus = 2  
    end  
  end  

  # DB SERVER  
  config.vm.define "db" do |db|  
    db.vm.box = "centos/7"  
    db.vm.network "private_network", ip: "192.168.33.11"  
    db.vm.provision "ansible", playbook: "provision/db.yml"  
  end  
end  
```

---

## **🔧 5. COMMANDS CHEATSHEET**  
| Command | Description |  
|---------|-------------|  
| `vagrant up` | Start VM |  
| `vagrant ssh` | SSH into VM |  
| `vagrant halt` | Shutdown VM |  
| `vagrant destroy` | Delete VM |  
| `vagrant reload` | Restart VM + reload config |  
| `vagrant provision` | Re-run provisioning |  

---



---

