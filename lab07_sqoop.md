# Utilizando o Sqoop

Agora vamos realizar a ingestão de dados de toda a base que carregamos com o MySQL para o Hadoop utilizando o Sqoop

Vamos logar com o usuário hdfs
```
su - hdfs
```

Agora, vamos importar todo o database para o Hive, esse procedimento vai demorar um pouco mais. Nosso ambiente é um composto de apenas um unico servidor. Na prática este processo pode ser paralelizado.
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
