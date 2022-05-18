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

Para configurar a estrutura do banco de dados, por exemplo, utilizar a configuração UTF-8 como padrão, deve-se utilizar os seguintes comandos ao criar o banco de dados:

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
  sexo enum('M', 'F'),
  peso decimal(5,2),
  altura decimal(3,2),
  nacionalidade varchar(20) DEFAULT 'Brasil',
  PRIMARY KEY(id)
) DEFAULT CHARSET = utf8;
```

Note que:

- AUTO_INCREMENT permite que o campo receba valores automaticamente, de acordo com a inserção de novos valores no banco e de forma que não haja repetição;
- NOT NULL é utilizado para a variável não aceitar valores nulos;
- O VARCHAR(n) recebe até n strings, enquanto o CHAR(n) completa os espaços em branco para uma string com tamanho n;
- ENUM delimitou as valores que podem ser adicionados no campo;
- Em DECIMAL(n,m) define o máximo de números que ficam à direita e à esquerda da vírgula. 
- PRIMARY KEY() define uma chave primária, a principal identificadora das observações, dessa forma, não podem existir dois registros com o mesmo valor neste campo;

Outras possibilidades de tipos para os campos são:

![Captura de Tela (127)](https://user-images.githubusercontent.com/57160675/167003960-6957a2b8-b5b8-4c48-8a3a-7fe7a9d5f936.png)

É possível conferir a estrutura final da tabela a partir do comando abaixo. Caso um banco de dados específico não esteja ativado, o nome da tabela também pode ser escrito na forma bancodedados.tabela .

``` sql
DESCRIBE tabela; 
```
Após a criação da tabela, também é possível acrescentar as instâncias. Como o campo `id` recebeu o AUTO_INCREMENT, não é necessário incluí-lo, em nacionalidade existe um valor default e caso tenha a intenção de acrescentar valores fora da ordem inicial, basta acrescentar (nome, nascimento, sexo, peso, altura, nacionalidade) após o nome da tabela.

``` sql
INSERT INTO tabela VALUES
('Laura', '1999-07-25', 'F', '50.0', '1.60', DEFAULT),
('Carlos', '1999-02-16', 'M', '72.0', '1.85', 'Brasil');
```

Caso deseje criar uma nova coluna na tabela, os comandos a seguir podem ser utilizados. Por default, a nova coluna é adicionada na última posição, mas é possível alterar isso com o comando FIRST ou AFTER. Se já existem observações na tabela, a coluna será formada por valores nulos.

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

Além de renomear a tabela.

``` sql
ALTER TABLE tabela
RENAME TO nome_novo;
```

Em caso de excluir uma coluna é possível utilizar o seguinte comando.

``` sql
ALTER TABLE tabela
DROP COLUMN profissao;
```

Se o interesse for alterar a informação de um campo em uma instância utiliza-se o comando mostrado abaixo. LIMIT define o número de alterações que podem ocorrer com o comando, pode ser interessante para evitar futuros erros.

``` sql
UPDATE tabela
SET column1 = "correção", column2 = "outra correção"
WHERE id = "cod"
LIMIT 1;
```

Para excluir linhas de uma tabela pode ser utilizada a função DELETE, que também permite a o uso da função LIMIT. Ainda é possível excluir todas as instâncias de uma tabela com a função TRUNCATE, mas mantém a estrutura da tabela.

``` sql
DELETE FROM tabela
WHERE column = "erro"
LIMIT 3;
```

E caso seja necessário apagar o banco de dados, é possível utilizar o comando a seguir (MAS MUITO CUIDADO COM ISSO).

``` sql
DROP DATABASE bancodedados; 
```

## Seleção de Observações

Com o comando SELECT é possível selecionar colunas e visualizá-las. Quando utilizamos * todas as colunas são selecionadas.

``` sql
SELECT column 
FROM tabela; 

SELECT * 
FROM tabela; 
```

É possível implementar ainda a forma como essas informações serão mostradas, por exemplo ordenando a partir de algum valor. No primeiro exemplo é a versão crescente e a segunda decrescente. Também permite a ordenação a partir de mais de uma coluna.

``` sql
SELECT column FROM tabela
ORDER BY column; 

SELECT column FROM tabela
ORDER BY column DESC; 
```

Para filtrar as linhas é utilizado o comando WHERE. Os sinais que são utilizados para as inequações são =, <, <=, >, >= ou !=, além de outros comandos específicos. No segundo exemplo a seleção ocorre para todos os valores presentes entre a sequência e no terceiro considera apenas os valores entre parênteses. Operadores lógicos também podem ser adicionados com AND e OR.

``` sql
SELECT column FROM tabela
WHERE column = 'obs'; 

SELECT column FROM tabela
WHERE column BETWEEN 2014 AND 2016; 

SELECT column FROM tabela
WHERE column IN (2014, 2016); 

SELECT column FROM tabela
WHERE column > 5 AND column2 < 10; 
```

O operador LIKE permite que a seleção seja feita observando padrões nas strings. O % indica a presença de qualquer outro caracter (inclusive de nenhum caracter), e pode ser posicionado no início, no final ou até no meio. Lembrando que o SQL não é case sensitve, e não faz distinção entre maiúsculo e minúsculo. No terceiro caso tudo é selecionado com exceção do que segue o padrão. Já o _ obriga que exista algum caracter na posição definida.

``` sql
SELECT column FROM tabela
WHERE column LIKE 'P%'; 

