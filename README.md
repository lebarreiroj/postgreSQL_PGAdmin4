# COMO CRIAR BANCO DE DADOS POSTGRESQL E PGADMIN 4 COM DOCKER - EXEMPLO EM MÁQUINA WINDOWS

## Criação do Ambiente de Banco de Dados - PostgreSQL e PGAdmin 4 em containers Docker

### O QUE É ESTE TUTORIAL?
Neste documento você terá o passo a passo para montar e configurar o nosso banco de dados PostgreSQL e pgAdmin 4 em containers Docker.

A função deste material é deixar um ambiente com banco de dados, PostgreSQL e pgAdmin 4 pronto para ser utilizado por qualquer pessoa que deseja preparar o seu ambiente de Banco de Dados para seus projetos.

Esse material não tem a função de explicar o que é e como funciona o Docker, somente o necessário para o funcionamento deste ambiente.

### HÁ ALGUM PRÉ-REQUISITO PARA ESTE TUTORIAL FUNCIONAR?
Sim, pois você já deve o Docker instalado em sua máquina.
Caso você ainda não tenha, veja como instalar na documentação do docker. 
- Para Mac: https://docs.docker.com/docker-for-mac/install/
- Para Windows: https://docs.docker.com/docker-for-windows/install/
- Para Linux: https://docs.docker.com/engine/install/

Neste tutorial, o ambiente será montado em uma máquina com o Linux Ubuntu 18.04.

### VAMOS AO PASSO A PASSO!
Agora que vocês já estão com Docker instalado, vamos montar esse ambiente de maneira bem rápida e simples!

Com esses passoa aqui, nós vamos criar 2 (dois) containers, um com o banco de dados PostgreSQL e outro com pgAdmin 4, e uma network para a comunicação entre os containers.

Para isso nós vamos utilizar o Docker Compose, um serviço do Docker para a criação e execução conjunta dos múltiplos containers em uma solução. A solução está no arquivo docker-compose.yml que você pode baixar para o diretório onde o ambiente será montado. Esse arquivo contém os 3 (três) comandos para criar e configurar os containers e a network. 

#### PASSO 1 – Baixar o arquivo docker-compose.yml
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
      - ../postgresql/data:/var/lib/postgresql/data
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
a)	Serviços (Containeres):
•	srv-bd-postgresql: com o banco de dados PostgreSQL com porta de acesso 15432; e
•	srv-pgadmin: com o PGAdmin 4 com porta de acesso 16543; 
b)	Rede (Network):
•	srv-postgres-network: serviço para comunicação dos containers srv-bd-postgresql e srv-pgadmin


#### PASSO 2 – Colocando para funcionar

Agora chegou a hora de colocar para funcionar!

Nós vamos trabalhar nesta etapa com o aplicativo Terminal.

