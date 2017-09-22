# Engine de processamento Spark 

Vamos usar um exemplo clássico de uso do Hadoop: word count

Precisamos nos conectar com o super usuário do Hadoop - hdfs

```
su - hdfs
```

Vamos criar um arquivo de exemplo:

```
cat > wordcount.txt <<EOF
Hadoop is an elephant
Hadoop is as yellow as can be
Oh what a yellow fellow is Hadoop
EOF
```

Vamos inserir o arquivo no HDFS

```
hadoop fs -put wordcount.txt /data 
```

Para acessar o console Scala do Spark, digite:

```
spark-shell
```

Vamos executar um programa simples para realizar a contagem de palavras a partir de um arquivo de texto:


Vamos carregar um arquivo para uma variável
```
val arquivo = sc.textFile("hdfs://ip-XXX-XXX-XXX-XXX.ec2.internal:8020/data/wordcount.txt")
```

Vamos realizar o MapReduce em uma única linha
```
val counts = arquivo.flatMap(line => line.split(" ")).map(word => (word, 1)).reduceByKey(_ + _)
```

Vamos salvar o resultado em um arquivo
```
counts.saveAsTextFile("hdfs://ip-XXX-XXX-XXX-XXX.ec2.internal:8020/data/spark")
```

Para sair do Spark (Scala)
```
exit
```

Vamos verificar o resultados do processamento

```
hadoop fs -ls /data/spark
```

Vamos verificar o conteúdo de um dos arquivos resultantes do processo de Map

```
hadoop fs -tail /data/spark/part-00000
```
