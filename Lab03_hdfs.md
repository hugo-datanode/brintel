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
