# -*- mode: ruby -*-
# vi: set ft=ruby :

# TODO:
#   não é necessário instalar os pacotes: xvfb x11vnc vncviewer. Esses pacotes fazem sentido se fosse uma VM headless.
#   performance:
#       fazer o build (docker-compose build wscef) antes do reload da máquina (provavelmente será necessário alterar o docker-compose). Verificar se melhora a desempenho quando o gnome.
#       Se não der certo a estratégia anterior, tentar o push do docker image.
#       O uso de uma box pré-configurada poderia melhorar o desempenho:
#           > usar o packer (exemplo: https://github.com/boxcutter/ubuntu);
#           > fazer o upload da box para https://atlas.hashicorp.com/;
#           > atualizar este Vagrantfile.
#   documentação:
#       fazer vídeo demonstrativo.

require_relative 'vagrant-reload.rb'

Vagrant.configure("2") do |config|

  config.vm.box = "boxcutter/ubuntu1604-desktop"
  
  config.vbguest.auto_update = false
  # Prevent TTY Errors. By default this is "bash -l"
  config.ssh.shell = "bash -c 'BASH_ENV=/etc/profile exec bash'"

  config.vm.provider "virtualbox" do |vb|
     vb.gui = true
     vb.memory = "1024"
  end
  
  config.vm.synced_folder ".", "/vagrant", disabled: true
    
  # faz o provisiomento da VM com as configurações e softwares necessários para ter acesso ao CEF e BB
  config.vm.provision "shell", privileged: false, inline: <<-SHELL
     sudo apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
     sudo apt-add-repository 'deb https://apt.dockerproject.org/repo ubuntu-xenial main'
     sudo apt-get update
     sudo apt-get install git xvfb x11vnc vncviewer docker-engine docker-compose -y --no-install-recommends
     sudo systemctl status docker  
     git clone https://github.com/farribeiro/wscef-docker
     bash -c 'echo "x11vnc  -auth /var/run/lightdm/root/:0 -display :0 -localhost &
export DISPLAY=:0
export XAUTHORITY=/home/vagrant/.Xauthority
cd ~/wscef-docker
sudo docker-compose run --rm wscef" > ~/warsaw.sh'
     gsettings set org.gnome.desktop.screensaver lock-enabled false
     chmod +x ~/warsaw.sh
     mkdir -p ~/.config/autostart/
     bash -c 'echo "[Desktop Entry]
Name=MyScript
GenericName=A descriptive name
Comment=Some description about your script
Exec=/home/vagrant/warsaw.sh
Terminal=true
Type=Application
X-GNOME-Autostart-enabled=true" > ~/.config/autostart/warsaw.desktop'
     sudo bash -c 'echo "[Seat:*]
autologin-user=vagrant" > /etc/lightdm/lightdm.conf'
     sudo bash -c 'echo "LANG=en_US.UTF-8
LANGUAGE=en_US:" > /etc/default/locale'
  SHELL

  # Depois do reboot da VM, o usuário vagrant será auto-logado no desktop do Ubuntu e, 
  # em seguida, o container warsaw será executado. Por fim, o firefox será executado e estará
  # pronto para acessar, por exemplo, o Intenet Banking da CEF ou BB.
  config.vm.provision :reload

end