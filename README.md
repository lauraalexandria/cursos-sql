# Comandos básicos de SQL

- Um banco de dados é um conjunto de tabelas;
- Uma tabela é um conjunto de instâncias;
- Uma instância é composta de campos/características;

## Para criar os esquemas e dados

Para criar um esquema de banco de dados:

``` sql
CREATE DATABASE bancodedados; 
```
Para criar uma tabela em um esquema:

``` sql
USE bancodedados; 
CREATE TABLE tabela (
  nome varchar(30),
  idade tinyint,
  sexo char(1),
  peso float,
  altura float,
  nacionalidade varchar(20)
);
```
Outras possibilidades de tipos para os campos são:

![Captura de Tela (127)](https://user-images.githubusercontent.com/57160675/167003960-6957a2b8-b5b8-4c48-8a3a-7fe7a9d5f936.png)

Também é possível conferir a estrutura final da tabela a partir do comando:

``` sql
DESCRIBE tabela; 
```