Vamos ao diretório onde está o arquivo docker-compose.yml. No meu caso é o diretório ```/home/luis/desenv/postgreSQL_PGAdmin4 ```. Ver na imagem abaixo, onde eu listo o conteúdo do diretório com o comando ``` ls ``` (letras L e S).  
![Captura de tela de 2020-06-10 19-50-38](https://user-images.githubusercontent.com/29760189/84326667-df95e780-ab53-11ea-9d96-d289ba81e834.png)

Vamos executar o comando ```docker-compose up -d``` que vai criar os containers. Caso as imagens Docker do PostgreSQL e do PGAdmin ainda não exista na máquina, esse comando vai realizar o download (não é o que ocorre nesse caso, pois as imagens existem). Veja na imagem abaixo:
![Captura de tela de 2020-06-10 19-54-54](https://user-images.githubusercontent.com/29760189/84326870-4b785000-ab54-11ea-948b-cff0bf21eec9.png)

Observem os passos executados a partir das mensagens mostradas logo após o comando:
*Passo 1*
Creating network "cursosgbdsql_srv-postgres-network" with driver "bridge" -> Criando a network;
*Passo 2*
Creating cursosgbdsql_srv-bd-postgresql_1 ... 
Creating cursosgbdsql_srv-bd-postgresql_1 ... done
*Passo 3*
Creating cursosgbdsql_srv-pgadmin_1 ... 
Creating cursosgbdsql_srv-pgadmin_1 ... done

#### PASSO 3 – Verificando se funcionou

Após o passo 2, agora vamos verificar o resultado da execução, ou seja, o que foi criado pelo docker-compose.  com comandos Docker para verificar o que foi criado pelo docker-compose. Para isso nós vamos utilizar alguns comandos do Docker.

Primeiro vamos verificar a criação da rede (network) srv-postgres-network com o comando ``` sudo docker network ls ```. 

![Captura de tela de 2020-06-10 20-10-10](https://user-images.githubusercontent.com/29760189/84327696-6946b480-ab56-11ea-833a-f4b084f6e472.png)

Veja que tem o postgresql_pgadmin4_srv-postgres-network, que é a concatenação do nome do diretório, postgresql_pgadmin4, com o nome da network, srv-postgres-network. 
```
NETWORK ID          NAME                                       DRIVER              SCOPE
a02951c8dfaf        postgresql_pgadmin4_srv-postgres-network   bridge              local

```

Neste passo nos vamos verificar a criação do banco de dados PostgreSQL e do PGAdmin com o comando ```sudo docker-compose ps```. 
![Captura de tela de 2020-06-10 19-59-44](https://user-images.githubusercontent.com/29760189/84327122-fbe65400-ab54-11ea-8d09-d64cad6bed0f.png)


Podemos ver o banco de dados PostgreSQL na porta 15432 e o PGAdmin 4 na porta 16543.
```
                 Name                                Command              State               Ports             
----------------------------------------------------------------------------------------------------------------
postgresql_pgadmin4_srv-bd-postgresql_1   docker-entrypoint.sh postgres   Up      0.0.0.0:15432->5432/tcp       
postgresql_pgadmin4_srv-pgadmin_1         /entrypoint.sh                  Up      443/tcp, 0.0.0.0:16543->80/tcp
```

#### PASSO 4 – Testando e configurando o ambiente para ser utilizado

Em um browser (neste exemplo eu uso o Google Chrome), acessem o endereço: http://localhost:16543. Aparecerá a tela abaixo:
![Captura de tela de 2020-06-10 20-16-59](https://user-images.githubusercontent.com/29760189/84328096-65676200-ab57-11ea-83c8-d706a675d105.png)

Essa é a tela de acesso ao PGAdmin 4. 
Vamos fornecer as credenciais definidas no nosso arquivos docker-compose.yml, quais sejam:
Usuário: luisjesus.ti@gmail.com
Senha: curso
Pronto! Já estamos no painel de gerenciamento PGAdmin 4 (veja a tela abaixo)
![Captura de tela de 2020-06-10 20-19-10](https://user-images.githubusercontent.com/29760189/84328214-bb3c0a00-ab57-11ea-8fc2-619f302326d9.png)

O próximo passo é criar a conexão com a instância de banco de dados PostgreSQL.

Para isso vamos:
- Adicionar um novo Servidor (Add New Server). Aparecerá a janela abaixo. 
![Captura de tela de 2020-06-10 20-22-07](https://user-images.githubusercontent.com/29760189/84328384-31d90780-ab58-11ea-9f95-8552249db38e.png)

Para criar a conexão para acesso à instância do PostgreSQL, vamos informar:
Host name/address: srv-bd-postgresql
- Esse é o nome do serviço PostgreSQL que definimos no nosso docker-compose.yml.
Port: 5432
- Essa é a porta padrão de acesso ao container. Porta disponível na rede srv-postgres-network, e definido no nosso docker-compose.yml.
Username: postgres
- Valor padrão definido no nosso docker-compose.yml.
Password: curso
- Valor padrão definido no nosso docker-compose.yml.

Feito isso, já podemos ver, na figura abaixo, que o PGAdmin 4 acessa o banco de dados PostgreSQL.
![Captura de tela de 2020-06-10 20-26-28](https://user-images.githubusercontent.com/29760189/84328575-b1ff6d00-ab58-11ea-8d23-73c6cb0bd1eb.png)

## Referências
PostgreSQL - Docker Hub
pgAdmin 4 - Docker Hub
