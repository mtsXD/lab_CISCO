
<h1> Criação de Labs EtherChannel</h1>

<h2>Configuração simples dos Switches.</h2>

Crie 2 Switches e conecte-os em duas portas físicas cada de sua preferência, como mostra a imagem abaixo. Em cada Switch, você aplicará os comandos abaixo da imagem para configurá-lo, deixando-o seguro e de acordo com as boas práticas da CISCO.</p>

<p align="center">
  <img width="500" src="https://github.com/mtsXD/lab_CISCO/blob/main/IMGS_PT/Cria%C3%A7%C3%A3o_cada_Switch.png?raw=true">
</p>

No terminal de cada Swtich, execute a sequencia de comandos mostradas abaixo no modo de configuração Global:</p>
<h2>Config Switch 1</h2>
<br>

>
>Uma observação, as senhas dos comandos "enable secret" e "password" são de sua preferência, o que coloquei é apenas um exemplo e não é uma boa prática, aconselho que coloque de forma que contenha números, letras e símbolos e que ultrapasse 8 caracteres.
<br>

```
enable
configure terminal
hostname Switch_1
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
end
write
```
<h2>Config Switch 2</h2>

```
enable
configure terminal
hostname Switch_2
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
end
write
```
<h2>Configuração PAgP</h2>

<p>A tabela abaixo mostra a combinação de modos PAgP e se estabelece conexão ou não.</p>

### Modo PAgP

|Switch_1|Switch_2|Estabelecimento de canal|
|:------:|:------:|:----------------------:|
|desirable|desirable|SIM|
|desirable|auto|SIM|
|auto|desirable|SIM|
|auto|auto|NÃO|

<h3>Estabelecimento de canal PAgP em ambos desirable:</h3>

Dentro da configuração global em cada switch, estabeleça os comandos abaixo para torna-lo PAgP desirable. Coloque os comandos com base nas interfaces de portas que você conectou os dois switches, no caso do nosso exemplo será as gigabitEthernet 0/1 e gigabitEthernet 0/2.</p>

>
>modo PAgP desirable coloca uma interface em um estado de negociação ativo, no qual a interface inicia negociações com outras interfaces enviando pacotes PAgP.

### Forneça esses comandos nos dois Switches.
```
interface range gigabitEthernet 0/1-2
channel-group 1 mode desirable
exit
interface port-channel 1
switchport mode trunk 
end
write
```

Para verificar se ambos os Switches estão com o canal PAgP estabelecido negociação ativa, usa-se dentro da interface do usuário o comando mostrado abaixo:</p>

```
show etherchannel summary
```
A figura mostra o resultado dentro de cada Switch, você nota que há conexão, quando ao lado do numero no Port-channel (Po1) está como camada 2 em uso (SU).</p>

<h2>Vizualização no Switch 1</h2>
<p align="center">
  <img width="500" src="https://github.com/mtsXD/lab_CISCO/blob/main/IMGS_PT/EtherChannel-sumary-PAgP_Switch_1.png?raw=true">
</p>
<h2>Vizualização no Switch 2</h2>
<p align="center">
  <img width="500" src="https://github.com/mtsXD/lab_CISCO/blob/main/IMGS_PT/EtherChannel-sumary-PAgP_Switch_2.png?raw=true">
</p>
<br>

<h3>Estabelecimento de canal PAgP em dispositivo como desirable e outro como auto:</h3>

Dentro da configuração global em cada switch, estabeleça os comandos abaixo para torna-lo PAgP com um canal desirable e outro auto. Coloque os comandos com base nas interfaces de portas que você conectou os dois switches, no caso do nosso exemplo será as gigabitEthernet 0/1 e gigabitEthernet 0/2.

>
> O modo auto coloca a interface como negociação passiva, ou seja, não há comunicação, a interface só responde os pacotes PAgP que recebe.

No caso desse exemplo, o Switch_1 será estabelecido como desirable e o Switch_2 como auto, mas a decisão partirá de você, ou no que você julga ser necessário para ser desirable ou não.</p>

### Forneça esses comandos no Switch_1.
```
interface range gigabitEthernet 0/1-2
channel-group 1 mode desirable
exit
interface port-channel 1
switchport mode trunk 
end
write
```
### Forneça esses comandos no Switch_2.
```
interface range gigabitEthernet 0/1-2
channel-group 1 mode auto
exit
interface port-channel 1
switchport mode trunk 
end
write
```
Para verificar se os Switches estão com o canal PAgP estabelecido como um em negociação ativa e outro como passiva, usa-se dentro da interface do usuário o comando mostrado abaixo:</p>

