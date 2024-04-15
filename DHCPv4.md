<h1> Criação de Lab DHCPv4</h1>

Antes de tudo, crie uma topologia simples, sem configurações no terminal, só posicionamento do que deseja trabalhar, no caso, esse nosso laboratório só precisará de 2 PCs, 2 Switches, 1 Roteador e 1 Servidor, com isso criou-se a topologia, conforme mostra a imagem abaixo.</p>

<p align="center">
  <img width="500" src="https://github.com/mtsXD/lab_CISCO/blob/main/IMGS_PT/Topologia_sem_configura%C3%A7ao.png?raw=true">
</p>

Observe que há algumas anotações, como o intervalo dos IPv4 nas áreas coloridas, o número IPv4 das portas Giga 0/0 e da Giga 0/1, esses valores ainda não foram atribuídos aos dispositivos, foi colocado apenas para facilitar na leitura da topologia.</p>
Outro ponto importante, os intervalos IPv4 são facultativos, fique a sua escolha para decidir, porém, pense nas boas práticas e intervalos que façam sentido no mundo real. Por último, particularmente gosto de trabalhar com os dois primeiros octetos como 192.168.0.0/16. Você pode trabalhar com o número de IP de sua preferência.</p>


<h2>Configuração simples dos Switches.</h2>

