# Configuração do banco de dados de exemplo

Vamos aprender a carregar dados a partir de uma base de dados relacional, para esse exemplo vamos utilizar uma instância do MySQL

Precisamos conectar com o usuário root para realizar as configurações do MySQL

```
su - root
```

Vamos instalar uma instância do MySQL

```
yum install mariadb mariadb-server
```

Vamos configurar a instância com a senha 12345678

```
mysql_secure_installation
```

Vamos realizar os seguintes procedimentos para baixar uma base de dados de teste para simular nossos dados

```
mkdir -p /opt/data/sakila
```
```
cd /opt/data/sakila
```
```
wget -c http://downloads.mysql.com/docs/sakila-db.zip
```
```
unzip sakila-db.zip
```
```
cd sakila-db
```

Vamos carregar os dados para o MySQL

```
mysql -u root -p
```
```
source /opt/data/sakila/sakila-db/sakila-schema.sql
```
```
source /opt/data/sakila/sakila-db/sakila-data.sql
```

Precisamos garantir o acesso externo ao Sqoop para que ele possa realizar a ingestão de dados

```
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' identified by '12345678' WITH GRANT OPTION;
```

Podemos navegar um pouco na base para conhecer os dados

Para sair do MySQl

```
exit
```

Agora precisamos configurar o driver do MySQL para o sqoop, para isso vamos baixar o driver jdbc e instalar no diretório do sqoop

```
mkdir -p /opt/mariadb
```
```
cd /opt/mariadb
```
```
wget -c https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-5.1.44.tar.gz
```
```
tar -zxvf mysql-connector-java-5.1.44.tar.gz
```
```
cp mysql-connector-java-5.1.44/mysql-connector-java-5.1.44-bin.jar /var/lib/sqoop/
```
```
chown sqoop:sqoop /var/lib/sqoop 
```
```
chmod 755 /var/lib/sqoop
```
