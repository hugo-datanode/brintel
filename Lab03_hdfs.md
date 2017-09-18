# Aprendendo a usar o HDFS

O HDFS é o componente do Hadoop responsável pelo armazenamento distribuído dos dados
Vamos acessar o ambiente pelo SSH, conforme descrito no laboratório 01

Como no Linux, o HDFS possui um usuário administrador capaz de realizar todas as atividades. Vamos interagir com a plataforma através deste usuário: hdfs
Para logar com usuário hdfs:

```shell
su - hdfs
```

Alguns comandos com o hdfs para familiarizar com o ambiente, ele é bem semelhante aos comandos de file system do Linux e segue um padrão POSIX Like.

Listar um diretório
```shell
hadoop fs -ls /user
```

Criar um diretório
```shell
hadoop fs -mkdir /bigdata
```

Criar um arquivo vazio, para teste
```shell
hadoop fs -cp /bigdata/arq1
```

Copiar arquivos 
```shell
hadoop fs -cp /bigdata/arq1 /bigdata/arq2
```

Deletar um arquivo
```shell
hadoop fs -rm /bigdata/arq1
```

Baixar um arquivo
```shell
hadoop fs -rm /bigdata/arq1
```

# Aprendendo a realizar a ingestão de dados para o Hadoop

baixar base de dados

mkdir data

cd data

wget -c http://portal.convenios.gov.br/images/docs/CGSIS/csv/siconv_convenio.csv.zip

unzip siconv_convenio.csv.zip

cat siconv_convenio.csv | tr ',' '.' > dados_convenio.csv

hadoop fs -ls /

hadoop fs -mkdir -p /data/siconv_convenio



hadoop fs -put  dados_convenio.csv /data/siconv_convenio

hadoop fs -ls /data/siconv_convenio

hadoop fs -tail /data/siconv_convenio/dados_convenio.csv


hadoop fs -checksum /data/siconv_convenio/dados_convenio.csv

md5sum dados_convenio.csv

hadoop fs -cat /data/siconv_convenio/dados_convenio.csv | md5sum