```
show etherchannel summary
```

A figura mostra o resultado dentro de cada Switch, você nota que há conexão, quando ao lado do numero no Port-channel (Po1) está como camada 2 em uso (SU).</p>

<h2>Vizualização no Switch 1</h2>
<p align="center">
  <img width="500" src="https://github.com/mtsXD/lab_CISCO/blob/main/IMGS_PT/EtherChannel-sumary-PAgP_Switch_1-DA.png?raw=true">
</p>
<h2>Vizualização no Switch 2</h2>
<p align="center">
  <img width="500" src="https://github.com/mtsXD/lab_CISCO/blob/main/IMGS_PT/EtherChannel-sumary-PAgP_Switch_2-DA.png?raw=true">
</p>
<br>

<h3>Estabelecimento de canal PAgP em ambos auto:</h3>

Dentro da configuração global em cada switch, estabeleça os comandos abaixo para torna-lo PAgP auto. Coloque os comandos com base nas interfaces de portas que você conectou os dois switches, no caso do nosso exemplo será as gigabitEthernet 0/1 e gigabitEthernet 0/2.</p>

### Forneça esses comandos nos dois Switches.
```
interface range gigabitEthernet 0/1-2
channel-group 1 mode auto
exit
interface port-channel 1
switchport mode trunk 
end
write
```
Para verificar se ambos os Switches estão com o canal PAgP estabelecido negociação passiva, usa-se dentro da interface do usuário o comando mostrado abaixo:</p>

```
show etherchannel summary
```
A figura mostra o resultado dentro de cada Switch, você nota que NÃO há conexão, quando ao lado do numero no Port-channel (Po1) está como camada 2 em Down (SD).</p>

<h2>Vizualização no Switch 1</h2>
<p align="center">
  <img width="500" src="https://github.com/mtsXD/lab_CISCO/blob/main/IMGS_PT/EtherChannel-sumary-PAgP_Switch_1-AA.png?raw=true">
</p>
<h2>Vizualização no Switch 2</h2>
<p align="center">
  <img width="500" src="https://github.com/mtsXD/lab_CISCO/blob/main/IMGS_PT/EtherChannel-sumary-PAgP_Switch_2-AA.png?raw=true">
</p>
<br>

<h2>Conclusão do PAgP</h2>

Como mostrado nas duas primeiras configurações, o estabelecimento de canal só é bem sucedido quando o modo é desirable/desirable ou desirable/auto; auto/desirable. Entretanto, caso o modo seja auto/auto, não haverá sucesso em estabelecer o canal.</p>
<br>

<h2>Configuração LACP</h2>

<p>A tabela abaixo mostra a combinação de modos LACP e se estabelece conexão ou não.</p>

### Modo LACP

|Switch_1|Switch_2|Estabelecimento de canal|
|:------:|:------:|:----------------------:|
|active|active|SIM|
|active|passive|SIM|
|passive|active|SIM|
|passive|passive|NÃO|

<h3>Estabelecimento de canal LACP em ambos active:</h3>

Dentro da configuração global em cada switch, estabeleça os comandos abaixo para torna-lo LACP active. Coloque os comandos com base nas interfaces de portas que você conectou os dois switches, no caso do nosso exemplo será as gigabitEthernet 0/1 e gigabitEthernet 0/2.</p>

>
>modo LACP active deixa a interface em negociação ativa, ou seja, inicia a negociação enviando pacotes LACP.

```
interface range gigabitEthernet 0/1-2
channel-group 1 mode active
exit
interface port-channel 1
switchport mode trunk 
end
write
```

Para verificar se ambos os Switches estão com o canal LACP estabelecido negociação ativa, usa-se dentro da interface do usuário o comando mostrado abaixo:</p>

```
show etherchannel summary
```
A figura abaixo mostra o resultado dentro de cada Switch, você nota que há conexão, quando ao lado do numero no Port-channel (Po1) está como camada 2 em uso (SU).</p>

<h2>Vizualização no Switch 1</h2>
<p align="center">
  <img width="500" src="https://github.com/mtsXD/lab_CISCO/blob/main/IMGS_PT/EtherChannel-sumary-LACP_Switch_1-AA.png?raw=true">
