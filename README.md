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
