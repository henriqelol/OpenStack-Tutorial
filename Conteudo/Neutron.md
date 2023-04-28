# Networking (neutron)

Responsavel por fornecer "a rede como um serviço" entre dispositivos de interfacegerido por outros serviços OpenStack.
Além disso, o Neutron permite que qualquer pessoa possa construir serviços avançados de rede que se conectam às redes 
OpenStack.

## Criação de redes e roteadores com Neutron

&nbsp;
### Criando rede privada

<details><summary>Dashboard</summary><blockquote>

- Networks > Create Network > Network | Subnet | Subnet Details > Create Network
&nbsp;

![](/Conte%C3%BAdo/Images/CreateNetwork.png)

>Não esqueça de marcar a rede como Shared caso queira ligar com o roteador ou compartilhar.
</blockquote></details>
<details><summary>Linha de comando</summary><blockquote>

```
openstack subnet create --network name-network --subnet-range 10.9.7.0/24 name-subnet
```

</blockquote></details>

&nbsp;
### Criando roteador

<details><summary>Dashboard</summary><blockquote>

- Routers > Create Router > Router Configurations > Create Router
&nbsp;

![](/Conte%C3%BAdo/Images/CreateRouter.png)
</blockquote></details>

<details><summary>Linha de comando</summary><blockquote>

```
openstack router create name-router
```

</blockquote></details>

&nbsp;
### Ligando redes com roteador

<details><summary>Dashboard</summary><blockquote>

- Routers > Selecione a rota escolhida > Add Interface > Select Subnet > Submit
&nbsp;

![](/Conte%C3%BAdo/Images/AddInterface.png)

</blockquote></details>

<details><summary>Linha de comando</summary><blockquote>

```
openstack router set --external-gateway a494a2e0-c61b-4339-8d58-a4180df71301 name-router
openstack router add subnet name-router name-subnet
```

Para verificar se criou, se ficou com as configurações que você deseja, confirme com o `list` e `show {{ID ou Name}}`.

```
openstack router list
openstack subnet list
```

```
openstack router show name-router
openstack subnet show name-subnet
```
</blockquote></details>

<details><summary>Dashboard</summary><blockquote>
Network -> Floating IPs. Em seguida, clique em Allocate IP to Project.