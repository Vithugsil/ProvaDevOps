Explicação de cada dockerfile:

ApiProdutos(node):
FROM node:18 -> especifica a imagem base
WORKDIR /app -> Define qual será a pasta onde o container trabalhara
COPY package*.json . -> copia o package.json para a  pasta do conteiner
RUN npm install -> executa o comando npm i para instalar as dependencias que estão no package.json
COPY . . -> copia todo o conteudo da pasta do pc para o container
EXPOSE 3001 -> Expõe a porta 3001 do container
CMD ["node", "app.js"] -> executa node app.js no terminal do container para executar o serviço

ApiPedidos(python):
FROM python:3.11-slim -> especifica a imagem base
WORKDIR /app -> Define qual será a pasta onde o container trabalhara
COPY requirements.txt ./ copia o txt com as dependencias necessárias para o projeto na pasta de trabalho do container.
RUN pip install --no-cache-dir -r requirements.txt -> executa o comando de pip install lendo o conteúdo do arquivo txt sem considerar o cache
COPY . . -> copia todo o conteudo da pasta do pc para o container
EXPOSE 3002 -> Expõe a porta 3002 do container
CMD ["python", "app.py"] -> executa node python app.py no terminal do container para executar o serviço

ApiPagamentos(PHP):
FROM php:8.2-cli -> especifica a imagem base
WORKDIR /app -> Define qual será a pasta onde o container trabalhara
COPY . . -> copia todo o conteudo da pasta do pc para o container
EXPOSE 3003 -> Expõe a porta 3002 do container
CMD ["php", "-S", "0.0.0.0:3003"] -> executa o comando no terminal para executar o serviço utilizando o servidot built-in do php


___________________________________________
Docker compose:
services: -> define os serviços
  products:
    build:
      context: ./api-produtos -> mostra aonde ele executara o build do docker file
    container_name: products -> nomeia o container
    ports:
      - "3001:3001" -> Aponta a porta do host pro container

  orders:
    build:
      context: ./api-pedidos -> mostra aonde ele executara o build do docker file
    container_name: orders -> nomeia o container
    ports:
      - "3002:3002" -> Aponta a porta do host pro container
    

  payment:
    build:
      context: ./api-pagamento -> mostra aonde ele executara o build do docker file
    container_name: payment -> nomeia o container
    ports:
      - "3003:3003" -> Aponta a porta do host pro container
    

  redis:
    image: redis:7 -> define a imagem base 
    container_name: redis - -> nomeia o container
    ports:
      - "6379:6379" -> Aponta a porta do host pro container

  db:
    image: mysql:8 -> define a imagem base 
    container_name: db  -> nomeia o container
    environment: -> define as variaveis de ambiente 
      MYSQL_ROOT_PASSWORD: example
      MYSQL_DATABASE: ecommerce
    ports:
      - "3307:3307" -> Aponta a porta do host pro container

**Alguns serviços não possuem apontamento de onde esta o dockerfile, redis e db, pois ele se utiliza unicamente da imagem base.
