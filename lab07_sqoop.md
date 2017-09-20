# Utilizando o Sqoop

Agora vamos realizar a ingestão de dados de toda a base que carregamos com o MySQL para o Hadoop utilizando o Sqoop

Vamos logar com o usuário hdfs
```
su - hdfs
```

Agora, vamos importar todo o database para o Hive, esse procedimento pode demorar um pouco por conta do nosso ambiente não está paralelizado
```
sqoop import-all-tables \
    -m 1 \
    --connect jdbc:mysql://localhost:3306/sakila  \
    --username=root \
    --password=12345678 \
    --compression-codec=snappy \
    --as-parquetfile \
    --warehouse-dir=/user/hive/warehouse \
    --hive-import \
	  --hive-database sakila
```

Podemos agora navegar nos dados através do Hadoop
