## Configuration de la machine client Windows 11 (CLIWIN01)

Renommez la machine :

hostname

Si le nom n’est pas correct :

Rename-Computer -NewName CLIWIN01 -Restart

Capture d’écran : résultat de la commande hostname.

Vérifiez que l’utilisateur Wilder existe :

Get-LocalUser -Name "Wilder"

Si l’utilisateur n’existe pas :

net user Wilder Azerty1* /add

Capture d’écran : résultat de Get-LocalUser.

Ajoutez l’utilisateur au groupe Administrateurs :

net localgroup Administrators Wilder /add

Vérifiez :

net localgroup Administrators

Capture d’écran : Wilder présent dans le groupe Administrators.

Configurez l’adresse IP manuellement :

IP : 172.16.20.20
Masque : 255.255.255.0
Passerelle : 172.16.20.254
DNS : 8.8.8.8

Capture d’écran : configuration IPv4.

Vérifiez la configuration réseau :

ipconfig

Capture d’écran : résultat de ipconfig.

## Configuration de la machine serveur Debian 13 (SRVLX01)

Vérifiez le nom de la machine :

hostname

Capture d’écran : résultat de hostname.

Vérifiez la version du système :

cat /etc/os-release

Capture d’écran : résultat de la commande.

Vérifiez l’utilisateur wilder :

id wilder

Si l’utilisateur n’existe pas :

adduser wilder

Mot de passe : Azerty1*

Ajoutez l’utilisateur au groupe sudo :

usermod -aG sudo wilder

Vérifiez :

groups wilder

Capture d’écran : présence de sudo.

Activez l’interface réseau :

ip link set ens18 up

Configurez l’adresse IP :

ip addr flush dev ens18
ip addr add 172.16.20.10/24 dev ens18

Configurez la passerelle :

ip route add default via 172.16.20.254

Configurez le DNS :

echo "nameserver 8.8.8.8" > /etc/resolv.conf

Vérifiez la configuration :

ip a
ip route

Capture d’écran : résultat de ip a et ip route.

## Test réseau

Depuis Windows 11 :

ping 172.16.20.10

Depuis Debian :

ping 172.16.20.20

Capture d’écran : ping réussi entre les deux machines.

## Installation et configuration SSH sur Debian

Vérifiez le service SSH :

systemctl status ssh

Si le service n’est pas installé :

apt update
apt install openssh-server -y
systemctl enable ssh
systemctl start ssh

Vérifiez :

systemctl status ssh

Le service doit être actif.

Capture d’écran : status SSH actif.

## Test de connexion SSH

Depuis Windows 11 :

ssh wilder@172.16.20.10

Mot de passe : Azerty1*

Capture d’écran : connexion SSH réussie.
