# Spark com interface Python

Vamos conhecer como é a interface do PySpark

Precisamos logar com o usuário hdfs

```
su - hdfs
```

Para acessar a interface do PySpark
```
pyspark
```

Para realizar a leitura de um arquivo

```
RDDread = sc.textFile ("hdfs://ip-172-31-26-14.ec2.internal:8020/data/wordcount.txt")
```

Verificar os registros da coleção

```
RDDread.collect()
```

Recuperar o primeiro arquivo

```
RDDread.first()
```

Exemplo como recuperar 3 registros

```
RDDread.take(3)
```
