**Virtual Machine**

Instalação do [Vagrant](https://linuxize.com/post/how-to-install-vagrant-on-ubuntu-20-04/)

<details>
<summary>Instalação por apt-get.</summary>

```
$ sudo apt-get update
$ sudo apt-get upgrade
$ sudo apt-get -y install vagrant
```

</details>

<details>
<summary>Instalação por download do pacote.</summary>

```
$ wget https://releases.hashicorp.com/vagrant/2.2.19/vagrant_2.2.19_x86_64.deb
$ sudo apt install ./vagrant_2.2.19_x86_64.deb
$ vagrant --version
```

</details>

<details>
<summary>Instalação do Plugin Vagrant Disk Size.</summary>

O Plugin **Vagrant Disk Size** permite redimensionar discos no VirtualBox.

```
$ vagrant plugin install vagrant-disksize
```

</details>
