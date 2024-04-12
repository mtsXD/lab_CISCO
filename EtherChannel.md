
<h1> Criação de Labs EtherChannel</h1>

<h2>Configuração simples dos Switches.</h2>

Crie 2 Switches e conecte-os em duas portas físicas cada de sua preferência, como mostra a imagem abaixo. Em cada Switch, você aplicará os comandos abaixo da imagem para configurá-lo, deixando-o seguro e de acordo com as boas práticas da CISCO.</p>

<p align="center">
  <img width="500" src="https://github.com/mtsXD/lab_CISCO/blob/main/IMGS_PT/Cria%C3%A7%C3%A3o_cada_Switch.png?raw=true">
</p>

No terminal de cada Swtich, execute a sequencia de comandos mostradas abaixo no modo de configuração Global:</p>
<h2>Config Switch 1</h2>

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
<h2>Config PAgP</h2>

<p>A tabela abaixo mostra a combinação de modos PAgP e se estabelece conexão ou não.</p>

### PAgP modes

|Switch_1|Switch_2|Estabelecimento de canal|
|:------:|:------:|:----------------------:|
|desirable|desirable|SIM|
|desirable|auto|SIM|
|auto|desirable|SIM|
|auto|auto|NÃO|

### Estabelecimento de canal PAgP em ambos desirable:

Dentro da configuração global em cada switch, estabeleça os comandos abaixo para tornalo PAgP desirable. Coloque os comandos com base nas interfaces de portas que você conectou os dois switches, no caso do nosso exemplo será as gigabitEthernet 0/1 e gigabitEthernet 0/2.</p>

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
A figura mostra o resultado dentro de cada Switch, você nota que há conexão, quando ao lado do numero no Port-channel (Po1) está como (SU) camada 2 em uso.</p>

<h2>Vizualização no Switch 1</h2>
<p align="center">
  <img width="500" src="https://github.com/mtsXD/lab_CISCO/blob/main/IMGS_PT/EtherChannel-sumary-PAgP_Switch_1.png?raw=true">
</p>
<h2>Vizualização no Switch 2</h2>
<p align="center">
  <img width="500" src="https://github.com/mtsXD/lab_CISCO/blob/main/IMGS_PT/EtherChannel-sumary-PAgP_Switch_2.png?raw=true">
</p>
<br>

### Estabelecimento de canal PAgP em dispositivo como desirable e outro como auto:

Dentro da configuração global em cada switch, estabeleça os comandos abaixo para tornalo PAgP com um canal desirable e outro auto. Coloque os comandos com base nas interfaces de portas que você conectou os dois switches, no caso do nosso exemplo será as gigabitEthernet 0/1 e gigabitEthernet 0/2.

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
