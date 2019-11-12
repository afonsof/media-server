# Instalar o Raspbian

### Passo 1
Baixar a imagem do Raspbian no site: https://www.raspberrypi.org/downloads/raspbian/
Eu opto sempre pela Lite já que não precisamos da interface gráfica aqui

### Passo 2
Inserir o cartão no computador e usar o Balena Etcher para gravar a imagem recém baixada

### Passo 3
Inserir o cartão no computador e criar um arquivo `ssh` vazio dentro da partição `boot`

### Passo 4
Inserir o cartão de memória no RaspberryPi

## Caso for usar rede wifi
* Conectar um teclado e monitor no dispositivo
* Ligar na força
* Assim que for solicitado, digitar a senha `raspberry`
* Digitar o comando `sudo raspi-config`
* Seu raspberry está pronto para receber o servidor de multimídia

## Caso for usar rede cabeada
* Ligar o cabo de rede.
* Ligar na força
* Encontrar o IP do seu dispositivo na lista do seu roteador ou ligar um monitor e esperar aparecer na tela
* Acessar o dispositivo através de um computador com o comando `ssh pi@[ip-do-seu-dispositivo]`
* Assim que for solicitado digitar a senha `raspberry`
* Seu raspberry está pronto para receber o servidor de multimídia