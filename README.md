# COMO USAR A IMAGEM

## Docker Run
```
docker run -itd --name integracao-helios --env FTP_URL=ftp.endereco.com.br --env FTP_USER=usuario --env FTP_PASSWD=senha --env HELIOS_URI=https://helios.policiamilitar.mg.gov.br/v3/api/_track/register --env TOKEN_FORNECEDOR=token_fornecedor sistemapmmg/integracao-helios:latest
```

## Docker-Compose

### Edite o arquivo docker-compose.yml conforme conteúdo abaixo e altere as variáveis de ambiente para se conectar ao FTP e ao HELIOS_URI
 

```
version: '2'
services:
  integracao-helios:
    image: sistemapmmg/integracao-helios:latest
    container_name: php-integracao-helios
    environment:
      - FTP_URL=ftp-server
      - FTP_USER=usr_ftp
      - FTP_PASSWD=pass_ftp
      - HELIOS_URI=https://helios.policiamilitar.mg.gov.br/v3/api/_track/register
      - TOKEN_FORNECEDOR=token_fornecedor
    restart: always
    entrypoint: bash -c "php ./run.php"
  ftpd-server:
    image: fauria/vsftpd
    container_name: ftp-server
    environment:
      - FTP_USER=usr_ftp
      - FTP_PASS=pass_ftp
      - PASV_MIN_PORT=21100
      - PASV_MAX_PORT=21110
      - PASV_ENABLE=NO
    ports:
      - "21:21"
      - "21100-21110:21100-21110"
    volumes:
      - ./ftp-files:/home/vsftpd
    restart: always
```

# INSTALAÇÃO EM LINUX

## Executar os comandos abaixo

```
pkill docker
iptables -t nat -F
ifconfig docker0 down
brctl delbr docker0
docker -d