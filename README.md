# Zorin OS i configuració de xarxa

Aquest recull presenta les captures del document de referència: inici de Zorin OS, configuració de xarxa, proves al terminal, Wireshark i servei DHCP. Les explicacions descriuen el que es veu a cada foto.

## Captures inicials de Zorin OS

![foto 1](img/foto1.png)

 Fragment visual de la finestra de recursos durant l’inici de Zorin.

![foto 2](img/foto2.png)

 Pantalla d’arrencada de Zorin OS 18.

![foto 3](img/foto3.png)

 Escriptori de Zorin OS un cop s’ha iniciat el sistema.

## Configuració de xarxa

![foto 4](img/foto4.png)

 Opcions de l’adaptador de xarxa de la màquina virtual, configurat en mode pont.

![foto 5](img/foto5.png)

 Configuració IPv4 manual amb adreça, passarel·la i DNS.

## Comprovacions de xarxa amb terminal

![foto 6](img/foto6.png)

 Informació de les interfícies de xarxa mostrada al terminal.

![foto 7](img/foto7.png)

 Vista de la configuració de l’adaptador virtual.

![foto 8](img/foto8.png)

 Resultat de consultes de xarxa i adreces del sistema.

## Entorn del sistema

![foto 9](img/foto9.png)

 Resum de les dades del sistema operatiu i dels recursos.

![foto 10](img/foto10.png)

 Ordres i sortides de terminal relacionades amb la configuració.

## Wireshark

![foto 11](img/foto11.png)

 Instal·lació de Wireshark i dels paquets que necessita.

![foto 12](img/foto12.png)

 Finestra de Wireshark abans d’iniciar una captura.

## Anàlisi del trànsit

![foto 13](img/foto13.png)

 Captura de paquets a Wireshark.

![foto 14](img/foto14.png)

 Detall dels camps i protocols d’un paquet capturat.

## Estat de la xarxa

![foto 15](img/foto15.png)

 Informació de les interfícies i del sistema consultada al terminal.

![foto 16](img/foto16.png)

 Adreces i estat de la interfície de xarxa.

## Comprovació de DHCP

![foto 17](img/foto17.png)

 La consulta indica que el fitxer de concessions no existeix a la ruta provada.

![foto 18](img/foto18.png)

 Fragment de configuració DHCP en format JSON.

![foto 19](img/foto19.png)

 Registres d’esdeveniments i canvis relacionats amb DHCP.

## Servei DHCP

![foto 20](img/foto20.png)

 Registres d’activitat del servei DHCP.

![foto 21](img/foto21.png)
 Detall de les concessions DHCP i dels clients.

## Conclusió
En aquesta pràctica he configurat la xarxa de Zorin i he comprovat el seu funcionament amb el terminal de Zorin i Wireshark. També he tingut que cambiar el servei DHCP i els seus registres. Això m’ha ajudat a verue com funciona i com es configura les ips i tota la xarxa de un sistema, ya sigui linux com windows