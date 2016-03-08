# -*- mode: ruby -*-
# vi: set ft=ruby ts=2 sw=2 expandtab:
require "yaml"

config = "config.yaml"
params = if File.exists?(config) then YAML::load_file(config) else {} end
params_vm_box = params["vm_box"] || "boxcutter/fedora23"
params_vm_provision = params["vm_provision"] || "provision/fedora23"
params_memoria_vm = params["memoria_vm"] || "512"

Vagrant.configure(2) do |config|
  config.vm.box = params_vm_box

  # Configuração de ambiente necessária (no Fedora 23) para selecionar o provedor padrão
  ENV['VAGRANT_DEFAULT_PROVIDER'] = 'virtualbox'

  # Configura a quantidade de memória utilizada pela VM
  config.vm.provider "virtualbox" do |vb|
    # Don't boot with headless mode
    # vb.gui = true
    # Use VBoxManage to customize the VM. For example to change memory:
    vb.customize ["modifyvm", :id, "--memory", params_memoria_vm, "--usb", "off", "--audio", "none"]
  end

  # É realizado um forward da porta 4567 pois ela é a padrão utilizada pelo Middleman
  config.vm.network :forwarded_port, guest: 4567, host: 4567

  # Exporta a porta utilizada pelo LiveReload
  config.vm.network :forwarded_port, guest: 35729, host: 35729

  # O diretório superior a localização desse Vagrantfile será montando como /vagrant na VM
  config.vm.synced_folder "..", "/vagrant"

  # Chama o script para provisionar o sistema operacional
  # O script de provisionamento recebe o caminho até este Vagrantfile
  #   e utiliza essa informação para saber onde localizar scripts (relativos a esse caminho)
  config.vm.provision "shell", path: params_vm_provision, args: ENV['PWD'], privileged: false

  # Configura o plugin vagrant-vbguest (necessário instalá-lo) para não atualizar a máquina
  config.vbguest.auto_update = false
end

# Referências úteis:
#
# 1) Algumas dicas sobre o que fazer quando o provisionamento fica "preso":
# http://answers.candoerz.com/question/33110/chef-freezes-on-vagrant-centos-box.aspx