SELECT column FROM tabela
WHERE column LIKE '%A%'; 

SELECT column FROM tabela
WHERE column NOT LIKE 'P%'; 

SELECT column FROM tabela
WHERE column LIKE 'P%_'; 
``` 

O comando DISTINCT seleciona apenas os primeiros registros que possuem certa categoria, de forma que as categorias não se repitam.

``` sql
SELECT DISTINCT column FROM tabela; 
```

Outra função útil para a observação inicial do banco de dados é a TOP N, que mostra os n primeiros registros da tabela.

``` sql
SELECT TOP 10 * 
FROM tabela; 
```

A seguir estão algumas funções de agregação. Com COUNT é possível contar o número de registros no banco de dados. Enquanto MAX mostra a maior observação da coluna, semelhante ao MIN. Além de SUM realizar a soma dos valores da coluna definida ou a média com AVG. 

``` sql
SELECT COUNT(*) FROM tabela; 

SELECT COUNT(*) FROM tabela
WHERE column = 'obs'; 

SELECT MAX(column) FROM tabela; 

SELECT SUM(column) FROM tabela; 
```

Com o GROUP BY os comandos são aplicados considerando cada uma das categorias presente na coluna definida separadamente e um valor é calculado para cada. No primeiro exemplo o resultado mostra apenas as categorias existentes. No segundo comando, além de mostrar as classes, uma nova coluna é adicionada com o valor referente a cada classe. Após realizar um agrupamento, em caso de realizar um filtro nas categorias definidas, deve-se utilizar o HAVING.

``` sql
SELECT column FROM tabela
GROUP BY column; 

SELECT column, COUNT(*) FROM tabela
GROUP BY column;

SELECT column, COUNT(*)  FROM tabela
WHERE column2 = 'obs'
GROUP BY column;

SELECT column, COUNT(*) FROM tabela
GROUP BY column
ORDER BY COUNT(*);

SELECT column, COUNT(*) FROM tabela
GROUP BY column
HAVING column > 10
ORDER BY COUNT(*);
```

Ainda é possível nomear essas novas relações quando realizamos a consulta, como pode ser visto a seguir.

``` sql
SELECT column, COUNT(*) AS "Frequência"
FROM tabela
GROUP BY column; 
```

Outras funções que permitam o tratamento dos dados podem ser observadas. Com o comando DATEPART pode ser realizada a extração de dados em datas. Enquanto o comando CONCAT concatena strings de colunas. Os comandos UPPER e LOWER fazem com que todas as letras da string fiquem maiúsculas ou minúsculas, respectivamente. E para obter o tamanho da string, pode-se utilizar a função LEN. SUBSTRING permite a seleção de uma parte da string definida. E por fim, também é possível substituir um padrão por outro com REPLACE.

```sql
SELECT DATEPART(DAY, data) AS Dia, DATEPART(MONTH, data) AS Mes, DATEPART(YEAR, data) AS Ano
FROM tabela

SELECT CONCAT(FirstName, LastName) AS Nome 
FROM tabela

SELECT UPPER(FirstName) AS Maiusculo, LOWER(LastName) AS Minusculo 
FROM tabela

SELECT FirstName, LEN(FirstName) AS Tamanho  
FROM tabela

SELECT FirstName, SUBSTRING(FirstName, 1, 3) AS TresPrimeiras
FROM tabela 

SELECT ProductNumber, REPLACE(ProductNumber, '-','#') AS Trocado
FROM tabela
```

Utilizando um SELECT dentro de outro SELECT. Nesse caso os valores utilizados para agrupar são apenas maiores que a média geral. Também conhecido como subquery.

```sql
SELECT column, COUNT(*) FROM tabela
GROUP BY column
HAVING column > (SELECT AVG(column) FROM tabela)
```

## Junção de Tabelas

Primeiro, para realizar a adição de uma chave estrangeira, utilizando a chave de outra tabela, é possível utilizar:

``` sql
ALTER TABLE tabela
ADD FOREIGN KEY (idtabela2) 
REFERENCES tabela2(id);
```

A junção entre dois bancos de dados pode ser feita da seguinte forma. Se o ON não é utilizado, todos os registros são ligados a todos os ids da segunda tabelas e ocorre uma repetição. É importante definir de qual tabela a coluna pertence para evitar que colunas com o mesmo nome sejam confundidas.

```sql
SELECT tabela1.id, tabela1.nome, tabela2.id, tabela2.info
FROM tabela1 JOIN tabela2 
ON tabela1.idtabela2 = tabela2.id;
```

Outros tipos de JOIN são:

![image](https://user-images.githubusercontent.com/57160675/167412801-2f57b694-7b6a-445d-8dda-51af46174415.png)

Também é possível utilizar abreviações no comando, afim de diminuir o tamanho do código.

```sql
SELECT t1.id, t1.nome, t2.id, t2.info 
FROM tabela1 t1
JOIN tabela2 t2
ON t1.idtabela2 = t2.id;
```

E para encadear uma nova junção, segue um novo exemplo:

```sql
SELECT * FROM tabela1 t1
JOIN tabela2 t2
ON t1.idtabela2 = t2.id
JOIN tabela3 t3
ON t1.idtabela3 = t3.id;
```

Continua na parte de UNION
