# -*- mode: ruby -*-
# vi: set ft=ruby ts=2 sw=2 expandtab:

Vagrant.configure(2) do |config|
  config.vm.box = "boxcutter/fedora23"

  # Configuração de ambiente necessária (no Fedora 23) para selecionar o provedor padrão
  ENV['VAGRANT_DEFAULT_PROVIDER'] = 'virtualbox'

  # É realizado um forward da porta 4567 pois ela é a padrão utilizada pelo Middleman
  config.vm.network :forwarded_port, guest: 4567, host: 4567

  # O diretório superior a localização desse Vagrantfile será montando como /vagrant na VM
  config.vm.synced_folder "..", "/vagrant"

  # Chama o script para provisionar o sistema operacional
  # O script de provisionamento recebe o caminho até este Vagrantfile
  #   e utiliza essa informação para saber onde localizar scripts (relativos a esse caminho)
  config.vm.provision "shell", path: "provision/fedora23", args: ENV['PWD'], privileged: false

  # Configura o plugin vagrant-vbguest (necessário instalá-lo) para não atualizar a máquina
  config.vbguest.auto_update = false
end

# Referências úteis:
#
# 1) Algumas dicas sobre o que fazer quando o provisionamento fica "preso":
# http://answers.candoerz.com/question/33110/chef-freezes-on-vagrant-centos-box.aspx
