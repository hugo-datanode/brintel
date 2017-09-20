# Execução de SQL no Hadoop

Vamos aprender como usar os componentes do Hadoop para a execução de códigos SQL, como uma analogia a um banco de dados relacional.

Precisamos acessar a camada shell do Linux para acessar a CLI das ferramentas SQL-on-Hadoop

Vamos logar com o usuário hdfs

```sh
su - hdfs
```

Para logar com o cliente do Hive, vamos executar o seguinte comando:

```sh
beeline
```

Vamos conectar no nosso servidor local com o usuário 'hdfs' e senha 'hdfs'

```sh
!connect jdbc:hive2://localhost:10000
```

Com o acesso ao cliente Beeline, podemos executar comandos SQL. A sintaxe do Hive é bem semelhante ao MySQL.

Vamos criar um database novo

```sh
create database brintell;
```

Para exibir todos os banco de dados dos sistema, execute:

```sh
show databases;
```

Vamos acessar o database novo

```sh
use brintell;
```

Agora, podemos criar tabelas dentro do novo database, vamos criar uma tabela para mapear o aquivo /data/siconv_convenio/dados_convenio.csv

Essa modalidade de tabela é chamada EXTERNAL TABLE, ela cria apenas uma camada de metadados sobre o arquivo para que possamos mapear os seus atributos.

```sql
CREATE EXTERNAL TABLE `brintell`.`siconv_convenio_ext`
(
  `NR_CONVENIO` string ,
  `ID_PROPOSTA` string ,
  `DIA` bigint ,
  `MES` bigint ,
  `ANO` bigint ,
  `DIA_ASSIN_CONV` string ,
  `SIT_CONVENIO` string ,
  `SUBSITUACAO_CONV` string ,
  `SITUACAO_PUBLICACAO` string ,
  `INSTRUMENTO_ATIVO` string ,
  `IND_OPERA_OBTV` string ,
  `NR_PROCESSO` string ,
  `UG_EMITENTE` string ,
  `DIA_PUBL_CONV` string ,
  `DIA_INIC_VIGENC_CONV` string ,
  `DIA_FIM_VIGENC_CONV` string ,
  `DIAS_PREST_CONTAS` bigint ,
  `DIA_LIMITE_PREST_CONTAS` string ,
  `QTDE_CONVENIOS` bigint ,
  `QTD_TA` bigint ,
  `QTD_PRORROGA` string ,
  `VL_GLOBAL_CONV` double ,
  `VL_REPASSE_CONV` double ,
  `VL_CONTRAPARTIDA_CONV` double ,
  `VL_EMPENHADO_CONV` double ,
  `VL_DESEMBOLSADO_CONV` double ,
  `VL_SALDO_REMAN_TESOURO` double ,
  `VL_SALDO_REMAN_CONVENENTE` double ,
  `VL_RENDIMENTO_APLICACAO` double ,
  `VL_INGRESSO_CONTRAPARTIDA` double ) 
  ROW FORMAT   DELIMITED
  FIELDS TERMINATED BY '\u0059'
  STORED AS TEXTFILE
  location '/data/siconv_convenio'
  TBLPROPERTIES("skip.header.line.count" = "1");
```
Agora podemos executar instruções SQL para analisar o arquivo. Vamos executar alguns exemplos:

Vamos realizar uma contagem de linhas:

```sh
select count(*) from siconv_convenio_ext;
```

Podemos executar consultas com a sintaxe muito proxíma ao SQL ANSI:

```sh
select SIT_CONVENIO, SUM(VL_REPASSE_CONV) from siconv_convenio_ext WHERE ANO = 2017 GROUP BY SIT_CONVENIO;
```

Vamos criar uma nova tabela que é gerenciada pelo Hive


```sql
CREATE TABLE `brintell`.`siconv_convenio_pqt`
(
  `NR_CONVENIO` string ,
  `SIT_CONVENIO` string ,
  `SITUACAO_PUBLICACAO` string ,
  `INSTRUMENTO_ATIVO` string ,
  `NR_PROCESSO` string ,
  `UG_EMITENTE` string ,
  `QTDE_CONVENIOS` bigint ,
  `VL_GLOBAL_CONV` double ,
  `VL_REPASSE_CONV` double ,
  `VL_CONTRAPARTIDA_CONV` double ,
  `VL_EMPENHADO_CONV` double ,
  `VL_DESEMBOLSADO_CONV` double 
 ) 
 STORED AS PARQUET;
```

Vamos carregar a nova tabela a partir do conteúdo da primeira

```sql
INSERT INTO siconv_convenio_pqt
SELECT
  `NR_CONVENIO`,
  `SIT_CONVENIO`,
  `SITUACAO_PUBLICACAO`,
  `INSTRUMENTO_ATIVO`,
  `NR_PROCESSO`,
  `UG_EMITENTE`,
  `QTDE_CONVENIOS`,
  `VL_GLOBAL_CONV`,
  `VL_REPASSE_CONV`,
  `VL_CONTRAPARTIDA_CONV`,
  `VL_EMPENHADO_CONV`,
  `VL_DESEMBOLSADO_CONV`
FROM siconv_convenio_ext
LIMIT 10;
```
Vamos verificar os registros da nova tabela

```sql
SELECT * FROM siconv_convenio_pqt;
```

Vamos apagar a tabela externa e verificar se o arquivo ainda permanece

```sql
drop table siconv_convenio_ext;
```

Para sair do beeline execute

```
!quit
```

Agora, vamos verificar se o arquivo ainda permanece. 

```
hadoop fs -ls /data/siconv_convenio
```

O Hive apaga apenas os metadados para a tabela, não o arquivo de origem.
