## Instalação do ambiente de testes com DevStack e Vagrant

Para instação do ansible é necessario saber quais Openstack existem, como e onde pode ser instalado.

- [DevStack](https://docs.openstack.org/devstack) é um conjunto de scripts para implantar um ambiente OpenStack de teste
  no Ubuntu, CentOS ou Fedora. É o mais indicado para aprendizado por ser fácil de instalar e usar, além de ser
  altamente personalizável.

- [Packstack](https://wiki.openstack.org/wiki/Packstack) é uma ferramenta de instalação OpenStack projetada para o Red
  Hat Enterprise Linux e o CentOS. É mais indicado para ambientes de produção em larga escala.

- [Kolla](https://wiki.openstack.org/wiki/Kolla) é uma ferramenta para implantar e gerenciar contêineres do OpenStack
  usando o Docker. É projetado para implantar e gerenciar contêineres OpenStack usando o Docker. As variações do Kolla
  são kolla, kolla-ansible e kolla-kubernetes.

Para o tutorial e estudo eu utilizo o DevStack por ser mais simples e a fácil para aprendizado e instalação, basicamente
é necessario executar o script de instalação no shell script e ele faz tudo automático.

> Para servidor de produção você vai fazer uma instalação distribuída em vários servidores físicos, distribuindo a
> instalação entre eles, totalmente diferente desta instação.

&nbsp;

![DevStack](https://docs.openstack.org/devstack/latest/_images/logo-blue.png)

O DevStack pode ser instalado no Fedora, Ubuntu e CentOS, mas antes alguns pontos antes da instalação deve ser
fortemente verificados:

1. Instale o DevStack em uma VM, e não em seu sistema principal.
2. Foi usado o Ubuntu 20.04 no tutorial, utilize versões similar ou acima da versão Ubuntu 14.04.
3. Utilize as configurações recomendadas ou se possivel maior:
   | | |  
   | ------------ | --------- |
   | Processador | 2 núcleos |
   | Memória | 8GB |
   | Disco rígido | 60GB |
   | | |

### Instalações

**Virtual Machine**

[VirtualBox](https://www.virtualbox.org/)

```
$ sudo apt install virtualbox
```

[Vagrant](https://www.vagrantup.com/)

<details>
<summary>Instalação por download do pacote.</summary>

```
$ wget https://releases.hashicorp.com/vagrant/2.2.19/vagrant_2.2.19_x86_64.deb
$ sudo apt install ./vagrant_2.2.19_x86_64.deb
$ vagrant --version
```

</details>

<details>
<summary>Instalação por apt-get.</summary>

```
$ sudo apt-get update
$ sudo apt-get upgrade
$ sudo apt-get -y install vagrant
```

</details>

<details>
<summary>Instalação do plugin vagrant-disksize.</summary>
O Plugin irá permitir redimensionar discos no VirtualBox.

```
$ vagrant plugin install vagrant-disksize
```

</details>


&nbsp;

**Openstack DevStack**

```
$ mkdir devstack
$ cd devstack
$ vim initial-devstack-setup.sh
```

Insira o código abaixo e salve(`wq`).

<details>
<summary>Código</summary>

```
#!/bin/bash

sudo apt update
sudo apt upgrade -y
sudo apt dist-upgrade -y
sudo useradd -s /bin/bash -d /opt/stack -m stack
sudo echo "stack ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers.d/stack
sudo git clone https://opendev.org/openstack/devstack /opt/stack/devstack
sudo echo '[[local|localrc]]
ADMIN_PASSWORD={{ADMIN_PASSWORD}}
DATABASE_PASSWORD=$ADMIN_PASSWORD
RABBIT_PASSWORD=$ADMIN_PASSWORD
SERVICE_PASSWORD=$ADMIN_PASSWORD
HOST_IP={{HOST_IP}}
FLAT_INTERFACE=enp0s8
FLOATING_RANGE={{FLOATING_RANGE}}
FIXED_RANGE=10.11.12.0/24
FIXED_NETWORK_SIZE=256
SWIFT_REPLICAS=1

enable_service s-proxy s-object s-container s-account
enable_plugin heat https://opendev.org/openstack/heat
enable_plugin heat-dashboard https://opendev.org/openstack/heat-dashboard
enable_plugin magnum https://opendev.org/openstack/magnum
enable_plugin magnum-ui https://opendev.org/openstack/magnum-ui' > /opt/stack/devstack/local.conf
sudo chown stack:stack -R /opt/stack
```

</details>

&nbsp;

**Altere as variaveis**

> **FLOATING_RANGE** - CIDR da sua rede de casa.
>
> **ADMIN_PASSWORD** - Senha que será usada para logar na interface gráfica depois.
>
> **HOST_IP** - Altere depois que a maquina virtual executar para o endereço IP da sua rede de casa.

Conceda permissão de execução:

```
$ chmod +x initial-devstack-setup.sh
```

Em qualquer dúvida, siga a [documentação de instalação](https://docs.openstack.org/devstack/latest/).

Crie o arquivo **VagrantFile** para subir a VM e adicione o código abaixo:

<details>
<summary>Código</summary>

```
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"
  config.disksize.size = "70GB"
  config.vm.hostname = "openstack"
  config.vm.network "public_network", bridge: "enp8s0"
  config.vm.provider "virtualbox" do |v|
    v.memory = 8192
    v.cpus = 10
    v.name = "openstack"
  end
  config.vm.provision "shell", path: "initial-devstack-setup.sh"
end
```

</details>

&nbsp;

Não esqueça de alterar os seguintes pontos:

> **enp8s0** - Mude para a interface de rede do seu equipamento host.
>
> **v.memory** - Defina o quanto de memória será usada, o mínimo.
>
> **v.cpus** - Quantas vCPUs serão alocadas.

&nbsp;

Execução do Vagrant

```
$ vagrant up
$ vagrant ssh
```

Vire seu usuário em **root** e logue com o usuário **stack** que foi criado.

```
$ sudo su
$ su - stack
```

Altere a variavel `{{HOST_IP}}` do arquivo _local.conf_. Para isto verifique o **ip** com o comando abaixo, será o valor
do ip da rede que está em bridge :

```
$ ip a
```

Inicie a execução e Let's Go!

```
$ ./stack.sh
```
Ao finalizar você terá uma tela similar a imagem abaixo:
&nbsp;

![](/Conte%C3%BAdo/Images/finished.png)

Para acessar o dashboard acesso o basta pegar o endereço ip e inserir no seu browser.

![](/Conte%C3%BAdo/Images/dashboard.png)

Caso queira verificar a quantidade de consumo, basta executar o comando:
```
free -h
```

**Notas importantes!**

- O endereço da placa de rede bridge será o endereço usado para chegar no dashboard do horizon.
- Não desligue essa maquina virtual, suspenda-a.
- Para suspender e voltar basta executar os comandos:

```
$ vagrant suspend
```

```
$ vagrant resume
```
