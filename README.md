# OLT INTELBRAS G16

## Configurações iniciais

> criar vlans (vlan 20 de gerenciamento)

```sh
configure terminal
interface range ethernet 1/1
switchport mode trunk
exit
vlan 20-36
exit
```

> Interface de Uplink

```sh
configure terminal
interface ethernet 1/1
switchport trunk allowed vlan 20-36
```

> Ip na vlan de gerencia

```sh
configure terminal
interface vlan-interface 20
ip address 172.18.18.18 255.255.255.252
exit
```

> Rota default

```sh
configure terminal
ip route 0.0.0.0 0.0.0.0 172.18.18.17
```

> Habilitar SSH

```sh
configure terminal
ssh
end
crypto key generate rsa
```

> Ativar descoberta automáticas das ONT's

```sh
configure terminal
ont-find interface gpon all
```

> Salvar as configurações

```sh
end
copy running-config startup-config
y
```

> Apagar todas as configurações (padrão de fábrica)

```sh
end
clear startup-config
y
```

## SNMP

> Habilitando o SNMP Server

```sh
configure terminal
snmp-server enable
snmp-server community <comunidade> ro permit
```

## Referente as ONU's

> Buscar ONU's não provisionadas

```sh
configure terminal
# todas as pons
show ont-find list interface gpon all
# apenas em uma pon
show ont-find list interface gpon 0/1
# expecificando o index da ont
show ont-find list interface gpon 0/1 index 1
```

```sh
configure terminal
```

> Profile DBA

```sh
deploy profile dba
aim name DBA-DEFAULT
type 4 max 1200000
active
exit
exit
```

> Profile VLAN

```sh
deploy profile vlan
aim name VLAN-PON1
translate old-vlan 21 new-vlan 21
active
exit
exit
```

>Profile LINE (para cada modelo)

```sh
deploy profile line
aim name LINE-PON1-ONU-110Gb
device type i41-100
tcont 1 profile dba name DBA-DEFAULT
gemport 1 tcont 1 vlan-profile name VLAN-PON1
mapping mode port-vlan
mapping 1 port eth 1 vlan 21 gemport 1
flow 1 port eth 1 default vlan 21
active
exit
exit

deploy profile line
aim name LINE-PON1-ONU-110Gi
device type i30-100
tcont 1 profile dba name DBA-DEFAULT
gemport 1 tcont 1 vlan-profile name VLAN-PON1
mapping mode port-vlan
mapping 1 port eth 1 vlan 21 gemport 1
flow 1 port eth 1 default vlan 21
active
exit
exit

deploy profile line
aim name LINE-PON1-ONU-R1
device type i40-100
tcont 1 profile dba name DBA-DEFAULT
gemport 1 tcont 1 vlan-profile name VLAN-PON1
mapping mode port-vlan
mapping 1 port eth 1 vlan 21 gemport 1
flow 1 port eth 1 default vlan 21
active
exit
exit
```

> Profile RULE (add onu)

```sh
deploy profile rule
aim name ITBS-6eeae83c
permit sn string-hex ITBS-6eeae83c line name LINE-PON1-ONU-R1
active
exit
exit
```

> Profile RULE (delete onu)

```sh
deploy profile rule
delete aim name ITBS-6eeae83c
y
```
