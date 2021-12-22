# deploy profile line

```sh
deploy profile line
aim name LINE-PON16-ONU-AN5506-02-B
device type i40-211
tcont 1 profile dba name SEM-LIMITE
gemport 1 tcont 1 vlan-profile name VLAN-PON16
mapping mode port-vlan
mapping 1 port eth 1 vlan 3066 gemport 1
flow 1 port eth 1 default vlan 3066
active
exit
exit
```

deploy profile line
aim 5 name VLAN-3054
device type i30-100
tcont 1 profile dba 1
gemport 1 tcont 1 vlan-profile 5
mapping mode port-vlan
mapping 1 port eth 1 vlan 3054 gemport 1
flow 1 port eth 1 default vlan 3054
active

```sh
deploy profile rule
aim name FHTT-942c41a0
permit sn string-hex FHTT-942c41a0 line name LINE-PON16-ONU-AN5506-02-B default line name LINE-PON16-ONU-AN5506-02-B
active
exit
exit
```

> Profile RULE (delete onu)

```sh
deploy profile rule
delete aim name FHTT-942c41a0
y
```
