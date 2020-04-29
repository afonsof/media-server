# Media Server

### Introdução
Esse é um passo a passo para configurar um servidor multimídia. No final dessas etapas você terá o seguinte funcionando:
* Séries monitoradas e baixadas automaticamente com o Sonarr e Deluge
* Filmes monitorados e baixados automaticamente com o Radarr e Deluge
* Legendas monitoradas e baixadas automaticamente com o Bazarr
* Servidor de multimídia que pode ser acessado pelo aplicativo do Plex em smartphones e tvs, bem como pela versão web

### Material necessário
* Raspberry Pi 3B ou superior
* Raspbian 9 ou superior instalado com rede configurada ([Ajuda aqui](INSTALL_RASPBIAN))
* HD USB com no mínimo 100GB para os filmes e configuração. (SSD recomendado)

### Passo 1
Fazer esse passo apenas se o Raspberry tiver apenas 1GB de memória.
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

### Passo 2
Aumentar a memória de vídeo do dispositivo para 128MB
```shell script
sudo raspi-config
```
Ir na opção `Advanced Options > Memory Split` e setar para 128

### Passo 3
Instalar as dependências genéricas
```shell script
sudo apt update
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
     python-backports.ssl-match-hostname \
     git
```

### Passo 4
Instalar o Docker
```shell script
curl -fsSL https://download.docker.com/linux/$(. /etc/os-release; echo "$ID")/gpg | sudo apt-key add -
echo "deb [arch=armhf] https://download.docker.com/linux/$(. /etc/os-release; echo "$ID") $(lsb_release -cs) stable" | \
sudo tee /etc/apt/sources.list.d/docker.list
sudo apt update
sudo apt install -y --no-install-recommends \
    docker-ce \
    cgroupfs-mount
sudo groupadd docker
sudo usermod -aG docker $USER
sudo systemctl enable docker
sudo systemctl start docker
```

### Passo 5
Instalar o docker compose. Este passo pode demorar bastante
```shell script
sudo pip install docker-compose
```

### Passo 6
Configurar o HD Externo
Apontar o disco fat para o /media/usbdisk dando todas as permissões necessárias
```
sudo mkdir /media/usbdisk
echo "/dev/sda1    /media/usbdisk   vfat    auto,rw,user,sync,exec,dev,suid,uid=500,gid=500,umask=0000,nofail,iocharset=iso8859-1,codepage=850     0        0" >> /etc/fstab
```

### Passo 7
Entrar na console do dispositivo e fazer clone desse repositório na pasta `/media-server`
```shell script
git clone https://github.com/afonsof/media-server.git /media-server
cd /media-server
```

### Passo 8
Testar o docker compose
```shell script
docker-compose up
```

### Passo 9
Subir docker-compose como serviço.
Copiar o script para a pasta do sistema
```
sudo cp media-server.service /etc/systemd/system/media-server.service
sudo systemctl enable media-server.service
sudo systemctl start media-server.service
```

### Considerações finais
Os servidores estarão disponíveis nos seguintes endereços:
* Sonarr: http://ip:8989
* Radarr: http://ip:7878
* Bazarr: http://ip:6767
* Deluge: http://ip:8112
* Emby: http://ip:8096

### Configuração
* Deluge
    - mudar a pasta de downloads de `/root/Downloads` para `/downloads`
* Sonarr:
    - Download Client: Deluge
    - Indexer: Rarbg
    - Quality:   
