**Virtual Machine**

Instalação do [VirtualBox](https://linuxhint.com/install-virtualbox-linux/)

<details>
<summary>Instalação por apt-get.</summary>

```
$ sudo apt-get update
$ sudo apt-get upgrade
$ sudo apt install virtualbox
```

</details>

<details>
<summary>Instalação por download do pacote.</summary>

```
$ wget -q https://www.virtualbox.org/download/oracle_vbox_2016.asc -O- | sudo apt-key add -
$ sudo add-apt-repository "deb [arch=amd64] http://download.virtualbox.org/virtualbox/debian $(lsb_release -cs) contrib"
$ sudo apt update
$ sudo apt install virtualbox-6.1
```

</details>
