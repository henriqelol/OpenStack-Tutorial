# Object Storage (swift)

## Object store com Swift

Aqui é onde se encontra o armazenamento de objeto, o sistema responsavel pelo de armazenamento distribuído de objetos 
altamente escalável.

<details><summary>Dashboard</summary><blockquote>

**Criando um Container**

- Objeto Store > Container > + Container

![](/Conte%C3%BAdo/Images/CreateContainer.png)

É possivel posteriormente incluir pastas ou arquivos dentro do container.

![](/Conte%C3%BAdo/Images/DetailsContainer.png)

</blockquote></details>
<details><summary>Linha de comando</summary><blockquote>

```
openstack container create name-container
```

Para verificar se criou, se ficou com as configurações que você deseja, confirme com o `list` e `show {{ID ou Name}}`.

```
openstack container list
openstack container show {{ID ou Name}}
```

Para criar arquivo, faça os passos abaixo
```
openstack object create --name arquivo-texto name-container oi.txt
```

</blockquote></details>
<details><summary>Dashboard</summary><blockquote>