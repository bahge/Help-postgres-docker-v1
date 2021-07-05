# Arquivos
  
Aqui serão descritos os arquivos que estão presentes no diretório. E algumas funcionalidades.

## docker-compose.yml

```yml
 version: '3.1' 
```  
> Versão do docker-compose, no caso da versão 3.1 como especificado na [documentação](https://docs.docker.com/compose/compose-file/) requerem a release do Docker Engine 1.13.1+.
  
```yml
services:

  db:
    image: postgres
    restart: always
    environment:
      POSTGRES_PASSWORD: password
      POSTGRES_USER: user
      POSTGRES_DB: user
    ports:
      - "15432:5432"
    volumes:
      - ./PostgreSQL:/var/lib/postgresql/data 
    networks:
      - postgres-network
      
  pgadmin:
    image: dpage/pgadmin4
    environment:
      PGADMIN_DEFAULT_EMAIL: "user@user.com"
      PGADMIN_DEFAULT_PASSWORD: "pg@dmin"
    ports:
      - "16300:80"
    depends_on:
      - db
    networks:
      - postgres-network
```
> Cria o container postgres com a configuração:  
> 
> 
> **Nome:** db  
>
> **Variáveis de ambiente**  
>>   **POSTGRES_PASSWORD:** password  
>>   ```Define a senha do postgres.```;  
>>   **POSTGRES_USER:** user  
>>   ```Define o usuário padrão.```;  
>>   **POSTGRES_DB:** user   
>>   ```Cria a database para o usuário.```;  
>
> **Portas:** 15432:5432   
> ```HOST:CONTAINER, As portas mencionadas são compartilhadas entre os diferentes serviços iniciados pelo docker-compose.```;   
> **Volume:** ./PostgreSQL:/var/lib/postgresql/data   
> ``` Referencia no host do hospedeiro o diretório mapeado para que este ser refletido no seu destino dentro do contêiner.```;  
> 
> **Nome:** pgadmin  
>
> **Variáveis de ambiente**  
>>   **PGADMIN_DEFAULT_EMAIL:** user@user.com  
>>   ```Define o e-mail de login do pgAdmin4.```;  
>>   **PGADMIN_DEFAULT_PASSWORD:** pg@dmin  
>>   ```Define a senha para entrar no pgAdmin.```;  
>
> **Portas:** 16300:80   
> ```HOST:CONTAINER, As portas mencionadas são compartilhadas entre os diferentes serviços iniciados pelo docker-compose.```;   
> **Depende de:** db   
> ``` Instancia o pgAdmin apenas se o postgres está ativo.```;   
>
```yml 
networks: 
  postgres-network:
    driver: bridge 
```
> Cria uma ponte de software que permite que contêineres conectados à rede postgres-network se comuniquem, mais informações acesse a [documentação](https://docs.docker.com/network/bridge/) oficial do docker. 