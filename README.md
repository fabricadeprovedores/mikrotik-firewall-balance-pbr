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
- PPPoE

2) Configurar Lista de Endereços

3) Configurar Regras de Firewall - Mangle

4) Configurar Regras de Firewall - Nat
