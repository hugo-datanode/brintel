
# Aprendendo a realizar a ingestão de dados para o Hadoop

Lembre-se de executar os comando de hadoop com o usuário hdfs do Linux

Para nosso laboratório, vamos trabalhar com dados dos convenios do governo. vamos criar um diretório com o propósito de capturar os dados baixados

```shell
mkdir data
```

Para baixar a base de dados execute:

```shell
cd data && wget -c http://portal.convenios.gov.br/images/docs/CGSIS/csv/siconv_convenio.csv.zip
```
Vamos descompactar a tabela e processar o seu conteúdo para ficar aderente ao padrão de separador de milhar

```shell
unzip siconv_convenio.csv.zip
```

```shell
cat siconv_convenio.csv | tr ',' '.' > dados_convenio.csv
```

Vamos criar um diretório no HDFS

```shell
hadoop fs -mkdir -p /data/siconv_convenio
```

Carregar os dados locais para o HDFS

```shell
hadoop fs -put  dados_convenio.csv /data/siconv_convenio
```

Verificar os arquivos carregados

```shell
hadoop fs -ls /data/siconv_convenio
```

Verificar o conteúdo do arquivo

```shell
hadoop fs -tail /data/siconv_convenio/dados_convenio.csv
```

Verificar a integridade do arquivo

```shell
md5sum dados_convenio.csv
```

```shell
hadoop fs -cat /data/siconv_convenio/dados_convenio.csv | md5sum
```
