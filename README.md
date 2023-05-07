# Projet-Infra

## Installation du serveur Minecraft

```` ssh root@serveripadress ```` 
Remplacer serveripadress par l'adresse IP de votre serveur.

### Installation de Java 

```` sudo yum install -y epel release````
```` sudo yum install -y java-latest-openjdk````

### Installation de l'écran

Install screen permet à votre serveur de fonctionner lorsque vous n'êtes pas connecté. 

````sudo yum install screen````

### Configuartion du serveur Minecraft

Créez un répertoire pour vos fichiers Minecraft.

````mkdir mineserver````

Vous vous déplacez dans ce répertoire. 

````cd mineserver````

Installez Wget afin de télécharger le serveur dans l'étape suivante.

````sudo yum install wget````

Téléchargez les fichiers de configuration nécessaires pour Minecraft, dirigez vous sur la page officielle de Minecraft : https://www.minecraft.net/en-us/download/server . Cliquez avec le bouton droit de la souris sur le lien .jar su serveur Minecraft et copiez l'adresse du lien. 

````wget https://piston-data.mojang.com/v1/objects/8f3112a1049751cc472ec13e397eade5336ca7ae/server.jar````

### Démarrez votre serveur ! 

Démarrez votre serveur pour la première fois.

````java -Xmx1024M -Xms1024M -jar server.jar nogui````

Avant que le serveur puisse démarrer, vous devrez accepter les termes de la licence de Mojang. Pour ce faire, vous devrez ouvrir le document dans l'éditeur de texte de votre choix. L'exemple ci-dessous utilisera Vim. La commande suivante ouvrira le document.

````vim eula.txt````

-Appuyez sur i pour passer en mode Insertion.
-Remplacez eula=false par eula=true.
-Appuyez sur Échap pour quitter le mode Insertion.
-Tapez :wq pour enregistrer vos modifications et quitter l'éditeur.

Une fois cela réalisé vous pouvez redémarrer votre serveur Minecraft.Tapez la commande que vous avez utilisée auparavant pour démarrer le serveur Minecraft.

````` java -Xmx1024M -Xms1024M -jar server.jar nogui ```` 

### Paramètres de votre pare-feu 

En plus de vous assurer que le port Minecraft (port 25565) est ouvert, vous devrez effectuer quelques ajustements supplémentaires pour que votre pare-feu accepte les connexions entrantes de Minecraft. 

`````firewall-cmd --permanent --add-port=25565/tcp`````

`````firewall-cmd --reload`````

Tout est bon désormais ! 

## Création du script 

Création du script afin de duppliquer les serveurs. 

`````cd`````

Création du fichier newserver.sh.

`````touch newserver.sh`````

Modifiez le script afin de duppliquer le serveur d'origine.

`````vim newserver.sh`````

-Appuyez sur i pour passer en mode Insertion.

`````
cp -R /root/mineserver "/root/$1"
sed -i "s/<MOTD_HERE>/$2/" "/root/$1/server.properties"
cp /root/server.service "/etc/systemd/system/0$.service"
sed -i "s/<25565>/$3/" "/root/$1/server.properties"
systemctl daemon-reload
`````

Vous pouvez désormais duppliquer votre serveur de base en modifiant le nom du fichier en argument 1, le nom du serveur in game en argument 2 et le port du serveur en argument 3 car deux serveurs ne peuvent être sur un même port. 

`````bash newserver.sh "nom du fichier" "nom du serveur" "port utulisé"````` 

Vous pouvez désormais duppliquer autant de serveurs que vous le souhaitez et y jouez ! 

![21gr1](https://user-images.githubusercontent.com/114937860/236700307-5bd80b77-2946-4a24-a43d-173968c5aa26.gif)
