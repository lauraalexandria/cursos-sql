# Comandos básicos de SQL

- Um banco de dados é um conjunto de tabelas;
- Uma tabela é um conjunto de instâncias;
- Uma instância é composta de campos/características;

DUMP é uma cópia do banco de dados e contém todos os comandos necessários para a criação dos dados.

## Para criar os esquemas e dados

Para criar um esquema de banco de dados:

``` sql
CREATE DATABASE bancodedados; 
```

Para configurar a estrutura do banco de dados, como por exemplo utilizar a configuração UTF-8 como padrão, ao criar o banco de dados é possível utilizar os comandos:

``` sql
CREATE DATABASE bancodedados
DEFAULT CHARACTER SET utf8
DEFAULT COLLATE utf8_general_ci;
```

Para criar uma tabela em um esquema é possível utilizar o comando a seguir. Note que IF NOT EXISTS foi utilizado para evitar a sobreposição de tabelas e ao final também foi especificado o padrão UTF-8:

``` sql
USE bancodedados; 
CREATE IF NOT EXISTS TABLE tabela (
  id int NOT NULL AUTO_INCREMENT,
  nome varchar(30) NOT NULL,
  nascimento date,
  sexo enum("M", "F"),
  peso decimal(5,2),
  altura decimal(3,2),
  nacionalidade varchar(20) DEFAULT "Brasil",
  PRIMARY KEY(id)
) DEFAULT CHARSET = utf8;
```

Note que:

- AUTO_INCREMENT permite que o campo recebe valores automaticamente, de acordo com a inserção no banco e sem repetição;
- NOT NULL é utilizado para a variável não aceitar valores nulos;
- O VARCHAR(n) recebe até n strings, enquanto o CHAR(n) completa os espaços em branco para uma string com tamanho n;
- ENUM delimitou as valores que podem ser adicionados no campo;
- Em DECIMAL(n,m) define o máximo de números que ficam a direita e a esquerda da vírgula. 
- PRIMARY KEY() define uma chave primária, a principal identificadora das observações, dessa forma, não podem existir dois registros com o mesmo valor nesse campo;

Outras possibilidades de tipos para os campos são:

![Captura de Tela (127)](https://user-images.githubusercontent.com/57160675/167003960-6957a2b8-b5b8-4c48-8a3a-7fe7a9d5f936.png)

Também é possível conferir a estrutura final da tabela a partir do comando:

``` sql
DESCRIBE tabela; 
```
Após a criação da tabela, também é possível acrescentar as instâncias. Como o campo `id` recebeu o AUTO_INCREMENT, não é necessário incluí-lo, em nacionalidade existe um valor default e caso tenha a intenção de acrescentar valores fora da ordem inicial, basta acrescentar (nome, nascimento, sexo, peso, altura, nacionalidade) após o nome da tabela.

``` sql
INSERT INTO tabela VALUES
('Laura', '1999-07-25', 'F', '50.0', '1.60', DEFAULT),
('Carlos', '1999-02-16', 'M', '72.0', '1.85', 'Brasil');
```

No caso de criar uma nova coluna na tabela. Por default, a nova coluna é adicionada na última posição, mas é possível alterar isso com o comando FIRST ou AFTER. Se já existem observações na tabela, a coluna será formada por valores nulos.

``` sql
ALTER TABLE tabela
ADD COLUMN profissao VARCHAR(10) AFTER nome;
```

A função anterior também permite alterar as configurações iniciais da tabela, como os tipos das colunas.

``` sql
ALTER TABLE tabela
MODIFY COLUMN profissao VARCHAR(20) NOT NULL DEFAULT '';
```

Essa segunda versão também altera o nome da coluna.

``` sql
ALTER TABLE tabela
CHANGE COLUMN profissao prof VARCHAR(20) NOT NULL DEFAULT '';
```

Além de renomear a tabela:

``` sql
ALTER TABLE tabela
RENAME TO nome_novo;
```

Em caso de excluir uma coluna é possível utilizar:

``` sql
ALTER TABLE tabela
DROP COLUMN profissao;
```

Se o interesse for alterar a informação de um campo em uma instância utiliza-se o comando mostrado abaixo.  LIMIT define o número de alterações que podem ocorrer com o comando, pode ser interessante para evitar futuros erros.

``` sql
UPDATE tabela
SET column1 = "correção", column2 = "outra correção"
WHERE id = "cod"
LIMIT 1;
```

Para excluir linhas de uma tabela pode ser utilizada a função DELETE, que também permite a o uso da função LIMIT. Ainda é possível exluir todas as instâncias de uma tabela com a função TRUNCATE, mas mantém a estrutura da tabela.

``` sql
DELETE FROM tabela
WHERE column = "erro"
LIMIT 3;
```

E caso seja necessário apagar o banco de dados, é possível utilizar o comando (MAS MUITO CUIDADO COM ISSO):

``` sql
DROP DATABASE bancodedados; 
```




