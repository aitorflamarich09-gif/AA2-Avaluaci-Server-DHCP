# Zorin OS: configuració de xarxa, Wireshark i servei DHCP

## 1. Introducció

Aquesta pràctica té com a objectiu configurar i verificar la xarxa d'una màquina amb Zorin OS dins d'un entorn virtualitzat. Durant l'activitat es treballa amb la configuració de les interfícies de xarxa, les adreces IP, Wireshark i el servei DHCP Kea.

La pràctica permet comprovar el funcionament de la xarxa tant des del terminal com mitjançant l'anàlisi del trànsit de xarxa.




## 2. Inici de Zorin OS

### 2.1. Arrencada del sistema

![Captura 1](img/foto1.png)

Aqui es pot veure com s'he esta instal·lan la maquina Zorin.

![Captura 2](img/foto2.png)

Pantalla d'arrencada de Zorin.

![Captura 3](img/foto3.png)

Escriptori de Zorin un cop finalitzada l'arrencada.

---

## 3. Configuració de la xarxa

### 3.1. Configuració de l'adaptador de VirtualBox

![Captura 4](img/foto4.png)

La màquina virtual disposa d'un adaptador de xarxa interna per poder comincar les dues màquines virtual entre elles.

![Captura 5](img/foto5.png)

Configuració de la interfície de xarxa amb IPv4. En les diferents fases de la pràctica es treballa amb configuracions d'adreçament diferents, que es poden observar a les captures.

![Captura 7](img/foto7.png)

Vista de l'adaptador de només l'anfitrio per poder fer ssh amb la terminal.

---

## 4. Comprovació de la configuració de xarxa

### 4.1. Instal·lació de Kea DHCP

```bash
sudo apt install kea -y
```

![Captura 6](img/foto6.png)

La captura mostra el procés d'instal·lació dels paquets de Kea i les seves dependències.

### 4.2. Consultar les interfícies i adreces IP


![Captura 9](img/foto9.png)

La comanda mostra informació general del sistema i l'adreça IP assignada a la interfície de xarxa.

També es pot consultar l'estat de les interfícies amb:

```bash
ip link
```

![Captura 16](img/foto16.png)

Aquesta ordre permet comprovar si les interfícies estan actives i consultar les seves adreces MAC.

### 4.3. Consultar la configuració de NetworkManager

```bash
nmcli device show
```

![Captura 15](img/foto15.png)

Aquesta comanda mostra informació detallada de les connexions, incloent-hi adreces IPv4, passarel·les, rutes i servidors DNS.

---

## 5. Configuració de la interfície de xarxa

![Captura 8](img/foto8.png)

```bash
sudo nano /etc/netplan/50-cloud-init.yaml
```

Fragment de configuració de xarxa en format YAML. La configuració mostrada inclou una interfície amb DHCP i una interfície amb adreça IP estàtica.

```yaml
version: 2

ethernets:
  enp0s3:
    dhcp4: true
  enp0s6:
    dhcp4: false
    addresses:
      - 192.168.50.10/24
```




## 6. Instal·lació i configuració de Wireshark

### 6.1. Instal·lació

```bash
sudo apt install wireshark
```

![Captura 11](img/foto11.png)

La captura mostra la instal·lació de Wireshark i dels paquets necessaris.

### 6.2. Inici de Wireshark

```bash
sudo wireshark
```

![Captura 14](img/foto14.png)

Un cop iniciat el programa, es pot seleccionar la interfície sobre la qual es vol realitzar la captura.

![Captura 12](img/foto12.png)

Finestra principal de Wireshark abans d'iniciar una captura.

---

## 7. Captura i anàlisi del trànsit

![Captura 13](img/foto13.png)

Durant la captura s'observen diferents paquets de xarxa. En l'exemple mostrat apareix trànsit **mDNS (Multicast DNS)**.

La captura permet observar el número i la mida del paquet, Ethernet II, adreces MAC, UDP, ports i informació del protocol mDNS.

![Captura 14](img/foto14.png)

També es mostra l'execució de Wireshark i la comprovació posterior de les interfícies amb:

```bash
ip a
```

---

## 8. Configuració del servei DHCP Kea

### 8.1. Fitxer de configuració

La configuració del servidor DHCP es realitza mitjançant:

```text
/etc/kea/kea-dhcp4.conf
```

En la pràctica es defineixen els temps de renovació, la base de dades de concessions, la subxarxa, les opcions de xarxa, el rang DHCP i una reserva d'adreça IP.

### 8.2. Configuració DHCP

El fragment següent correspon a la configuració que es mostra a les captures:

