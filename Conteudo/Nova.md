# Compute (nova)

Nova permite fornecer recursos de computação massivamente escaláveis sob demanda. Um ponto interessante é que que a
maior parte da comunicação componente a componente deve ser feita via fila de mensagens, e para evitar o bloqueio cada
componente enquanto espera por uma resposta, usamos objetos diferidos, com um callback que é acionado quando for
recebida uma resposta.

O Nova permite realizar um emaranhado de coisas, seus principais componentes são:

- Banco de dados SQL para armazenamento de dados.
- Auth Manager: componente responsável por usuários, projetos e papéis ("roles").
- Objectstore: servidor http que replica a api s3 e permite o armazenamento e recuperação de imagens.
- Scheduler: decide qual host recebe cada VM.
- Rede: gerencia IP Forwarding, pontes e vlans.
- Compute: gerencia a comunicação com máquinas virtuais e hypervisor.

## Criando regras de firewall com Security Groups

É possivel pegar uma rede e criar regras atraves do security group conforme a sua necessidade,
como egress (trafego de saida) e ingress (trafego de entrada), protocolo como ICM, etc. Existem diversas regras prontas
que basta apenas habilitar para o security group. A ideia principal é nada mais do que criar regras especificas para
um security group especifico;.

<details><summary>Dashboard</summary><blockquote>

- Networks > Security Groups > Create Security Group > name > Create Security Group
  ![](/Conte%C3%BAdo/Images/NewSecurityGroup.png)

Após criar

- Networks > Security Groups > Botton Manage Rules > Add Rule > Add
  ![](/Conte%C3%BAdo/Images/addrule.png)
  &nbsp;

</blockquote></details>
<details><summary>Linha de comando</summary><blockquote>

```
openstack security create --project admin name-security-group-cli

openstack security group rule create --ingress --dst-port 22 --protocol tcp
--remote-ip 192.168.0.6/32 name-security-group-cli
```

Para verificar se criou, se ficou com as configurações que você deseja, confirme com o `list` e `show {{ID ou Name}}`.

```
openstack security group list
openstack security group list --project admin
openstack security group rule list
```

</blockquote></details>

## Gerenciamento chaves SSH

Uma maneira para acessar os nós e vms que posteriormente criaremos será atráves de do SSH como usuário root. Caso
durante o processo de implantação da nuvem privada foi enviado as chaves SSH, está será a maneira de conectar. O comando
para se conectar (você terá que alterar o nome da chave e o IP para corresponder às suas informações):

```
ssh -i ~/.ssh/{{NOME_SUA_CHAVE}} root@{{IP_VM}}
```

<details><summary>Dashboard</summary><blockquote>

**Criando um par de chave ssh**

Um arquivo será gerado para a sua maquina, este arquivo será a sua chave privada, a chave que aparece posteriormente
quando criado no openstack é a sua chave publica e pode ser inserido nas suas vms.

- Compute > Key Pairs > Create Key Pair > Key Pair Name | Key Type > Create Key Pair

  ![](/Conte%C3%BAdo/Images/CreateKeyPar.png)

**Importando chave ssh**

- Compute > Key Pairs > Import Public Key > Key Pair Name | Key Type | Escolher Arquivo > Import Public Key
  ![](/Conte%C3%BAdo/Images/ImportKeyPairs.png)
  &nbsp;

</blockquote></details>
<details><summary>Linha de comando</summary><blockquote>
Copie a sua chave publica ssh da sua máquina LOCAL

```
cat .ssh/{{NOME_CHAVE_SSH}}.pub
```

Crie sua chave publica na maquina Openstack

```
sudo vim .ssh/{{NOME_CHAVE_SSH}}.pub
```

cole o conteudo no arquivo e salve (`wq`).

Para criar a key pair no openstack execute o comando

```
openstack keypair create --public-key .ssh/id_rsa.pub name-key-pair
```

Para verificar se criou, se ficou com as configurações que você deseja, confirme com o `list` e `show {{ID ou Name}}`.

```
openstack keypair list
openstack keypair show {{ID ou Name}}
```

</blockquote></details>

## Flavors, o que são e como criar ?

Os flavors são uma maneira de definir as VCPUs, RAM e espaço em disco usados por uma instância. Por default já existem alguns flavors pré-construídos durante a instalação do devstack.

![](/Conte%C3%BAdo/Images/Flavors.png)

Um ponto interessante é que para as ações em relação ao Flavor é necessário acessar a parte admin do devstack e os defaults tem uma nomeclatura referente as suas configurações.

<details><summary>Dashboard</summary><blockquote>

**Criando um Flavor**

Um arquivo será gerado para a sua maquina, este arquivo será a sua chave privada, a chave que aparece posteriormente
quando criado no openstack é a sua chave publica e pode ser inserido nas suas vms.

- Compute > Flavor > Create Flavor > Flavor Information > Create Flavor

![](/Conte%C3%BAdo/Images/CreateFlavor.png)

</blockquote></details>
<details><summary>Linha de comando</summary><blockquote>

