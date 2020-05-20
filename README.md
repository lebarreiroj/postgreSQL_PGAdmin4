# curso-sgbd-sql
Curso Sistema Gerenciador de Banco de Dados e SQL

## Criação do Ambiente de Banco de Dados
- https://medium.com/@renato.groffe/postgresql-pgadmin-4-docker-compose-montando-rapidamente-um-ambiente-para-uso-55a2ab230b89

### PostgreSQL e PGAdmin 4 em containers Docker

#### O QUE É ESTE TUTORIAL?
Neste documento você terá o passo a passo para montar e configurar o nosso banco de dados PostgreSQL e pgAdmin 4 em containers Docker.

A função deste material é deixar um ambiente com banco de dados, PostgreSQL e pgAdmin 4 pronto para ser utilizado por qualquer pessoa que deseja preparar o seu ambiente de Banco de Dados para seus projetos.

Esse material não tem a função de explicar o que é e como funciona o Docker, somente o necessário para o funcionamento deste ambiente.

#### HÁ ALGUM PRÉ-REQUISITO PARA ESTE TUTORIAL FUNCIONAR?
Sim, pois você já deve o Docker instalado em sua máquina.
Caso você ainda não tenha, veja como instalar no tutorial “Como instalar o docker na minha máquina!” <criar tutorial e inserir o link> 

#### VAMOS AO PASSO A PASSO!
A montagem desse ambiente é rápida e simples!
Serão criados 2 (dois) containers, um com o banco de dados PostgreSQL e outro com pgAdmin 4, e uma network para a comunicação entre os containers.
Esse ambiente será criado utilizando-se o Docker Compose, um serviço do Docker para a criação e execução conjunta dos múltiplos containers de uma solução. Para isso vamos utilizar um arquivo chamado docker-compose.yml com os 3 (três) comandos para a criação e configuração dos containers e a network. 

##### PASSO 1 – Baixar o arquivo docker-compose.yml
Baixe o arquivo docker-compose.yml para a sua máquina. Mova o arquivo baixado para o seu diretório de trabalho.
Apenas como sugestão, crie seu diretório de trabalho com o nome curso_bd_sql.

Uma breve explicação do que tem no arquivo docker-compose.yml:
```
version: '3'

services:
  srv-bd-postgresql:
    image: postgres
    environment:
      POSTGRES_PASSWORD: "curso"
    ports:
      - "15432:5432"
    volumes:
      - /home/luisjesus/Dev/Docker-Compose/PostgreSQL:/var/lib/postgresql/data
    networks:
      - srv-postgres-network
      
  srv-pgadmin:
    image: dpage/pgadmin4
    environment:
      PGADMIN_DEFAULT_EMAIL: "luisjesus.ti@gmail.com"
      PGADMIN_DEFAULT_PASSWORD: "curso"
    ports:
      - "16543:80"
    depends_on:
      - srv-bd-postgresql
    networks:
      - srv-postgres-network

networks: 
  srv-postgres-network:
    driver: bridge
```

Esse arquivo docker-compose.yml cria:

Esse arquivo docker-compose.yml cria:
a)	Serviços (Containeres):
•	srv-bd-postgresql: com o banco de dados PostgreSQL com porta de acesso 15432; e
•	srv-pgadmin: com o PGAdmin 4 com porta de acesso 16543; 
b)	Rede (Network):
•	srv-postgres-network: serviço para comunicação dos containers srv-bd-postgresql e srv-pgadmin


##### PASSO 2 – Colocando para funcionar

Agora chegou a hora de colocar para funcionar!
Nós vamos trabalhar nesta etapa com o aplicativo Windows PowerShell. Para isso vamos abrir o aplicativo Windows PowerShell.