```json
{
  "renew-timer": 1000,
  "rebind-timer": 2000,

  "lease-database": {
    "type": "memfile",
    "persist": true,
    "name": "/var/lib/kea/kea-leases4.csv"
  },

  "subnet4": [
    {
      "subnet": "192.168.200.0/24",

      "option-data": [
        {
          "name": "routers",
          "data": "192.168.200.1"
        },
        {
          "name": "domain-name-servers",
          "data": "8.8.8.8"
        }
      ],

      "pools": [
        {
          "pool": "192.168.200.100-192.168.200.200"
        }
      ],

      "reservations": [
        {
          "hw-address": "08:00:27:57:20:fc",
          "ip-address": "192.168.200.50"
        }
      ]
    }
  ]
}
```


## 9. Comprovació de les concessions DHCP

A la pràctica es realitza una consulta directa al fitxer de concessions:

```bash
sudo cat /var/lib/kea/dhcp4.leases
```

![Captura 17](img/foto17.png)

El resultat de la captura indica:

```text
cat: /var/lib/kea/dhcp4.leases: No such file or directory
```

Això és coherent amb la configuració posterior mostrada a la pràctica, on Kea utilitza una base de dades de tipus `memfile` amb el nom:

```text
/var/lib/kea/kea-leases4.csv
```

Per consultar el fitxer definit en la configuració:

```bash
sudo cat /var/lib/kea/kea-leases4.csv
```

---

## 11. Validació de la configuració de Kea

Abans d'iniciar o reiniciar el servei, es comprova la validesa de la configuració amb:

```bash
sudo kea-dhcp4 -t /etc/kea/kea-dhcp4.conf
```

![Captura 19](img/foto19.png)

La sortida mostra que Kea carrega la subxarxa `192.168.200.0/24` i que la configuració es processa sobre la interfície indicada.

---

## 12. Reinici i comprovació del servei DHCP

Després de validar la configuració, es reinicia el servei:

```bash
sudo systemctl restart kea-dhcp4-server
```

A continuació, es comprova el seu estat:

```bash
sudo systemctl status kea-dhcp4-server
```

![Captura 20](img/foto20.png)

El resultat mostra:

```text
Active: active (running)
```

Això confirma que el servei Kea DHCPv4 està executant-se correctament en el moment de la comprovació.

---

## 13. Comprovació final de les interfícies

```bash
ip a
```

![Captura 21](img/foto21.png)

La captura mostra diverses interfícies de xarxa, entre elles enp0s3, enp0s8 i enp0s9, juntament amb les seves adreces IPv4, adreces IPv6 i estat.

---

## 14. Resum de totes les ordres

### Instal·lació de Kea

```bash
sudo apt install kea -y
```

### Instal·lació de Wireshark

```bash
sudo apt install wireshark
```

### Iniciar Wireshark

```bash
sudo wireshark
```

### Consultar les interfícies

```bash
ip a
```

### Consultar l'estat de les interfícies

```bash
ip link
```

### Consultar NetworkManager

```bash
nmcli device show
```

### Consultar el fitxer de concessions que apareix a la captura

```bash
sudo cat /var/lib/kea/dhcp4.leases
```

### Consultar el fitxer definit a la configuració de Kea

```bash
sudo cat /var/lib/kea/kea-leases4.csv
```

### Validar la configuració de Kea

```bash
sudo kea-dhcp4 -t /etc/kea/kea-dhcp4.conf
```

### Reiniciar el servei DHCP

```bash
sudo systemctl restart kea-dhcp4-server
```

### Comprovar l'estat del servei DHCP

```bash
sudo systemctl status kea-dhcp4-server
```

---

## 16. Conclusió

Aquesta pràctica ha permès configurar i verificar el funcionament de la xarxa en un sistema Zorin OS virtualitzat.

S'han treballat diferents aspectes de l'administració de xarxes: configuració d'interfícies, adreçament IPv4, consulta de rutes i DNS, captura de paquets amb Wireshark i instal·lació i configuració d'un servidor DHCP mitjançant Kea.

La validació de la configuració i la comprovació de l'estat del servei han permès confirmar que el servidor DHCP Kea queda en execució i preparat per gestionar les concessions de la subxarxa configurada.

La documentació de les ordres i configuracions en blocs de codi facilita la reproducció de la pràctica i permet copiar directament cada comanda des del document.

## Enllaç al github
https://github.com/aitorflamarich09-gif/AA2-Avaluaci-Server-DHCP#13-comprovaci%C3%B3-final-de-les-interf%C3%ADcies