> Verifique com atenção os pontos ao criar um flavor, como para qual projeto quer criar ele, ele vai ser publico ou
> privado, tamanho, criação de id aleatorio ou algum já esperado, etc

Para criar a key pair no openstack execute o comando

```
openstack flavor create  --private --vcpus 4 --ram 4096 --disk 4 --project name-project name-flavor
```

Para verificar se criou, se ficou com as configurações que você deseja, confirme com o `list` e `show {{ID ou Name}}`.

```
openstack flavor list
openstack flavor show {{ID ou Name}}
```

</blockquote></details>
<details><summary>Dashboard</summary><blockquote>

**Criando Maquinas Virtuais - Instancias**

Instâncias ou máquinas virtuais desempenham um papel importante na carga de trabalho de uma nuvem, e elas são a junção
de configurações pré realizadas anteriormentes. Para criar uma instância, incluindo a configuração de uma rede privada e
de um roteador, a criação de um grupo de segurança e como adicionar um par de chaves SSH.

- Compute > Instances > Launch Instance
  Vá completando as opções (algumas são obrigatorioas) e preencha os detalhes conforme desejar.

Aba Details

![](/Conte%C3%BAdo/Images/LaunchInstance.png)

Aba Source

![](/Conte%C3%BAdo/Images/SourceFlavor.png)

Aba Flavor

![](/Conte%C3%BAdo/Images/FlavorInstance.png)

Aba Networks

![](/Conte%C3%BAdo/Images/NetworkInstance.png)

Aba Security Groups

![](/Conte%C3%BAdo/Images/SecurityGroupsInstance.png)

Aba Key Pair

![](/Conte%C3%BAdo/Images/KeyPairInstance.png)

Clique em Launch Instance e aguarde finalizar o processo de criação de uma instancia. Um ip se torna fixo para a
instancia criada, assim como um volume que será utilizado para a vm. O Status final da instancia deve ser active.

Para acessar via IP na instancia é necessario de um IP Flutuante para esta maquina.

</blockquote></details>
<details><summary>Linha de comando</summary><blockquote>

> Verifique com atenção todos os pontos ao criar uma instancia, varias coisas (algumas obrigatorias) devem ser passadas,
> como ID ou Nome, para qualquer duvida use `openstack server create -h`.

```
openstack server create --flavor name-flavor --image ubuntu-20.04 --network name-network --security-group name-security-group-cli --key-name name-key-pair name-instance
```

Para verificar se criou, se ficou com as configurações que você deseja, confirme com o `list` e `show {{ID ou Name}}`.

```
openstack server list
openstack server show {{ID ou Name}}
```

</blockquote></details>

## Atribuir e anexar um IP flutuante

A instância criada anteriormente está associada a uma rede privada, não permitimos que a acessemos, para resolve isto,
é preciso conectar-se a ela com os nós de hardware da nuvem ou usar um IP flutuante.

> Uma das principais utilizações dos ips flutuantes no openstack é o Failover(tolerância a falhas). Quando uma vm,
> servidor ou outro componente de hardware ou software fica indisponível, é alterado o direcionamento da maquina daquele
> ip para outra similar mas com o mesmo ip, assumindo as operações sem que haja interrupção nos serviços.

<details><summary>Dashboard</summary><blockquote>
Para alocar um IP flutuante, vamos seguir os passos:

1. Verifique em suas networks (Network > Networks) sua rede publica e qual faixa de ip da sua Subnet ela se encontra
   (caso não exista) crie uma nova rede com uma subnet publica.

2. Para alocar um IP flutuante, vá em Network -> Floating IPs. Clique em Allocate IP to Project.

   2.1. Verifique se o campo **Pool** está definido como External, clique em Allocate IP para adicionar esse endereço IP
   flutuante para uso.

![](/Conte%C3%BAdo/Images/CreateFloatingIPs.png)

3. Relacione IP à instância clicando no botão Associate no canto direito.

   3.1. Em Port to be associated selecione a instancia que deseja ligar o IP e clique em associate

Para valicação via SSH:

`ssh -i {{NAME_SYSTEM}}@{{IP_ACESS_INSTANCE}}`

Para validação via ping:

`ping {{IP_ACESS_INSTANCE}}`

> Nota:
> É necessario verificar se o security group que a instancia se encontra possui regras de permissao de ping (ICMP) e
> SSH.

Para duvidas, verifique o desenho via Network Topology, se a redes se encontram tendo relação.

</blockquote></details>
<details><summary>Linha de comando</summary><blockquote>

Criando um ip publico.

```
openstack floating ip create public
```

Verifique o IP em **floating_ip_address**

Listando os servidores.

```
openstack server list
```

Relacione o server com o endereço de floating ip (floating_ip_address) criado anteriormente.

```
openstack server add floating ip name-server {{floating_ip_address}}
```

Para valicação via SSH:

`ssh -i {{NAME_SYSTEM}}@{{IP_ACESS_INSTANCE}}`

Para validação via ping:

`ping {{IP_ACESS_INSTANCE}}`

> Nota:
> É necessario verificar se o security group que a instancia se encontra possui regras de permissao de ping (ICMP) e
> SSH.

</blockquote></details>
