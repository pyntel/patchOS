# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "archlinux/archlinux"

  config.vm.provider "virtualbox" do |v|
    v.memory = 2048
    v.cpus = 2
  end

  config.vm.provision "shell", inline: <<-SHELL
    sudo echo "Server = http://mirror.archlinux.no/\\$repo/os/\\$arch" > /etc/pacman.d/mirrorlist
    sudo pacman -Syu --noconfirm
    sudo pacman -S base-devel parted dosfstools gcc-libs wget rsync git python2 --noconfirm
    rm -Rf yay-git
    su vagrant -c "git clone https://aur.archlinux.org/yay-git.git"
    cd yay-git
    su vagrant -c "makepkg -si --skipchecksums --noconfirm"
    yay -Syu --noconfirm
    gpg --recv-key 9766E084FB0F43D8
    yay -S binfmt-qemu-static-all-arch qemu-user-static --noconfirm
    systemctl restart systemd-binfmt.service
  SHELL
end
