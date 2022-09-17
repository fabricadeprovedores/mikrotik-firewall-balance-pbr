# mikrotik-firewall-balance-pbr
Mikrotik Firewall Balance PBR

Script de Balanceamento de Carga entre 2 (dois) Links de Internet com base no IP de Origem. Essa técnica utiliza o conceito de Policy Based Routing (PBR) via marcação de conexões, marcação de rotas e utilização de lista de endereços.

Cenário
---

WAN Link 1 --> ether1 ->

                        --> Mikrotik --> ether10 --> LAN                      
                        
WAN Link 2 --> ether2 ->

---
            
1) Configurar Links de Internet (PPPoE ViVO)
- Vlan 10

> */interface ethernet switch vlan add independent-learning=no ports=ether1,ether2 switch=switch1 vlan-id=10*

- PPPoE

> */interface pppoe-client add add-default-route=yes default-route-distance=10 disabled=no interface=ether1 name=pppoe-out-link01 password=cliente use-peer-dns=no user=cliente@cliente*

> */interface pppoe-client add add-default-route=yes default-route-distance=20 disabled=no interface=ether2 name=pppoe-out-link02 password=cliente use-peer-dns=no user=cliente@cliente*

- Route

> */ip route add distance=10 gateway=pppoe-out-link01 routing-mark=via-link-01*

> */ip route add distance=20 gateway=pppoe-out-link02 routing-mark=via-link-02*

- NAT

> */ip firewall nat add action=masquerade chain=srcnat out-interface=pppoe-out-link01 routing-mark=via-link-01*

> */ip firewall nat add action=masquerade chain=srcnat out-interface=pppoe-out-link02 routing-mark=via-link-02*


2) Configurar Lista de Endereços

> */ip firewall address-list add address=100.64.0.1 list=link-01*

> */ip firewall address-list add address=100.64.0.2 list=link-02*

> */ip firewall address-list add address=100.64.0.3 list=link-01*

> */ip firewall address-list add address=100.64.0.4 list=link-02*

> */ip firewall address-list add address=100.64.0.5 list=link-01*

> */ip firewall address-list add address=100.64.0.6 list=link-02*

> */ip firewall address-list add address=100.64.0.7 list=link-01*

> */ip firewall address-list add address=100.64.0.8 list=link-02*



3) Configurar Regras de Firewall - Mangle


Wan Link 01

> */ip firewall mangle add action=mark-connection chain=prerouting new-connection-mark=link-01-con packet-mark=no-mark passthrough=yes src-address-list=link-01*

> */ip firewall mangle add action=mark-routing chain=prerouting connection-mark=link-01-con new-routing-mark=link-01 passthrough=no src-address-list=link-01*

Wan Link 02

> */ip firewall mangle add action=mark-connection chain=prerouting new-connection-mark=link-02-con packet-mark=no-mark passthrough=yes src-address-list=link-02*

> */ip firewall mangle add action=mark-routing chain=prerouting connection-mark=link-02-con new-routing-mark=link-02 passthrough=no src-address-list=link-02*

Obsevação:

- Não nos responsabilizamos pelo uso desse script e quaisquer "problemas" causados;
- Caso você não tenha experiência com Mikrotik, ou tenha dúvidas sobre sua implementação, considere contratar uma consultoria especializada;

Contatos:

E-mail..: admin@fabricadeprovedores.com.br

WebSite.: www.fabricadeprovedores.com.br
