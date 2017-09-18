# Configurar os recursos do cluster

Vamos acessar o serviço de configuração para otimizar a execução das cargas de trabalho dos exercícios de laboratório no cluster

http://XXX.XXX.XXX.XXX:7180

Agora podemos renomear o cluster com seu nome para facilitar a sua identificação

```
Cluster 1 > [v] > Renomear cluster
```
Vamos navegar no Cloudera Manager um pouco para conhecer o ambiente do ecossistema e verificar as mensagens de configuração

Vamos alterar as configurações do componente YARN, que é responsável pelo balanceamento dos recursos do cluster

```
Buffer de memória de classificação de E/S (MiB)
mapreduce.task.io.sort.mb
16 MiB

Memória do ApplicationMaster
yarn.app.mapreduce.am.resource.mb 128 MiB

Núcleos de CPUs virtuais do ApplicationMaster
yarn.app.mapreduce.am.resource.cpu-vcores 1

Tamanho máximo do heap Java do ApplicationMaster 50 MiB

Memória da tarefa map
mapreduce.map.memory.mb 128 MiB

Núcleos virtuais de CPUs de tarefa de mapas
mapreduce.map.cpu.vcores 1

Memória da tarefa reduce
mapreduce.reduce.memory.mb 128MiB

Núcleos virtuais de CPU de tarefa de redução
mapreduce.reduce.cpu.vcores 1

Tamanho máximo do heap da tarefa map 50 MiB

Tamanho máximo do heap da tarefa reduce 50 MiB

Tamanho do heap do Java cliente em bytes 50 MiB

Tamanho em bytes do heap Java do JobHistory Server 50 MiB

Tamanho em bytes do heap Java do NodeManager 50 MiB

Memória para contêineres
yarn.nodemanager.resource.memory-mb 3 GiB

Tamanho em bytes do heap Java do ResourceManager 50 MiB

Memória mínima do contêiner
yarn.scheduler.minimum-allocation-mb 1 MiB

Incremento de memória do contêiner
yarn.scheduler.increment-allocation-mb 512 MiB

Máximo de memória do contêiner
yarn.scheduler.maximum-allocation-mb 2.75 GiB

Mínimo de núcleos de CPU virtuais do contêiner
yarn.scheduler.minimum-allocation-vcores 1 

Incremento de núcleos de CPU virtuais do contêiner
yarn.scheduler.increment-allocation-vcores 1

Máximo de núcleos de CPU virtuais do contêiner
yarn.scheduler.maximum-allocation-vcores 4
```

Agora temos um ambiente funcional para realizar os outros laboratórios do curso com o ecossistema Hadoop

Parabéns!