![image](https://user-images.githubusercontent.com/29760189/82391336-6fde8200-9a17-11ea-9e11-3ff03af5bf48.png)

Veja como é o Windows PowerShell na figura abaixo.

![image](https://user-images.githubusercontent.com/29760189/82391829-b385bb80-9a18-11ea-9507-6565e5b424bc.png)

Vamos ao diretório onde está o arquivo docker-compose.yml. 
No meu caso é o diretório C:\Users\lebar\Documents\curso-sgbd-sql.
Para mudar de diretório eu usei o comando:
``` 
cd .\Documents\curso-sgbd-sql
```
Também pode ser digitado todo o caminho: 
```
cd C:\Users\lebar\Documents\curso-sgbd-sql
```
![image](https://user-images.githubusercontent.com/29760189/82394875-84734800-9a20-11ea-893f-cbad28b6fddc.png)

Para verificar se o arquivo está no diretório, digite o comando ``` ls ``` (letras L e S) e o nome do arquivo que está procurando. 
O comando ls lista o conteúdo do diretório (arquivos e diretórios). O comando seguido de um nome ``` ls docker-compose.yml```, vai listar somente o arquivo procurado (o docker-compose.yml) se existir no diretório. 

A figura abaixo mostra o comando e o resultado da busca, o nome do arquivo na lista. 

![image](https://user-images.githubusercontent.com/29760189/82394974-d1efb500-9a20-11ea-9019-dc73ee58433a.png)

Vamos executar o comando docker-compose up -d que vai criar os containers. Caso as imagens do PostgreSQL e do PGAdmin ainda não exista na máquina, esse comando vai realizar o download (é o que ocorreu neste caso).



## Subindo imagens docker para o dockerhub (um exemplo)
- https://jtemporal.com/subindo-imagens-docker-pro-dockerhub/

## Objetivos do Curso
### Ao final do curso o aluno será capaz de:
- identificar os bancos de dados estruturados, semi-estruturados, quase estruturados e não estruturados;
- Realizar consultas em banco de dados estruturados por meio da linguagem SQL;
- Realizar manipulação de dados nas tabelas de Banco de Dados como, inclusão de registros, alteração de registra e exclusão de registro;
- Criar, alterar e excluir tabelas de Banco de Dados;
- Conceitos de Integridade Referencial, Chaves Primárias, Chaves Estrangeiras, etc...

## Profissões relacionadas
- Governança de dados: Políticas e boas práticas de dados a serem aplicadas na organização.
- Gerente de Dados (Data manager): é o encarregado da aplicação das polítitas de governança de dados desde o ponto de vista funcional e organizacional. É responsável por decidir se um usuário pode ou não acessar uma base de dados, e se afirmativo quais permissões o usuário terá. 
- Ingeniero de datos (Data engineer): é o encarregado de aplicar as políticas desde o ponto de vista técnico. É o responsável por conceer os acessoa aos usuários, conforma a autorização obtida do Administrador de Dados.
- Data steward (Administrador de Dados): se encarrega da formação e execução de políticas para a gestão dos dados que tem sob seu domínio, assegurando-se que em seus dasos se aplicam as mesmas políticas que definidas pelo Data manager.
- propietario de datos (Data owner): são responsáveis pelo uso dos dasod que estão sob o seu controle. Encarregam-se de devinir as regras de armazenamento dos dados nos bancos de dados, os padrões de qualidade e a utilidade de desses dados.
- Cientista de dados (Data scientist): Encarregados da criação de valor através da explotação de dados. Esses se encarregam de analisar os dados armazenados para encontrar padrões e oportunidades de aplicação ao negócio.

## Seções do Curso
- Preparando o ambiente para os estudos deste curso.
Vamos utilizar o SGBD PostgresSQL + PGAdmin 4

## Como os dados estão organizados e como os classificamos?
- Dados estruturados:
- Dados semi-estruturados:
- Dados quase estruturados:
- Dados não estruturados:

### Qual será o objeto de estudo neste curso?
Dados estruturados!

## Minhas primeiras consultas!
-

## Como podemos classificar os comandos da linguagem SQL?

### DQL, DML e DDL
A linguagem SQL é única, porém podemos dividi-la conforme a funcionalidade dos comandos.
DQL - Data Query Language (Linguagem de Consulta de Dados, em português), DML - Data Manipulation Language (Linguagem de Manipulação de Dados, em português) e Data Definition Language (Linguagem de Definição de Dados, em português) são tipos de linguagem SQL.
- DQL: Tipo de linguagem para realizar consultas nos dados que estão nas tabelas.
- DML: Tipo de linguagem para lidar com os dados dentro das tabelas. Com essa linguagem é possível, por exemplo: Inserir (INSERT), Alterar (UPDATE) e apagar (DELETE) dados em tabela.
- DDL: Tipo de linguagem para lidar com os objetos do Banco de Dados. Com essa linguagem é possível, por exemplo: criar (CREATE), alterar (ALTER) e apagar (DROP) Tabelas.

