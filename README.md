# Cloud Computing Data Warehouse com Terraform na AWS

## Sobre o projeto: 
Criação de Cluster Redshift utilizando IaC. 

(Projeto do curso de Formação Engenheiro de Dados da Data Science Academy. Arquivos contém modificações do original)

![arquitetura](https://github.com/misoliv/aws_dw_iac/blob/main/img/arquitetura.png)

---
**O que é IaC?**

Infraestrutura como código (IaC) é uma prática de gerenciamento e provisionamento de recursos de TI. Através do uso de código e automação, é possível criar, realizar manutenção e escalar ambientes de TI de forma eficiente.

**Terraform**

HCL (HashiCorp Configuration Language) é uma linguagem desenvolvida pela HashiCorp, usada em ferramentas de infraestrutura como Terraform. A HCL é utilizada para definir e fornecer recursos de infraestrutura. Os arquivos de configuração do Terraform tem a extensão ".tf". 
A HCL é composta por blocos, argumentos e expressões.

A HashiCorp mantém uma plataforma conhecida como Terraform Registry. É um local que funciona como repositório centralizado, onde os usuários compartilham e encontram módulos Terraform. 

[Terraform Registry](https://registry.terraform.io/)

**Deploy com Terraform**

Os principais comandos utilizados para realizar um deploy através do Terraform são:

-terraform init: inicializa o terraform

-terraform validate: verifica se os arquivos de configuração estão livres de erros

-terraform plan: serve para visualizar as ações que o Terraform realizará com base nos arquivos de configuração

-terraform apply: cria, modifica ou exclui recursos conforme descrito nos arquivos de configuração

-terraform destroy: remove todos os recursos que foram criados

---
**AWS Reshift**

Serviço de Data Warehousing gerenciado pela AWS. É indicado para armazenar  e analisar grandes volumes de dados.
É escalável e possui integração com ferramentas de BI.

---
**Recursos utilizados no projeto:**
- Amazon Web Service (AWS): IAM, AWS Cli, S3, Redshift
- Docker
- Postgresql
- Terraform
- Metabase

---
![arquitetura](https://github.com/misoliv/aws_dw_iac/blob/main/img/architecture.png)

1- Criar um container Docker local:

`docker run --name milena_projeto -p 5438:5432 -e POSTGRES_USER=admin -e POSTGRES_PASSWORD=admin123 -e POSTGRES_DB=dsdb -d postgres`

2- Acessar o container e instalar utilitários:

`docker exec -it [nome_container] /bin/bash`

- Instalar utilitários
apt-get update
apt-get upgrade
apt-get install curl nano wget unzip sudo

- Criar pasta de Downloads
mkdir Downloads
cd Downloads

- Download do AWS CLI
[aws cli](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

- Versão
aws --version

- Configurar AWS CLI
aws configure

- Teste
aws s3 ls

- Instalar o Terraform

[Terraform_download](https://developer.hashicorp.com/terraform/install)

- Versão do Terraform
  
terraform -version

3- Criar a pasta abaixo:

mkdir s3
cd s3

4- Criar os arquivos abaixo:

touch provider.tf
touch dados.tf

5- Editar cada um dos arquivos com o conteúdo que está na pasta s3:

nano provider.tf
nano dados.tf

6- Pelo terminal, na pasta s3, execute os comandos abaixo:

terraform init (iniciar o terraform)
terraform validate (validar arquivos)
terraform plan (criar plano de execução)
terraform apply (aplicar a infraestrutura)

3- Fazer o upload dos arquivos CSV para a pasta dados dentro do AWS S3

![bucket](https://github.com/misoliv/aws_dw_iac/blob/main/img/bucket.png)

4- Criar a pasta abaixo:

mkdir redshift
cd redshift

5- Criar os arquivos abaixo:

touch provider.tf
touch redshift.tf
touch redshift_role.tf

6- Editar cada um dos arquivos com o conteúdo que está na pasta Redshift:

nano provider.tf
nano redshift.tf
nano redshift_role.tf

![redshift_tf](https://github.com/misoliv/aws_dw_iac/blob/main/img/redshift_tf.png)

7- Pelo terminal, na pasta redshift, execute os comandos abaixo:

terraform init (iniciar o terraform)
terraform validate (validar arquivos)
terraform plan (criar plano de execução)
terraform apply (aplicar a infraestrutura)

![terraform](https://github.com/misoliv/aws_dw_iac/blob/main/img/terraform_apply.png)

8- Acessar o painel do Redshift na AWS e confirmar que o cluster do Redshift foi criado para o DW.

![redshift-cluster](https://github.com/misoliv/aws_dw_iac/blob/main/img/redshift_cluster.png)

9- Acessar o painel do IAM na AWS e verifique se a role RedshiftS3AccessRole foi criada. Copiar o endereço ARN da role e colocar no arquivo load_data.sql.

![iam_role](https://github.com/misoliv/aws_dw_iac/blob/main/img/role.png)

10- Dentro da pasta dados colocar o arquivo load_data.sql (está na pasta dados). Alterar o ACCOUNT ID para o número da sua conta AWS.

touch load_data.sql
nano load_data.sql

![sql](https://github.com/misoliv/aws_dw_iac/blob/main/img/sql.png)

11- Copiar o endpoint do seu cluster Redshift e ajustar o comando abaixo e então executar no terminal do container dentro da pasta dados. Digitar a senha quando solicitada (a mesma senha que está no arquivo redshift.tf)

`psql -h redshift-cluster.XXXXXXXXXX.us-east-2.redshift.amazonaws.com -U adminuser -d [nome do banco de dados] -p 5439 -f load_data.sql`

12- Acessar o editor de consultas do Redshift e conferir se os dados foram carregados.

![redshift](https://github.com/misoliv/aws_dw_iac/blob/main/img/redshift_painel.png)

13- Criar um container Docker com Metabase: docker run -d -p 3000:3000 --name metabase metabase/metabase

14- Acessar o Metabase pelo navegador [metabase](http://localhost:3000) e criar uma conexão ao Redshift.

15- Explorar os dados e criar relatórios e gráficos.

![metabase1](https://github.com/misoliv/aws_dw_iac/blob/main/img/metabase1.png)
![metabase2](https://github.com/misoliv/aws_dw_iac/blob/main/img/metabase2.png)
