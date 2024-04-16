<h1> Criação de Lab DHCPv4</h1>

Antes de tudo, crie uma topologia simples, sem configurações no terminal, só posicionamento do que deseja trabalhar, no caso, esse nosso laboratório só precisará de 2 PCs, 2 Switches, 1 Roteador e 1 Servidor, com isso criou-se a topologia, conforme mostra a imagem abaixo.</p>

<p align="center">
  <img width="500" src="https://github.com/mtsXD/lab_CISCO/blob/main/IMGS_PT/Topologia_sem_configura%C3%A7ao.png?raw=true">
</p>

Observe que há algumas anotações, como o intervalo dos IPv4 nas áreas coloridas, o número IPv4 das portas Giga 0/0 e da Giga 0/1, esses valores ainda não foram atribuídos aos dispositivos, foi colocado apenas para facilitar na leitura da topologia.</p>
Outro ponto importante, os intervalos IPv4 são facultativos, fique a sua escolha para decidir, porém, pense nas boas práticas e intervalos que façam sentido no mundo real. Por último, particularmente gosto de trabalhar com os dois primeiros octetos como 192.168.0.0/16. Você pode trabalhar com o número de IP de sua preferência.</p>


<h2>Conciderações.</h2>

A primeira coisa a se fazer é separar alguns emails como boa prática, neste laboratório, será separado 10 em cada intervalo, no mundo real, há situações que você terá mais recursos para selecionar com melhor precisão os intervalos, mas no caso desse laboratório, 10 já é mais que o suficiente.</p>
O ato de separar, é por conta das configurações DHCPv4 no roteador não apresentarem problemas no momento da execução dos comandos. Nesse caso, separaremos os 10 primeiros de cada intervalo, o primeiro número de IPv4 separado de cada intervalo, será atribuído como gateway padrão das portas Giga 0/0 e Giga 0/1. Os outros serão guardados para futuras aplicações.</p>

>
>A tabela abaixo apresenta como quero que as configurações sejam aplicadas no switchs:

||IPv4|porta|maneira|
|:----:|:----:|:---:|:----:|
|PC_1|192.168.10.11||Automatica|
|PC_2|192.168.11.11||Automatica|
|Server_1|192.168.11.5||Manual|
|R_1|192.168.10.1|Giga 0/0|Manual|
|R_1|192.168.11.1|Giga 0/1|Manual|
|S_1|192.168.10.2|Giga 0/1|Manual|
|S_2|192.168.11.2|Giga 0/1|Manual|

A ideia desse laboratório é que o Roteador R_1 aplique os IPv4 de maneira automática no PC_1 e PC_2, nesse cenário, pode não fazer tanto sentido essa função, mas imagina uma escala de dezenas ou até mesmo de centenas de dispositivos, é muito mais eficiente trabalhar de maneira automática.</p>

<h2>Configuração dos Switchs.</h2>
<h3>Configuração S_1</h3>

>
>Dentro do CLI do Switch, execute os seguintes comandos:

```
enable
configure terminal
vlan 199
name ADN
exit
vlan 666
name LIXO
exit
vlan 999
name Nativa
exit
interface vlan 199
ip address 192.168.10.2 255.255.255.0
no shutdown
interface gigabitEthernet 0/1
switchport mode access
switchport access vlan 199
interface gigabitEthernet 0/2
switchport mode access
switchport access vlan 666
shutdown
interface range fastEthernet 0/2-24
switchport mode access
switchport access vlan 666
shutdown
hostname S_1
enable secret 12345
service password-encryption 
banner motd #Acesso somente para pessoas autorizadas.#
line console 0
password 54321
login
logging synchronous
exec-timeout 5 00
exit
line vty 0 15
password 54321
login
logging synchronous
exec-timeout 10 00
exit
ip default-gateway 192.168.10.1
end
write
```

<h3>Configuração S_2</h3>

>
>Dentro do CLI do Switch, execute os seguintes comandos:

```
enable
configure terminal
vlan 199
name ADN
exit
vlan 666
name LIXO
exit
vlan 999
name Nativa
exit
interface vlan 199
ip address 192.168.11.2 255.255.255.0
no shutdown
interface gigabitEthernet 0/1
switchport mode access
switchport access vlan 199
interface gigabitEthernet 0/2
switchport mode access
switchport access vlan 666
shutdown
interface range fastEthernet 0/3-24
switchport mode access
switchport access vlan 666
shutdown
hostname S_2
enable secret 12345
service password-encryption 
banner motd #Acesso somente para pessoas autorizadas.#
line console 0
password 54321
login
logging synchronous
exec-timeout 5 00
exit
line vty 0 15
password 54321
login
logging synchronous
exec-timeout 10 00
exit
ip default-gateway 192.168.11.1
end
write
```
