
### EtherChannel + Criação de Labs
<br>

>
> Antes de tudo, dentro do terminal do Switch, você deve ir na configuração global e lá especificar as interfaces de porta que irão compor a Etherchannel usando o seguinte comando:
```
interface range [conexão] [númemo da interface] / [intervalo da interface que você deseja trabalhar]
```
>
>[conexão], como o nome já diz, é o tipo de conexão que você deseja trabalhar, pode escolher dentre 4, como mostra a tabela abaixo.</p>
>[número de interface], é o número que o cabo que você deseja trabalhar está conectado.</p>
>[intervalo da interface que você deseja trabalhar], é a continuação da representação do número de interface, pois existe várias interfaces 0, logo para expecificar, é necessário colocar quais deseja trabalhar.<\p>


Observe esse exemplo dos 4 tipos de conexões com o comando interface range:
```
interface range fastEthernet 0/23-24
interface range ethernet 9/2-3
interface range gigabitEthernet 0/10-19
interface range vlan 99-357
```

>
>Dentro da interface de porta, você agora criará um channel-group e decidirá a qual grupo as interfaces de porta devem pertencer e você deverá selecionar o tipo de protocolo de negociação que ela deve se comunicar com o Switch vizinho, como mostra o comando a seguir:</p>
```
channel-group [n] mode [protocolo]
```
>[n], é o número do grupo que você quer criar, podendo ser entre 1 a 6.</p>
>[protocolo], é o tipo de nogociação entre swithcs que você deseja estabelecer, podendo ser EtherChannel, LACP ou PAgP.</p>