</p>
<h2>Vizualização no Switch 2</h2>
<p align="center">
  <img width="500" src="https://github.com/mtsXD/lab_CISCO/blob/main/IMGS_PT/EtherChannel-sumary-LACP_Switch_2-AA.png?raw=true">
</p>
<br>

<h3>Estabelecimento de canal LACP em dispositivo como active e outro como passive:</h3>

Dentro da configuração global em cada switch, estabeleça os comandos abaixo para torna-lo LACP com um canal em modo active e outro em modo passive. Coloque os comandos com base nas interfaces de portas que você conectou os dois switches, no caso do nosso exemplo será as gigabitEthernet 0/1 e gigabitEthernet 0/2.

>
> O modo passive deixa a interface em modo passiva, ou seja, respone os pacotes LACP enviados, mas não inicia negociação.

No caso desse exemplo, o Switch_1 será estabelecido como active e o Switch_2 como passive, mas a decisão partirá de você, ou no que você julga ser necessário para ser active ou não.</p>

### Forneça esses comandos no Switch_1.
```
interface range gigabitEthernet 0/1-2
channel-group 1 mode active
exit
interface port-channel 1
switchport mode trunk 
end
write
```
### Forneça esses comandos no Switch_2.
```
interface range gigabitEthernet 0/1-2
channel-group 1 mode passive
exit
interface port-channel 1
switchport mode trunk 
end
write
```
Para verificar se os Switches estão com o canal LACP estabelecido como um em negociação ativa e outro como passiva, usa-se dentro da interface do usuário o comando mostrado abaixo:</p>

```
show etherchannel summary
```

A figura mostra o resultado dentro de cada Switch, você nota que há conexão, quando ao lado do numero no Port-channel (Po1) está como camada 2 em uso (SU).</p>

<h2>Vizualização no Switch 1</h2>
<p align="center">
  <img width="500" src="https://github.com/mtsXD/lab_CISCO/blob/main/IMGS_PT/EtherChannel-sumary-LACP_Switch_1-AP.png?raw=true">
</p>
<h2>Vizualização no Switch 2</h2>
<p align="center">
  <img width="500" src="https://github.com/mtsXD/lab_CISCO/blob/main/IMGS_PT/EtherChannel-sumary-LACP_Switch_2-AP.png?raw=true">
</p>
<br>

<h3>Estabelecimento de canal LACP em ambos passive:</h3>

Dentro da configuração global em cada switch, estabeleça os comandos abaixo para torna-lo LACP passive. Coloque os comandos com base nas interfaces de portas que você conectou os dois switches, no caso do nosso exemplo será as gigabitEthernet 0/1 e gigabitEthernet 0/2.</p>

### Forneça esses comandos nos dois Switches.
```
interface range gigabitEthernet 0/1-2
channel-group 1 mode passive
exit
interface port-channel 1
switchport mode trunk 
end
write
```
Para verificar se ambos os Switches estão com o canal LACP estabelecido negociação passiva, usa-se dentro da interface do usuário o comando mostrado abaixo:</p>

```
show etherchannel summary
```
A figura mostra o resultado dentro de cada Switch, você nota que NÃO há conexão, quando ao lado do numero no Port-channel (Po1) está como camada 2 em Down (SD).</p>

<h2>Vizualização no Switch 1</h2>
<p align="center">
  <img width="500" src="https://github.com/mtsXD/lab_CISCO/blob/main/IMGS_PT/EtherChannel-sumary-LACP_Switch_1-PP.png?raw=true">
</p>
<h2>Vizualização no Switch 2</h2>
<p align="center">
  <img width="500" src="https://github.com/mtsXD/lab_CISCO/blob/main/IMGS_PT/EtherChannel-sumary-LACP_Switch_2-PP.png?raw=true">
</p>
<br>

<h2>Conclusão do LACP</h2>

Como mostrado nas duas primeiras configurações, o estabelecimento de canal só é bem sucedido quando o modo é active/active ou active/passive; passive/active. Entretanto, caso o modo seja passive/passive, não haverá sucesso em estabelecer o canal.</p>
<br>

<p align="center"> Obrigado~ </p>
<p align="center">
  <img  src="https://media.tenor.com/EpgQmvLA3rUAAAAC/misato-katsuragi-neon-genesis-evangelion.gif">
</p>
