# AWS

Criei este repositório apenas para registrar algumas notas enquanto estudo sobre a AWS.

# CloudWatch

- Monitoramento  
  - Possibilita, por exemplo, enviar um alerta quando exceder X dólares  
  
# IAM (Identity and Access Management)

- Controle de acesso, credenciais, etc.
- MFA (autenticação multifator)
  - Funciona com apps como, por exemplo, Duo Mobile (Dispositivo MFA virtual)
- Action (operação que pode performar) / Effect (allow ou deny) / Resource
- Já possui várias politicas padrões que pode utilizar, mas é possível criar politicas customizadas
- É recomendado criar grupos e nunca aplicar a política direto a uma pessoa usuária diretamente (sempre com o mínimo de permissões necessárias)
- É possível customizar as políticas de senha (pelo menos um número, caracteres especiais, etc)
- Usuárias criadas por aqui devem fazer o login através de outro link (ex: https://513034491943.signin.aws.amazon.com/console)

# EC2 (Elastic Compute Cloud)


## VPC (Virtual Private Cloud)
- IPs privados
- Routing
- Security groups
- Ferramenta importante para segurança
- Access Control List (filtro por ips, por exemplo)
- Pode possuir um grupo de Subnets

## Load Balancer
- Ouve uma porta (ex: um para 80 e outro pra 443)
- É criado dentro de uma VPC
- É habilitado para N availability zones
- Deve ser associado à um security group (quais ips podem acessar uma determinada porta)
- É criado um Target group (podendo ser do tipo Instance, IP ou Lambda Function)
  - Neste passo, cuidado ao preencher a porta. Deve ser a porta que a aplicação está escutando internamente
- Possui uma URL para healthcheck
- Permite fidelizar a sessão (através de um cookie) chamado de "stickiness". Fica associado ao target group.

## AMI (Amazon Machine Image)
-  É possível criar a partir de uma instância criada (EC2 -> Instances -> Actions -> Image -> Create Image)

## Auto scaling group
- Você escolhe qual imagem que será usada (para usar a sua já previamente criada, selecione "My AMIs")
- Escolhe que tipo de instância EC2 será criada (t2.micro, t2.medium, etc indicando poder computacional da máquina)

## Security group
- É possível configurar para que aceite conexões apenas através de um load baleancer previamente criado (Security group -> Inbound -> Edit -> No campo Source, selecione o security group com load balancer no lugar do IP).

## Elastic IP addresses
- É possível criar um ip estático publico e associa-lo com recurso que desejar. É possível associa-lo com outro recurso depois.

## S3 (Simple Storage Service)
- Armezana objetos (não importando o tipo)
- Permite gerenciar quem pode consulta-los
- Objetos são separados em "buckets"
  - Um bucket serve para armazenar um grupo de arquivos
  - Serve tbm para criar uma URL de acesso
  - O nome do balde precisa ser único para todo S3 (não apenas na sua conta)
- Um objeto pode ter até 5 terabytes (mas tem um limite de upload de 5Gb, para mais que isso precisa usar uma ferramenta de multipart
- Pode escolher qual região do mundo serão armazenados
- Além do arquivo, armazena também meta-dados (file type, data de modificação, etc)
- Um objeto é idenficado pela sua chave, formada pelo path e nome do arquivo, excluindo o nome do bucket (ex: images/image.png)
- É possível utilizar o IAM para dar permissões de acesso
- Existe a opção de "cross region replication" para diminuir a latência e aumentar a redundância
  - Permite a redundância para apenas uma outra região, para ter no mundo todo, use o CloudFront
- AWS Policy Generator (ferramenta para facilitar a criação de politicas de acesso)
- Copiando arquivos para um bucket via linha de comando:
  - `aws s3 cp <pasta>-s3 s3://<bucket-name>/<pasta> --recursive`
- Permite acessar os arquivos diretamente por uma URL (podendo inclusive deixar o acesso público)
- É possível habilitar CORs (por bucket)
- Existe SDKs para fazer upload diretamente para o S3

## DataBases

### RDS (Relational Database Service)
- Várias engines disponíveis: Amazon Aurora, PostgreSQL, MySQL, MariaDB, Oracle Database e SQL Server
- Permite replicar em zonas diferentes
  - Failover automatico
- Permite criar replica da base para apenas leitura
  - Consistência eventual
  - Neste caso, não é usada como failover

### DynamoDB
- Não relacional (NoSQL)
- Schemaless
- É formado por um conjunto de tables
- Cada tabela tem um conjunto de itens.
- Cada item tem uma primary key (obrigatóriamente)
- A chave primária pode ser uma string, número ou binário
- A tabela tem um "provisioned throughput capacity"
  - Quantidade de operações de leitura/escrita por segundo (baseado em uma unidade de 4kb)
  - Se passar o limite estabelecido, a amazon pode fazer throttle ou até mesmo recusar a requisição
  
## Cloud Formation
 - Permite provisionar recursos utilizando templates (em formato json)
 - É possível, por exemplo, recriar a infrastrutura facilmente (ex: replicar todo ambiente de produção em um ambiente de dev)
 - Você pode versionar os templates no git e fazer rollback se precisar
 - CloudFormation Stack
   - Grupo de recursos AWS
   - É possivel atualizar ou deletar uma stack
   - No caso de atualizar, tentará fazer isso sem reiniciar sua instancia, se possível
 - CloudFormation Designer 
   - Ferramante "drag and drop" para criar templates
   - Permite criar do zero ou importar um template previamente criado
 - CloudFormer
   - Cria um CloudFormation template a partir da uma infraestrutura previamente criada

## Elastic Beanstalk
  - Automatiza scaling, monitoring e resource provisioning
  - Executa aplicação em uma plataforma específica (node.js, Java, etc)
  - Armazena no S3 uma ou mais versão do seu app, permitindo por exemplo fazer rollback do deploy
  - Permite ter vários environments (ex: dev e prod)
    - cada um tem seu próprio AMI, EC2 instance, auto scalling group etc
    - cada environment pode ter uma versão diferente da aplicação rodando

## Cloud Front
  - Global CDN
  - Tem como objetivo diminuir a latência
    - Para isso, tenta diminuir a distância entre as usuárias e o app
  - Funciona em apps previamente criados, sem necessidade de alterações
  
## EC2 Instance
  - Uma instância EC2 é separada em duas partes: 
    - Execution Environment (CPU, Memory, Bandwidth)
    - File System (três tipos)

## EC2 File System
  - Existem 3 formas possíveis de tratar file system em uma instância EC2
    - Instance Store
      - Fisicamente conectados à instância
      - Funciona basicamente como um hard drive
      - Não podem ser re-utilizados para outras instâncias EC2
    - Elastic Block Storage (EBS)
      - Pode ser associada a mais instâncias
      - Pode sobreviver mesmo que a instância EC2 seja destruida
    - Elastic File System (EFS)
      - Similar ao EBS, mas tem tamanho escalável

## AMI (Amazon Machine Image)
  - Cada instância no EC2 precisa de uma AMI
  - Uma AMI pode ter apenas o Sistema Operacional (Linux ou Windows por exemplo) ou SO + Softwares pré-instalados (proprietário ou open-souce). Ex: Uma máquina Linux com node.js pré-instalado.
  - Você pode criar sua própria imagem com os softwares que quiser.
    - Pode inclusive criar de forma incremental. Cria uma máquina com Linux, instala os softwares que quiser a cria uma AMI a partir dela.
  - Pode escolher uma imagem a partir da AWS Marketplace.
  - Tipos de visibilidade de uma AMI
    - Public (qualquer um pode utilizar e iniciar uma instância a partir desta imagem)
    - Explicit (visível para quem você permitir acessa-la)
    - Implicit (privada)
    - Pode ser publicada na AWS Markplace
    
## Criando uma instância a partir de uma AMI
  - Podem ser usadas duas formas para criar a AMI:
    - EBS Backed AMI
      - Podem ser pausadas por determinado período
      - Boot muito mais rápido
      - Dados serão persistidos no EBS
      - Foi introduzina da AWS posteriormente e existem várias vantagens em utiliza-la
    - Instance Volume Backed AMI
      - Não podem ser pausada, apenas destruída ("terminated") ou reiniciada
      - Boot lento (precisa copiar dados da S3 durante boot)
