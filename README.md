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
