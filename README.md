#### Otimizacão Raspberry Pi OS 

>[!NOTE]
>Hardware - Raspberry Zero 2 W

#### Otimização Swap

```
sudo nano /etc/sysctl.conf
```

Adicionar no final do arquivo:  ```vm.swappiness=1```

```
sudo sysctl -p
```

#### Desativar IPV6
>[!NOTE]
>Opcional

```
sudo nano /etc/sysctl.conf
```

Adicionar no final do arquivo:
```
net.ipv6.conf.all.disable_ipv6 = 1
net.ipv6.conf.default.disable_ipv6 = 1
net.ipv6.conf.lo.disable_ipv6 = 1
```

```
sudo sysctl -p
```

#### Configurar Journal para Memória

```
sudo nano /etc/systemd/journald.conf
```
```
[Journal]
Storage=volatile
RateLimitIntervalSec=10s
RateLimitBurst=200
RuntimeMaxUse=20M
```

#### Configurar Commit do sistema de arquivos e enviar /tmp e /var/log para memória

```
sudo /etc/fstab
```
##### Configurar o commit

>[!IMPORTANT]
> Em caso de desligamento do sistema os dados de cache que ainda não foram gravados no disco serão perdidos.

Adicionar o parâmetro ```commit="seconds"``` na linha EXT4:

```PARTUUID=aaaaaaa  /               ext4    defaults,noatime,commit=3600  0       1```

##### Configurar /tmp e /var/log para memória

>[!IMPORTANT]
> Em caso de desligamento do sistema os arquivos temporários e logs serão perdidos, pois estão em modo RAM-ONLY.

>[!TIP]
> Para definir o tamanho ideal (parâmetro ```size```) do arquivo /tmp e /var/log consulte o tamando em disco antes de alterar o ```/etc/fstab```
>
>```du -h /tmp```
>
>```du -h /var/log```

Adicionar no final do arquivo:

```
tmpfs /tmp tmpfs defaults,noatime,size=5M 0 0
tmpfs /var/log tmpfs defaults,noatime,size=16M 0 0
```
Reniciar o sistema.

>[!TIP]
> Para consultar se as alterações tiveram efeito: ```df -h```






