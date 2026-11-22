# ProjetReseau 
DHCP sur routeur

Pitch : le routeur sert de passerelle et distribue les adresses IP.

1. Contexte : configurer chaque machine à la main ne tient pas à l’échelle (erreurs, doublons). Le DHCP automatise l’attribution.

2. Principe : échange en 4 temps, DORA :

Discover : le client cherche un serveur (broadcast)
Offer : le serveur propose une adresse
Request : le client accepte
Acknowledge : le serveur confirme le bail

3. Topologie : 1 routeur, 1 switch, <N> PC. Plan d’adressage : <réseau/masque>, passerelle <IP>.

4. Configuration

enable
configure terminal
interface <INTERFACE_LAN>
 ip address <IP_PASSERELLE> <MASQUE>
 no shutdown
 exit
ip dhcp excluded-address <IP_PASSERELLE>
ip dhcp pool <NOM_POOL>
 network <RESEAU> <MASQUE>
 default-router <IP_PASSERELLE>
 dns-server 8.8.8.8
end
write memory

5. Pourquoi chaque commande

Commande	Rôle
ip dhcp excluded-address	Empêche le routeur de distribuer sa propre IP (évite un conflit)
network	Définit la plage du pool
default-router	Passerelle donnée aux clients
dns-server 8.8.8.8	DNS donné aux clients

6. Vérification et resultat

<img width="920" height="802" alt="image" src="https://github.com/user-attachments/assets/9392cfea-b003-4fef-8dd0-b8567af76862" />

PC : IP Configuration > DHCP, puis ipconfig pour contrôler l’adresse, la passerelle et le DNS.
Routeur : show ip dhcp binding, show ip dhcp pool.
ping vers la passerelle puis vers un autre PC.

7. Erreurs fréquentes : interface restée en shutdown ; passerelle non exclue ; mauvais masque dans le pool ; PC en IP statique au lieu de DHCP.

8. Compétences : IOS, DHCP, adressage IPv4, tests de connectivité.
