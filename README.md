# Box vagrant para execução de um projeto Middleman

Este projeto pode ser utilizado como um sub-módulo de qualquer outro que necessite de uma [box Vagrant](https://www.vagrantup.com/docs/boxes.html) provisionada para a execução do [Middleman](https://middlemanapp.com/).

## Pré-requisitos

### Instalação do Cygwin (se estiver utilizando o Windows)

Se você estiver utilizando o Windows, precisará de um interpretador Bash para executar scripts deste projeto. Uma forma de se instalar esse interpretador é através da instalação do Cygwin, conforme os [passos descritos pelo Paulo Jerônimo](seguinte://github.com/paulojeronimo/dicas-windows/blob/master/instalacao-cygwin.asciidoc).

### Instalação do VirtualBox

No Windows, uma das formas de se instalar o VirtualBox é através do [Chocolatey](https://chocolatey.org/). Faça a sua instalação e, em seguida, execute o comando: 

    choco install virtualbox VirtualBox.ExtensionPack -y

No Fedora 23, a instalação do VirtualBox é detalhada [neste artigo do Paulo Jerônimo](https://github.com/paulojeronimo/dicas-linux/blob/master/instalacao-virtualbox.asciidoc).

### Instalação do Vagrant

No Windows, de forma semelhante a instalação do VirtualBox, o Vagrant também pode ser instado através do Chocolatey. Utilize o seguinte comando:

    choco install vagrant -y

No Fedora 23, a instalação do Vagrant é detalhada [no site Fedora Developer](https://developer.fedoraproject.org/tools/vagrant/about.html).

### Instalação do plugin vagrant-vbguest

O plugin [vagrant-vbguest](https://github.com/dotless-de/vagrant-vbguest) é utilizado no [Vagrantfile](Vagrantfile) deste projeto para impedir a atualização do [VirtualBox Guest Additions](https://www.virtualbox.org/manual/ch04.html) na box, caso o Vagrant detectasse que ele está mais atual na versão do VirtualBox sendo utilizada. Impedir essa atualização evita problemas que poderiam ocorrer na montagem de diretórios compartilhados caso a box fosse atualizada.

Esse plugin pode ser instalado com o seguinte comando:

    vagrant plugin install vagrant-vbguest

## Provisionamento da box e execução do teu projeto

### No Fedora 23 (default)

Por _default_, este projeto já está configurado para provisionar uma máquina executando o [Fedora 23](https://getfedora.org/). A box base utilizada é a [boxcutter/fedora23](https://atlas.hashicorp.com/boxcutter/boxes/fedora23) cujo projeto de construção está disponível no [GitHub](https://github.com/boxcutter/fedora). O provisionamento dessa base é realizado através de shell script, pelo script [provision/fedora23](provision/fedora23).

Para construir uma box que possa ser utilizada dentro de teu projeto Middleman, execute os seguintes passos:

    git clone https://github.com/paulojeronimo/middleman-box
    cd middleman-box
    vagrant up

O teu projeto Middleman é colocado em execução na box provisionada, e acessível pela porta _URL default_ http://localhost:4567, através do script [middleman-server](middleman-server). Esse script é executado ao final do provisionamento. Como o servidor do Middleman estará em execução, você notará que o shell ficará preso na execução do comando `vagrant up`. Para interromper esse servidor e, consequentemente, a execução da aplicação, você precisará digitar um `<Ctrl+C>`. Em seguida, deverá executar o comando `./middleman-server stop`.

Toda vez que o servidor middleman for interrompido dentro da box, você poderá reexecutar esse script (`middleman-server`) para colocar o Middleman em execução novamente.

### No Ubuntu (TODO)

## Sites Middleman que foram testados com este projeto

* http://github.com/paulojeronimo/middleman-asciidoc-example
* http://github.com/paulojeronimo/middleman-guides

