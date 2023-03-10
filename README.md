# AirBee
<img src="Picture/montagev1.png" width="500"/>

L'idée est de transmettre toutes les données suivantes :
- température dans la ruche -- fait
- humidité dans la ruche -- fait
- masse de la ruche -- fait
- nombre d'abeilles entrées -- pas fait
- nombre d'abeille sortie -- pas fait
- tension de la batterie -- fait
Via un système 100% autonome en énergie et communication

Il y a eu 2 versions v1 et v2, je vous laisse les 2 pour voir ce qui n'a pas/mal fonctionné sur la premiere et les améliorations apportées.

Le système est maintenant abouti et fonctionnel.

	
## Coté alimentation : 
- Une lipo 5Ah <http://www.cdiscount.com/telephonie/batteries-telephone/easyacc-batterie-externe-5000mah-ultra-slim-charge/f-14487-eas0700686513077.html> (33€)
- Panneau solaire 3W <https://www.digikey.fr/product-detail/fr/seeed-technology-co-ltd/313070001/1597-1414-ND/5488054> (12USD)
- lipoRider <https://www.seeedstudio.com/Lipo-Rider-v1.3-p-2403.html> (9.5USD)

3 fils partent du module d'alim vers la carte (vcc,gnd,Vbat)

<img src="Picture/alim.jpg" width="500"/>
<img src="Picture/alim3D.jpg" width="500"/>

Cette partie n'a pas évoluée entre les 2 versions

## Coté intelligence :
- Carte ucontrolleur Arduino nano (2€) <https://fr.aliexpress.com/item/Freeshipping-Nano-3-0-controller-compatible-for-arduino-nano-CH340-USB-driver-NO-CABLE/32242060255.html?spm=a2g0s.9042311.0.0.Ohpjts>

## Coté communication :
- Kit Carte Breakout Sigfox BRKWS01 + Antenne +fdp TTC + un an abonnement <https://yadom.fr/carte-breakout-sfm10r1.html> (26€)
ou que le module <https://yadom.fr/reseaux-iot/sigfox/pack-10-modules-wisol-sfm10r1-pour-le-reseau-sigfox.html> (4€) mais il faut payer l'abonnement, sauf que l'on ne peut pas en prendre a l'unit", c'est par 10 pour plus de 220€ par an...

## Coté Capteur :
- pour avoir l'heure RTC  <https://www.adafruit.com/product/3296> (7.5USD) -- pas indispensable, supprimé sur la v2.
- Compteur I/O abeilles : projet BeeCounter2 (25USD) <http://www.instructables.com/id/Honey-Bee-Counter-II/> pas fait finalement.
- Capteur température humidité SHT11 (35USD) (2€ sur Aliexpress) <https://www.adafruit.com/product/246>. Une autre version avec DHT22 marche trés bien aussi.
- v1 : Capteur poids X4 + Load Cell Amplifier - HX711 (5€) <https://fr.aliexpress.com/item/FREE-SHIPPING-20pcs-lot-Experiments-body-scale-wholesale-human-scale-load-cell-sensor-resistance-strain-50kg/32271599811.html> et <https://fr.aliexpress.com/item/1PCS-HX711-Weighing-Sensor-Dual-Channel-24-Bit-Precision-A-D-Module-Pressure-Sensor-for-Arduino/32653087154.html?spm=a2g0s.9042311.0.0.zTDiLJ> 
Impossible d'avoir une valeur stable et fiable.... du coup mieux en v2

<img src="Picture/plateau.jpg" width="500"/>
  
  
  v2 : Un seul capteur 100kg + Load Cell Amplifier - HX711, bcp plus précis et sans dérive (17€) <https://fr.aliexpress.com/item/100kg-120kg-parallel-beam-type-load-cell-for-counting-scale-balance-TAL201/32616652058.html?spm=a2g0s.9042311.0.0.10bb6c37t0ZMNg>

<img src="Picture/capteur2.1.jpg" width="500"/>

<img src="Picture/capteur2.2.jpg" width="500"/>
  
## PCB

- v1
a la main !
<img src="Picture/electronique.jpg" width="500"/>

- v2

Fait sur Easy EDA <https://easyeda.com/rastafouille/airbeev2-2> fabriqué maison dans un premier temps mais commandé chez jlcpcb ou pcbway pour quelques €

<img src="Picture/PCB.jpg" width="500"/>


## Packaging

- Toutes les pièces en impression 3D pour la v1, les fichiers sont dans le dossier CAD. 
pour la v2 seul le module d'alimentation est en 3D print, pour le reste si dessous, bien mieux en finition et étanchéité.

- Connecteurs commandés pour la V2 (8€): <https://www.aliexpress.com/snapshot/0.html?spm=a2g0s.9042647.0.0.352f36faI0AsiM&orderId=506229421346969&productId=32761320961>

- Boites commandées pour la V2 (6€): <https://fr.aliexpress.com/item/1-Pc-tanche-Bo-tier-En-Plastique-Bo-te-lectronique-Projet-Instrument-Cas-lectrique-Bo-te/32862281438.html?spm=a2g0s.9042311.0.0.32036c37mz9SdR>

<img src="Picture/boite.jpg" width="500"/>

## Vue d'ensemble 

- V1 08/10/2017 

<img src="Picture/ensemble2.jpg" width="500"/>

- V2 

<img src="Picture/ensemblev2.jpg" width="500"/>


## Fonctionnement
L'Arduino centralise les données est les envoie au backend sigfox. On a droit à 140 messages de 12 Octets par jour, ca passe juste sans optimisation.

Payload : 
	
	ID::uint:8 Temp::int:16:little-endian Hum::uint:16:little-endian Poids::uint:16:little-endian IN::uint:16:little-endian OUT::uint:16:little-endian Vbat::uint:8

Sigfox renvoie les données via des CallBacks vers serveur perso (php-->json) et backend web (app.we-sens.com). je n'ai rien fait coté web j'utilise le site hebergeur d'un copain (app.we-sens.com) qui m'affiche les données.

<img src="Picture/courbes.png" width="500"/>

<img src="Picture/poids.png" width="500"/>


# Release
- v2 : suppression du RTC / changement capteur de poids et plateau / carte electronique dédiée faite maison

- V2.1: boite / connecteur / PCB pro / rafraichissement toutes les 15 min

- V2.2 04/2020 : soudage pieds au plateau / DHT22 car SHT11 plus dispo / module sigfox en 3.3v / passage de circuit maker à easyEDA


# Perspectives
- Rajouter le compteur d'abeilles

- Rajouter une station météo mais les données capteurs humudité+température+tension batterie (ensoleillement) peuvent suffires.

- Peut etre créer un reseau Xbee local sur 5 ruches pour un seul module Sigfox (si utile d'avoir plusieurs ruches connectées dans le même rucher?)

- optimiser l'energie pour passer en pile lithium 3.3V et se passer du panneau solaire.






