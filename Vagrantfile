Vagrant.configure(2) do |config|
  # Use the same key for each machine
  config.ssh.insert_key = false
  config.vagrant.plugins = "vagrant-libvirt"
  config.vm.provision "shell", inline: $script
  config.vm.define "kk01" do |kk01|
    kk01.vm.box = "generic/ubuntu2204"
    kk01.vm.hostname = "kk01"
    kk01.vm.provider "libvirt" do |libvirt|
      libvirt.driver = "kvm"
      libvirt.memory = "2048"
      libvirt.cpus = "2"
      libvirt.management_network_name = "default"
      libvirt.management_network_address = "192.168.122.0/24"
      libvirt.management_network_mode = "nat"
    end
  end
  config.vm.define "grafana01" do |grafana01|
    grafana01.vm.box = "generic/ubuntu2204"
    grafana01.vm.hostname = "grafana01"
    grafana01.vm.provider "libvirt" do |libvirt|
      libvirt.driver = "kvm"
      libvirt.memory = "2048"
      libvirt.cpus = "2"
      libvirt.management_network_name = "default"
      libvirt.management_network_address = "192.168.122.0/24"
      libvirt.management_network_mode = "nat"
    end
  end
  config.vm.define "harbor01" do |harbor01|
    harbor01.vm.box = "generic/ubuntu2204"
    harbor01.vm.hostname = "harbor01"
    harbor01.vm.provider "libvirt" do |libvirt|
      libvirt.driver = "kvm"
      libvirt.memory = "4096"
      libvirt.cpus = "2"
      libvirt.management_network_name = "default"
      libvirt.management_network_address = "192.168.122.0/24"
      libvirt.management_network_mode = "nat"
    end
  end
end

$script = <<-SCRIPT
sudo cat <<EOF > /etc/apt/sources.list
deb http://mirror.yandex.ru/ubuntu jammy main restricted universe multiverse
deb http://mirror.yandex.ru/ubuntu jammy-updates main restricted universe multiverse
deb http://mirror.yandex.ru/ubuntu jammy-backports main restricted universe multiverse
deb http://mirror.yandex.ru/ubuntu jammy-security main restricted universe multiverse
EOF
sudo apt update
sudo apt upgrade -y
sudo apt install -y mc wget curl zip tree vim apt-transport-https ca-certificates gnupg2 software-properties-common
export OS_VERSION=Ubuntu_20.04
export CRIO_VERSION=1.23
SCRIPT
