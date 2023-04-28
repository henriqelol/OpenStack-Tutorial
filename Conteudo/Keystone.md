# Identidade (keystone)

O serviço de Identity providencia validação de credenciais e informações para usuários e grupos.

O serviço de Assignment fornece dados sobre Roles atribuições de Role às entidades geridas pelos serviços de Identity e
de Resource. O serviço de Token valida e administra tokens utilizados para autenticar solicitações de uma vez as
credenciais do utilizador tenham sido verificados;

Para realizar as criações a seguir é preciso o seu projeto ter permissão de **admin**. Caso queira confirmar, verifique
via linha de comando, deve estar com permissão **root** o arquivo `/etc/neutron/policy.json` o que cada um pode
utilizar em cada serviço.

```
{"context_is_admin":  "role:admin or user_name:neutron"}
```

Importante
A primeira vez que for utilizar o comando do `openstack service list` receberá a mensagem
`Missing value auth-url required for auth plugin password`. Este problema se dá devido a falta de das variaveis de
ambiente que ele utiliza para se conectar no keystone
e poder gerenciar os outros serviços.

Para resolver este problema, baixe o arquivo Openstack RC File no botão direito acima em **admin**, copie o conteudo,
crie um arquivo `admin-openrc`, cole o conteudo e carregue na sessão atual.

```
source admin-openrc
```

Será requisitado a senha do projeto admin `Please enter your OpenStack Password for project admin as user admin:`

O arquivo baixado deve ser algo similar com o arquivo abaixo.

<details>
<summary>admin-openrc.sh</summary>

```shell
#!/usr/bin/env bash
# To use an OpenStack cloud you need to authenticate against the Identity
# service named keystone, which returns a **Token** and **Service Catalog**.
# The catalog contains the endpoints for all services the user/tenant has
# access to - such as Compute, Image Service, Identity, Object Storage, Block
# Storage, and Networking (code-named nova, glance, keystone, swift,
# cinder, and neutron).
#
# *NOTE*: Using the 3 *Identity API* does not necessarily mean any other
# OpenStack API is version 3. For example, your cloud provider may implement
# Image API v1.1, Block Storage API v2, and Compute API v2.0. OS_AUTH_URL is
# only for the Identity API served through keystone.
export OS_AUTH_URL={{OS_AUTH_URL}}/identity
# With the addition of Keystone we have standardized on the term **project**
# as the entity that owns the resources.
export OS_PROJECT_ID={{OS_PROJECT_ID}}
export OS_PROJECT_NAME="admin"
export OS_USER_DOMAIN_NAME="Default"
if [ -z "$OS_USER_DOMAIN_NAME" ]; then unset OS_USER_DOMAIN_NAME; fi
export OS_PROJECT_DOMAIN_ID="default"
if [ -z "$OS_PROJECT_DOMAIN_ID" ]; then unset OS_PROJECT_DOMAIN_ID; fi
# unset v2.0 items in case set
unset OS_TENANT_ID
unset OS_TENANT_NAME
# In addition to the owning entity (tenant), OpenStack stores the entity
# performing the action as the **user**.
export OS_USERNAME="admin"
# With Keystone you pass the keystone password.
echo "Please enter your OpenStack Password for project $OS_PROJECT_NAME as user $OS_USERNAME: "
read -sr OS_PASSWORD_INPUT
export OS_PASSWORD=$OS_PASSWORD_INPUT
# If your configuration has multiple regions, we set that information here.
# OS_REGION_NAME is optional and only valid in certain environments.
export OS_REGION_NAME="RegionOne"
# Don't leave a blank variable, unset it if it was empty
if [ -z "$OS_REGION_NAME" ]; then unset OS_REGION_NAME; fi
export OS_INTERFACE=public
export OS_IDENTITY_API_VERSION=3
```

</details>

Para verificar se deu certo, verifique se o sistema encontra as variaveis com o comando `env | grep OS_`. Caso sim,
execute novamente o comando `openstack service list`.

![](/Conte%C3%BAdo/Images/openstack-services-list.png)

**Dica**

> `Openstack -h` ou `--help` apresenta todas as opções que voce pode gerenciar com a linha de comando, então quando
> ficar em duvida do que fazer, use e abuse do `-h` no final do comando.

## Criando Novo Projeto

<details><summary>Dashboard</summary><blockquote>

- Identity - Projects > Create Project > Project Information | Project Members | Project Groups > Create Project
</blockquote></details>

<details><summary>Linha de comando</summary><blockquote>

```
openstack project create name-project
```

</blockquote></details>

## Criando Novo Usuário

### Por Dashboard

<details><summary>Dashboard</summary><blockquote>

- Identity - Users > Create User > User Informations > Create User
</blockquote></details>

<details><summary>Linha de comando</summary><blockquote>

Para criar um usuário você pode linkar algumas configurações como projeto, email, senha, etc.

```
openstack user create --project {{IDProject}} --email {{EmailUser}} --password-prompt name-user
```

Para verificar se criou, se ficou com as configurações que você deseja, confirme com o `list` e `show {{ID ou Name}}`.

```
openstack user list
```

```
openstack user show name-user
```

```
openstack project list
```

Para permitir o novo usuário acessar o dashboard, adicione uma `role` para ele. Código abaixo da uma `role` **admin**
para o project **name-project**.

```
openstack role add --user name-user --project name-project admin
```

</blockquote></details>

Posteriomente deve-se permitir logar com user e password do novo usuario criado

## Criando Novo Group

<details><summary>Dashboard</summary><blockquote>

- Identity - Groups > Create Group > Group Informations > Create Group
</blockquote></details>

<details><summary>Linha de comando</summary><blockquote>

```
openstack group create name-group
```

Inserindo o usuario no grupo.

```
openstack group add user name-group name-user
```

Verificando se o usuario é do grupo.

```
openstack group contains user name-group name-user

```

</blockquote></details>

Caso queira, coloque usuarios no seu group conforme sua necessidade ou permissão

## Criando Nova Roles

<details><summary>Dashboard</summary><blockquote>

- Identity
- Roles > Create Role > Create Role
</blockquote></details>

<details><summary>Linha de comando</summary><blockquote>

```
code
```

</blockquote></details>

Caso queira, coloque usuarios no seu group conforme sua necessidade ou permissão
