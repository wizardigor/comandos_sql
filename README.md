# COMANDOS SQL COM PHP E MYSQL

## - CRIANDO A TABELA NO BANCO DE DADOS
~~~mysql
CREATE TABLE pessoas (
id INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
cpf_cnpj varchar(16)
nome varchar(100),
profissao varchar(50),
endereco text);
~~~

## - PREPARAÇÃO PARA CONECTAR AO BANCO
~~~php
private $host= "localhost";
private $database= 'nome_do_banco';
private $user= "root";
private $senha= '';

function setDB(){
  $con= mysqli_connect($this-> host, $this->user, $this->senha, $this->database);
  mysqli_set_charset($con, "UTF8");
  if(mysqli_connect_errno()){
    echo "Erro ao tentar se conectar". mysqli_connect_errno();
  }
  return $con;
}
~~~~

## - PARA INSERIR DADOS AO BANCO:
~~~php
"INSERT INTO nome_da_tabela (campo_da_tabela) VALUE (valor_a_inseriri) WHERE condicao"
~~~

### EXEMPLO:
~~~php
$query_insert= "INSERT INTO pessoas (cpf_cnpj, nome, profissao, endereco) 
                VALUES('$cpf_cnpj', '$nome','$profissao', '$endereco'
                WHERE nome = 'João'";
mysqli_query(setDB(), $query_insert) or die('Não foi possivel inserir os dados');
~~~

## - PARA USAR UMA INFORMAÇÃO DO BANCO DE DADOS (CONSULTA):
~~~php
"SELECT nome_da_tabela FROM campo_da_tabela WHERE condicao"
~~~~

### EXEMPLO:
~~~php
$query_select = "SELECT * FROM pessoas WHERE nome = 'João' AND profissao = 'Programador'";
mysqli_query(setDB(), $query_select) or die("Erro ao selecionar");
~~~

## - ALTERAR UMA INFORMAÇÃO DO BANCO DE DADOS:
~~~php
"UPDATE nome_da_tabela SET campo_da_tabela WHERE condicao "
~~~

### EXEMPLO:
~~~php
$query_update = "UPDATE pessoas 
                SET nome = 'Maria', profissao = 'Analista' 
                WHERE id = 101001 AND cpf_cnpj = '12044201220012'";
mysqli_query(setDB(), $query_update) or die(mysqli_error(setDB()));
~~~

## - APAGAR UMA INFORMAÇÃO DO BANCO DE DADOS:
*apaga toda a tupla (linha) incluindo o registro, não podendo mais acessar o identificador, mantem o auto incremente dando continuidade a sequencia sem sobrescrever*
~~~php
"DELETE FROM nome_da_tabela WHERE condicao"
~~~

### EXEMPLO:
~~~php
$query_delete = "DELETE FROM pessoas WHERE nome = 'João'";
mysqli_query(setDB(), $query_delete) or die(mysqli_error(setDB()));
~~~

## - APAGAR UMA TABELA E TODOS OS SEUS DADOS
~~~php
"DROP TABLE nome_da_tabela"
~~~

### EXEMPLO
~~~php
$query_drop = "DROP TABLE pessoas";
mysqli_query(setDB(), $query_drop) or die(mysqli_error(setDB()));
~~~

## - LIMPAR TODOS OS DADOS DE UMA TABELA
*apaga todos os dados da tabela zerando o auto incremente*
~~~php
"TRUNCATE TABLE 'nome_da_tabela'"
~~~

### EXEMPLO
~~~php
$query_truncate = "TRUNCATE TABLE 'pessoas'";
mysqli_query(setDB(), $query_truncate) or die(mysqli_error(setDB()));
~~~

## - MESCLAR TABELAS
*consutar dados em duas tabelas distintas*
~~~php
"SELECT A.nome_da_coluna_a, B.nome_da_coluna_b
 FROM nome_da_tabela_a AS CODENOME_DA_TABELA_A
 INNER JOIN nome_da_tabela_b AS CODENOME_DA_TABELA_B
 ON condição_da_tabela_b
 //caso tenha mais tabelas é só repetir do INNER ate ON e sua condição//
 WHERE condição_da_tabela_a"
~~~

### EXEMPLO:
~~~php
$query_select = "SELECT L.Nome_Livro, A.Nome_autor, E.Nome_Editora, L.Preco_Livro
                FROM tbl_Livro AS L
                INNER JOIN tbl_autores AS A
                ON L.ID_autor = A.ID_autor
                INNER JOIN tbl_editoras AS E
                ON L.ID_editora = E.ID_editora
                WHERE E.Nome_Editora LIKE 'Saraiva'
                ORDER BY L.Preco_Livro DESC";

mysqli_query(setDB(), $query_select) or die("Erro ao selecionar");
~~~

## - OPERADOR LIKE
*O operador **LIKE** é usado para localizar um valor dentro de um campo textual*
~~~php
"SELECT campo_da_tabela FROM nome_da_tabela WHERE campo LIKE 'valor'"
~~~

### EXEMPLOS 1
~~~php
"SELECT nome, profissao, endereco FROM pessoas WHERE nome LIKE 'João'"
~~~
tambem podemos usar o caractere **%** que será um coringa nas buscas, representando o valor que falta(ou que você não saiba qual é mas pode estar lá).
No exemplo acima temos **'João'** como valor de busca, veja como se comporta a busca usando esse valor e o **%**
- **João:** Nesse caso, serão retornados todos os registros que contêm no campo buscado exatamente o "João" informado no filtro. O funcionamento aqui é equivalente a utilizar o operador de igualdade (=);
- **%João%:** Serão retornados os registros que contêm no campo buscado o **"João"** informado. Por exemplo, podemos buscar os nomes que contêm **"Santos"**, ou que contêm uma sílaba ou letra específica. O registro com nome **"Luis da Silva"**, por exemplo, contém o termo **"da"**, então atenderia ao filtro **'%da%'**;
- **%João:** Serão retornados os registros cujo valor do campo filtrado termina com o **"João"** informado. O **%**, nesse caso, indica que pode haver qualquer valor no começo do campo, desde que ele termine com o **"João"**. Por exemplo, o registro com nome **"Luis da Silva"** atenderia ao filtro **'%Silva'**;
- **João%:** Serão retornados os registros cujo valor do campo filtrado começa com o **"João"** informado. Dessa vez, o **%** indica que após o **"João"** pode haver qualquer valor. Por exemplo, o registro com nome **"Luis da Silva"**, atenderia ao filtro **'Luis%'**.

### EXEMPLO 2 (USANDO %)
~~~php
"SELECT * FROM pessoa WHERE nome LIKE 'J%'"
~~~
Você tambem pode usar o underscore ou sublinhado **(\_)** e ainda juntar com o **%**. O underscore indica a quantidade de casas/caracteres antes ou depois do texto buscado.
- **'\_este'**: Filtra os registros que contém 1 caractere qualquer no começo e em seguida o termo **'este'**. Por exemplo, seriam retornados registros contendo o valor **'teste'**, **'peste'**, **'veste'**;
- **'b_m'**: Filtra os registros que comecem com a letra **"b"**, contenham 1 caractere em seguida, e depois a letra **"m"**. Nesse caso, atenderiam a esse filtro, por exemplo, os valores **"bom"**, **"bem"**, **"bPm"**, etc.
- **'\_u%'**: Filtra os registros cujo campo especificado comece com um caractere qualquer, em seguida contenha uma letra **"u"**, e depois qualquer valor. Por exemplo, os valores **"Luis da Silva"** e **"Gustavo"** atenderiam a esse filtro.

### EXEMPLO 3 (USANDO \_UNDESCORE)
~~~php
"SELECT * FROM pessoa WERE nome LIKE '_u%'"
~~~

## - OPERADOR IN
*O operador IN é utilizado quando desejamos consultar uma tabela, filtrando o valor de um de seus campos a partir de uma lista e possibilidades. Enquanto o operador de comparação de igualdade (=) avalia se os dois valores são iguais, o **IN** permite verificar se o valor de um coluna/campo se encontra em uma lista.*
Sua sintaxe é a seguinte:
~~~php
"SELECT colunas/campo FROM tabela WHERE campo IN (valor1, valor2, valor3)"
~~~
### EXEMPLO
~~~php
"SELECT * FROM pessoa WHERE id IN (2, 3, 7)";
~~~
*Nesse caso, filtramos apenas os as registros da tabela **pessoa** que possuem o **id** igual a **2, 3, ou 7.***

## - OPERADOR BETWEEN
*Esse operador é usado quando precisamos recuperar as linhas de uma tabela cujo valor de um campo encontra-se em um intervalo especificado. Esse tipo de consulta é muito comum quando queremos filtrar os dados por intervalos de datas.*
~~~php
"SELECT campos FROM tabela WHERE campo BETWEEN inicio_intervalo AND fim_intervalo"
~~~
### EXEMPLO
~~~php
"SELECT * FROM pessoa WHERE nascimento BETWEEN '01-01-1981' AND '31-12-1990'";
~~~
