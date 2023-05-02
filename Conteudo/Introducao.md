## O que é? Para que serve?

O OpenStack é um projeto de plataforma de computação em nuvem de código aberto que se destaca por sua escalabilidade e
ampla variedade de recursos. É altamente personalizável, podendo ser usado em clouds privadas ou públicas.

O Openstack é uma plataforma de codigo aberto (gratuito) e tem o intuito de operar em um hardware obtida por uma
empresa ou entidade.

Atualmente é utilizado o modelo [IaaS](https://cloud.google.com/learn/what-is-iaas) e entrega como serviço os recursos
da infraestrutura sob demand pela propria nuvem. Para entender o funcionamento, e olhando a figura abaixo, imaginemos um
cenário onde uma pessoa necessita de uma máquina virtual com algumas especificações:

- Armazenamento de objetos (Swift)
- Serviço de imagem (Glance)
- Armazenamento em bloco (Cinder)
- Serviço de identidade (Keystone)

Uma implantação do OpenStack contém 
[vários componentes](https://www.openstack.org/software/project-navigator/openstack-components#openstack-services) 
que fornecem APIs para acessar recursos de infraestrutura, cabe a você
verificar qual serviço melhor irá te atender conforme sua necessidade.

![](https://www.laintimes.com/wp-content/uploads/2020/02/schema-open-stack.png)

Basta apenas realizarmos atraves de requests ou comandos para o serviço que necessita conforme as configurações que
deseja.

**Nota:** Além de utilizar o Openstack/DevStack como ferramente de aprendizaro, é possivel realizar muita coisa em 
relação ao mesmo como contribuição, alteração, novas implementações, configurações, testes e entre outro. Caso tenha 
interesse, acesse o [Profit](https://docs.openstack.org/devstack/latest/#profit).