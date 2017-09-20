# Banco de dados NoSQL Apache Hbase

Vamos aprender a trabalhar com o Hbase

Precisamos logar com o usuário hdfs

 ```
 su - hdfs
 ```
 
Vamos criar uma massa de dados de exemplo, para isso vamos copiar e colar o comando no Linux

```
cat > DEC_00_SF3_P077_with_ann_noheader.csv <<EOF
8600000US00601,00601,006015-DigitZCTA,0063-DigitZCTA,11102
8600000US00602,00602,006025-DigitZCTA,0063-DigitZCTA,12869
8600000US00603,00603,006035-DigitZCTA,0063-DigitZCTA,12423
8600000US00604,00604,006045-DigitZCTA,0063-DigitZCTA,33548
8600000US00606,00606,006065-DigitZCTA,0063-DigitZCTA,10603
EOF
```

Para acessar o Hbase vamos prosseguir com o comando:

```
hbase shell
```

Vamos criar uma tabela no Hbase

```
create 'zipcode_hive', 'id', 'zip', 'desc', 'income'
```

Para listar as tabelas que existem:

```
list
```

Para descrever os metadados da tabela:

```
list 'zipcode_hive'
```

Para inserir registros no Hbase:

```
put 'zipcode_hive' , '8600000US00600' , 'zip:zip' , '00901'
put 'zipcode_hive' , '8600000US00600' , 'desc:desc1' , '009015-DigitZCTA'
put 'zipcode_hive' , '8600000US00600' , 'desc:desc2' , '0093-DigitZCTA'
put 'zipcode_hive' , '8600000US00600' , 'income:income' , '91102'
```

para listar os registros da base de dados

```
scan 'zipcode_hive'
```

Ler os registros de uma tabela


```
get 'zipcode_hive', '8600000US00600'
```

```
get 'zipcode_hive', '8600000US00600', {COLUMN => 'desc:desc2'}
```
Para sair do Hbase

```
exit
```

# Criando uma tabela Hive para ler o Hbase

Vamos acessar o hive

```
hive
```

Criar uma tabela externa apontada para a tabela do Hbase

```
CREATE EXTERNAL TABLE ZIPCODE_HBASE (
key STRING,
zip STRING,
desc1 STRING,
desc2 STRING,
income STRING) 
ROW FORMAT DELIMITED 
FIELDS TERMINATED BY ',' 
STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler' 
WITH SERDEPROPERTIES ("hbase.columns.mapping" = ":key,zip:zip,desc:desc1,desc:desc2,income:income") 
TBLPROPERTIES("hbase.table.name" = "zipcode_hive");
```

Vamos sair do Hive

```
exit;
```

# Carregar dados no Hbase utilizando o Pig

```
pig -x mapreduce
```

vamos executar os seguintes comandos no Grunt

```
copyFromLocal DEC_00_SF3_P077_with_ann_noheader.csv ziptest.csv
A = LOAD 'ziptest.csv' USING PigStorage(',') as (id:chararray, zip:chararray, desc1:chararray, desc2:chararray, income:chararray); 
STORE A INTO 'hbase://zipcode_hive' USING org.apache.pig.backend.hadoop.hbase.HBaseStorage('zip:zip,desc:desc1,desc:desc2,income:income');
```

Vamos sair do Pig

```
quit
```

# Agora pelo Impala, vamos realizar uma consulta aos dados

```
impala-shell
```

```
select * from zipcode_hbase limit 4
```
