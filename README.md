# Box vagrant para a execução de sites Middleman

Este projeto pode ser utilizado como um sub-módulo de qualquer outro que necessite de uma [box Vagrant](https://www.vagrantup.com/docs/boxes.html) provisionada para construção e execução de sites criados com o uso do [Middleman](https://middlemanapp.com/).

## Sites que já foram executados com o auxílio deste projeto

*   10/Mar/2016 - http://github.com/paulojeronimo/opendevise.io
    * Fork do [projeto](https://github.com/opendevise/opendevise.io) que cria o [site da OpenDevise](http://opendevise.io/)
*   10/Mar/2016 - http://github.com/paulojeronimo/atomic-site
    * Fork do [projeto](https://github.com/projectatomic/atomic-site) que cria o [site do Project Atomic](http://www.projectatomic.io/)
*   09/Mar/2016 - http://github.com/paulojeronimo/middleman-guides
    * Fork do [projeto](https://github.com/middleman/middleman-guides) que cria o [site do Middleman](https://middlemanapp.com/)
*   08/Mar/2016 - http://github.com/paulojeronimo/middleman-asciidoc-example
    * Fork do [projeto da OpenDevise que demonstra a integração entre o Middleman e o AsciiDoctor](https://github.com/opendevise/middleman-asciidoc-example)

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

O teu projeto Middleman pode ser colocado em execução na box provisionada, e acessível pela porta _URL default_ http://localhost:4567, através do seguinte comando:

    ./middleman-server

O script [middleman-server](middleman-server) também pode ser executado automaticamente ao final do processo de provisionamento, se o valor para o parâmetro de configuração `executa_servidor_midlleman` for `true`. Nesse caso, como o servidor do Middleman estará em execução, você notará que o shell ficará preso na execução do comando `vagrant up`. Para interromper esse servidor e, consequentemente, a execução da aplicação, você precisará digitar um `<Ctrl+C>`. Em seguida, deverá executar o comando `./middleman-server stop`.

Toda vez que o servidor Middleman for interrompido dentro da box, você poderá reexecutar esse script (`middleman-server`) para colocar o Middleman em execução novamente.

### No Ubuntu (TODO)

## Criação de um novo projeto Middleman

Os passos a seguir exemplificam como este projeto pode ser utilizado na criação de um novo site que será construído com a utilização do Middleman.

### Crie um diretório vazio para o teu novo site Middleman e vá para dentro dele

    mkdir meu-novo-site-middleman && cd $_

### Clone o projeto middleman-box

    middleman_box=https://github.com/paulojeronimo/middleman-box
    git clone $middleman_box

### Construa a box Vagrant

    cd `basename $middleman_box`
    vagrant up

### Acesse a box criada via SSH

    vagrant ssh

### Instale o Middleman

    gem install middleman

### Crie o teu novo projeto e mova seu conteúdo para o diretório /vagrant

    middleman init meu-novo-site-middleman
    rsync -av meu-novo-site-middleman/ /vagrant/
    rm -rf meu-novo-site-middleman

### Vá para o diretório /vagrant e inicie o servidor Middleman

    cd /vagrant
    bundle exec middleman server

### Acesse a URL do teu novo site e comece a editá-lo

Abra a URL http://localhost:4567 em teu browser. Edite os arquivos de teu novo site acessando o diretório `meu-novo-site-middleman` (criado no teu HOST).

