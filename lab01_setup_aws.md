# Acesso ao servidor remoto por SSH

O primeiro passo é obter chave de acesso privada .pem, com ela você poderá acessar o ambiente remoto para o treinamento com um cliente ssh

```shell
ssh root@ec2-XXX-XXX-XXX-XXX.compute-1.amazonaws.com -i brintell.pem
```

Caso esteja utilizando o Linux para realizar o acesso remoto, será necessário alterar as propriedades de permissões do arquivo .pem
```shell
chmod 400 /home/usuario/brintell.pem
```

# Configuração inicial de implantação do Hadoop

Com o acesso estabelecido no ambiente, vamos realizar o setup inicial para a configuração de implantação do ecossistema Hadoop. Para isso vamos realizar alguns procedimentos para a configuração de prerequisitos do sistema operacional

Update e instalação dos pacotes do sistema

```shell
yum update
```
```shell
yum install nscd bind-utils sysstat wget unzip ntp vim 
```

# Configurar os serviços de segurança e rede

```shell
setenforce 0
```

# Criar chave de acesso para autenticação remota sem senha

```shell
ssh-keygen
```
```shell
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
```

# Copiar a chave para uso posterior, ela será necessária para autenticar no cluster

```shell
cat ~/.ssh/id_rsa
```

Seu conteúdo é semelhante ao descrito a seguir:
```
-----BEGIN RSA PRIVATE KEY-----
MIIEpQIBAAKCAQEAkLwsWNmupNCGrMyREvdp5HS0D/vEhgJCSSDuo27RzYCEC/3d
b2pd6DqSw/KJa6eOBDJnk+0zEn2/bS+k2jcmAlrGE1flJ9JqDD9iZ3jH5N85TxER
JUf0Js6qx4jwaHQH6jkLIR9nckormj7lkvpegy2/2BF07/+6bufW6DVc7n3T60fF
KVoEnSsIpQ0aqCqikAAqUOpE7Lpeocs56IZJjQ4Ddc3WxplP6YrG3y6VXz2VkW7C
o8ZCiLVOaZw/3zlfh6lgfF3zc2Xc1FUg25HD/TUf/dKTGHmXMnjFn5WRFxSHQdnW
6LU2sETTbpN95Ge7Ruxs1ptaUtWblxMft0oiKwIDAQABAoIBAAWurZsLaNTlrvPn
0CZLemfSwSMDgnK0cf/HADeAaVJFImoKHRc+uNMeQZbZ1dVZLbUyeWiQXnnyX+qc
fT9n/OEIyVAHGmMW2r0CXA2t60MsFGbrR54MFiTT5laRJMclDw5+ENbLEdel29Jh
d8fudnl1+Vs1TD8D7kDeb0yMk8p1LRhuwqg+lRnAKme8x+XsLCNfkApGv7ZppkuW
FHVfC/Hnq3FtEzICThY4VgmfFqmk/8qDwperenzJbXNNV7JTkhM1p9++zK5c2DJW
D3AggRdsQQHhabNE0Tunbqh7lbDwAxRT8fR+Y+ypeUlon8ltqxz7B/hsU53hDzDI
3KmKVNkCgYEAwM/al8DxqLnitHayfAR0lyvpwJ5YpsT7dNskRAU4iibjQRIDUyZa
hvIoRS7sLqer0gsj6TMJq929uTn8nl8570cFUrzKucgzYXoffzjv2GqJzAmTzutF
aaLga/GDP7C7BHcjIUA8EBQ00n1Sa7CVIirZ5HJjI6oZk0u0Z38h8EUCgYEAwCrc
zTIov1l0sB+1sZKww+ZBpucff5gjiAx1zj59eUKWFF0sQXQeigiOKL6S9rZouoTY
OIO7rWz9h2IWhvfDTb/r4W7q1nTSHYLHHXzUiz6QfwQtbXLl7NepoZ+FaoUfgPeV
WcfhFMt7XRYTe6lhUbUUuf6Bov0J7gsXVmBBB68CgYEAgWY9ruDnjjQKiNCsYnze
/mGTRBlBJ9NFaxxzT08trdIBbDc5kgFIeg5kpmGiUoFm19VwKV5+XCC55mibOHJy
QDqqwOdBKsPIb9/06X39wYFmr0+yKglNkWKlOOxiCEmEia+nHPauGKBm/ujqeqmM
vNyDVUTLcjEDbw48qcTxsv0CgYEAt8vHJdNjubB7tMB/bXmZ66RdAp9oNwdyZHtW
aY7HP6V6GbwLygaf9vG71iiAM8u/WzYX/+WvKW5nBofAeBKdD84Sc6k8nyVYmbUt
cHymZQ/P8Ew0jswoMWEL83O5jWoJ+bXTeO19z//W2+9zbwFP/XAuhL5xi0xtpOmi
xpCFYi0CgYEAjX2J2Y3vYGQwha5vWUigA5D9GV7m6gNUY25U7fEmxvFcJTViB61p
eD9saO5ljoJhzlj2VvtMyii4sE1YU9u56AjOpVQsBnlVXQbVQb4Wh9fV6CymIXLl
ynuGVkSm3qWOpXzCA/3TDfPORQAQlqCUqAiPiStzpq3R4gGF/xr/ICd=
-----END RSA PRIVATE KEY-----
```
# Alterar a configuração do cliente ssh para não solicitar confirmação da chave

```shell
vim /etc/ssh/ssh_config
```

Vamos alterar a seguinte configuração:

```shell
StrictHostKeyChecking no
```

# Alterar as configurações do THP do linux. Isto é um prerequisito para um melhor gerenciamento de memória

```shell
echo "never" > /sys/kernel/mm/transparent_hugepage/defrag
echo "never" > /sys/kernel/mm/transparent_hugepage/enabled
```

# Alterar as configurações do THP do linux. Isto é um prerequisito para um melhor gerenciamento de escrita em disco

```shell
sysctl -w vm.swappiness=10
```

# Baixar o script de instalação do cluster

```shell
wget https://archive.cloudera.com/cm5/installer/latest/cloudera-manager-installer.bin
```

# Alterar as permissões de execução do script de instalação do cluster

```shell
chmod u+x cloudera-manager-installer.bin
```

# Executar o script seguindo o wizard de configuração

```shell
./cloudera-manager-installer.bin
```

# O restante da configuração de implantação será executado através do seu navegador no endereço indicado pelo assistente de configuração

http://XXX.XXX.XXX.XXX:7180/cmf/login







