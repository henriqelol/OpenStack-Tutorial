# Block Storage (cinder)

O Cinder é responsavel por providenciar dispositivos de armazenamento para serem usados com instâncias do Compute.

## Criação de volumes com Cinder

<details><summary>Dashboard</summary><blockquote>

- Volumes > Volumes > Create Volume > Informations About Volume > Create Volume

    ![](/Conte%C3%BAdo/Images/CreateVolume.png)

</blockquote></details>

<details><summary>Linha de comando</summary><blockquote>

```
openstack volume create --size 1 nane-volume
```

Para verificar se criou, se ficou com as configurações que você deseja, confirme com o `list` e `show {{ID ou Name}}`.

```
openstack volume list
openstack volume show {{ID ou Name}}
```

</blockquote></details>


## Associando Volumes

Você pode atachar volumes com instancias, para isto realize os passos abaixos

<details><summary>Dashboard</summary><blockquote>

- Compute > Instances

    Na instancia que deseja atachar o volume, clique em `Attach Volume`, selecione o volume e em `Attach Volume` novamente. Pronto, o volume já se encontra atachado na instancia.

    ![](/Conte%C3%BAdo/Images/AttachVolume.png)

    Caso queira verificar, acesse a instancia e verifique o campo volume.

</blockquote></details>

<details><summary>Linha de comando</summary><blockquote>

```
openstack server add volume name-server name-volume
```

Caso queira verificar, faça acesso SSH em uma maquina virtual e execute  o comando `sudo fdisk -l`.