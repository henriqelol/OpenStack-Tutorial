# Image Service (glance)

O Glance providencia serviços de pesquisa, registro e entrega para imagens de disco e servidor. Imagens de VM
disponibilizados através Glance podem ser armazenados em uma variedade de sistemas de arquivos simples
para sistemas como o projeto OpenStack Swift.

É possivel criar imagens atráves de diferentes formatos como .iso ou .img. No exemplo a seguir foi usado a imagem
`ubuntu-20.04.5-live-server-arm64.iso` do Ubuntu Server 20.04 LTS (Focal Fossa), baixado em
https://cdimage.ubuntu.com/releases/focal/release/.


## Criando Nova Imagem

>Não esqueça de passar as informações do formato, tamanho de disco e memoria ram.

<details><summary>Dashboard</summary><blockquote>

- Images > Create Image > Image Details > Create Image
&nbsp;
![](/Conte%C3%BAdo/Images/image.png)
</blockquote></details>

<details><summary>Linha de comando</summary><blockquote>

```
openstack image create --file ubuntu-20.04.5-live-server-arm64.iso --disk-format iso --public ubuntu-20.04
```

Para verificar se criou, se ficou com as configurações que você deseja, confirme com o `list` e `show {{ID ou Name}}`.

```
openstack image list
```

```
openstack image show ubuntu-20.04
```

</blockquote></details>