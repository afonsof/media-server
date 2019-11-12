### Introdução
Esse é um passo a passo para configurar um servidor multimídia. No final dessas etapas você terá o seguinte funcionando:
* Séries monitoradas e baixadas automaticamente com o Sonarr e Deluge
* Filmes monitorados e baixados automaticamente com o Radarr e Deluge
* Legendas monitoradas e baixadas automaticamente com o Bazarr
* Servidor de multimídia que pode ser acessado pelo aplicativo do Plex em smartphones e tvs, bem como pela versão web

### Material necessário
* Raspberry Pi 3B ou superior
* Raspbian 9 ou superior instalado com rede configurada
* HD USB com no mínimo 100GB para os filmes e configuração. (SSD recomendado)

### Passo 1
Fazer checkout desse repositório na pasta /media-server do seu raspberrypi
```
https://github.com/afonsof/media-server.git media-server
```

### Passo 2
Aumentar a memória de vídeo do dispositivo para 128MB
```shell script
sudo raspi-config
```
Ir na opção `Advanced Options > Memory Split` e setar para 128

### Passo 3
Aumentar a quantidade de memória Swap
```shell script
sudo dphys-swapfile swapoff
sudo nano /etc/dphys-swapfile
```
Dentro do arquivo procurar CONF_SWAPSIZE e trocar o valor para `CONF_SWAPSIZE=2048`
Salvar e sair
```shell script
sudo dphys-swapfile swapon
```

### Passo 4
Instalar as dependências genéricas
```shell script
sudo apt install -y \
     apt-transport-https \
     ca-certificates \
     curl \
     gnupg2 \
     software-properties-common \
     libffi-dev \
     libssl-dev \
     python \
     python-pip \
     python-backports.ssl-match-hostname
```

### Passo 5
Instalar o Docker
```shell script
curl -fsSL https://download.docker.com/linux/$(. /etc/os-release; echo "$ID")/gpg | sudo apt-key add -
echo "deb [arch=armhf] https://download.docker.com/linux/$(. /etc/os-release; echo "$ID") $(lsb_release -cs) stable" | \
sudo tee /etc/apt/sources.list.d/docker.list
sudo apt update
sudo apt install -y --no-install-recommends \
    docker-ce \
    cgroupfs-mount
sudo systemctl enable docker
sudo systemctl start docker
```

### Passo 6
Instalar o docker compose. Este passo pode demorar bastante
```shell script
sudo pip install docker-compose
```

### Passo 7
Configurar o HD Externo
Apontar o disco fat para o /media/usbdisk dando todas as permissões necessárias
```
sudo mkdir /media/usbdisk
echo "/dev/sda1    /media/usbdisk   vfat    rw,user,exec,umask=0000     0        0" >> /etc/fstab
```

### Passo 8
Subir docker-compose como serviço.
Copiar o script para a pasta do sistema
```
cp media-server.service /etc/systemd/system/media-server.service
sudo reboot
```
Após o reboot, cheque se está tudo certo
```
systemctl status media-server
```

### Considerações finais
Os servidores estarão disponíveis nos seguintes endereços:
* Sonarr: http://ip:8989
* Radarr: http://ip:7878
* Bazarr: http://ip:6767
* Deluge: http://ip:8112
* Plex: http://ip:32400
