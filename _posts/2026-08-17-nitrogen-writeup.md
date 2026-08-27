---
title: CyberDefenders - Nitrogen - Blackcat Ransomware Lab
date: 2026-08-26 10:00:00 +0100
categories:
  - Writeups
tags:
  - windows
  - sysmon
  - EZ_Tools
  - splunk
description: "Writeup · Nitrogen - Blackcat Ransomware\r

  \ Lab"
image:
  path: assets/img/posts/silverclaw2.png
---
# ESCENARIO
On September 10, 2025, trustwave.lab’s SOC team identified suspicious activity originating from a user workstation. The investigation revealed that the compromise began when the user searched online for legitimate software and clicked on an advertised website appearing in the first search results. Unbeknownst to the user, this site hosted a malicious download, which they executed with a single click, initiating a chain of malicious activity across the environment.

Subsequent events included staged payloads, beaconing to command-and-control (C2) infrastructure, lateral movement to the file server and domain controller, credential dumping, and attempts at data exfiltration.

Your task is to perform a full incident investigation using Splunk telemetry, process creation logs, registry artifacts, scheduled task information, network connections, and forensic disk images to reconstruct the attacker’s actions, identify persistence mechanisms, and determine the impact on trustwave.lab’s network.

**Splunk Credentials:**

- User: `student`
- Password: `CyDefStudent`

## HERRAMIENTAS
- BrowsingHistoryView, Splunk, EZ Tools

---
## Initial Access
1. Before the compromise, the victim performed a web search that led them to the malicious download. What exact search query did the victim enter into the browser?
	- Vamos a usar BrowsingHistoryView para ver exactamente qué buscó el usuario el 10 de septiembre de 2025.
![](assets/img/posts/Pasted%20image%2020260825123758.png)

2. After conducting a search, the victim accessed a malicious site. What was the name of the rogue website they visited prior to the compromise?
	- En la misma ventana podemos ver el nombre de la web. Hora de visita a la web para tenerla de referencia: 9:54:09 AM
![](assets/img/posts/Pasted%20image%2020260825124355.png)

3. After the victim visited the rogue site, a file was downloaded. What is the full path to the file that was downloaded on the victim machine?
	- Se puede ver de varias maneras pero en mi caso he buscado en Sysmon Event ID 11 cerca de las 9:54:09 AM.
![](assets/img/posts/Pasted%20image%2020260825130336.png)

## Execution
1. After the malicious archive was downloaded, the victim user extracted its contents and executed a program inside it. What was the name of the executable that was launched, and which DLL did it attempt to load?
	- Para saber el nombre del ejecutable, nos vamos a UserAssist y vemos que la útlima vez que se ejecutó fue a las 09:57:04:
![](assets/img/posts/Pasted%20image%2020260825132701.png)

	- Para saber qué DLL ha intenado cargar, tenemos que ver el Prefetch del ejecutable previamente descubierto:

![](assets/img/posts/Pasted%20image%2020260825132802.png)

2. Immediately after the executable from Q4 ran, a second-stage component was launched. What is the full path to the script file that was executed as the next stage?
	- Nos vamos a Sysmon Event ID 1 y buscamos a partir de las 09:57:04. Si no hubiese salido tan fácil, hubiese filtrado por python o por la ruta del ejecutable malicioso para afinar más pero no ha hecho falta
![](assets/img/posts/Pasted%20image%2020260825134338.png)

3. Based on forensic analysis, what Python library was used for decryption after the second-stage script ran, and what was the name of the encrypted file that was decrypted?
	- En MFT filtramos por el path del script para ver qué hay en esa misma carpeta. Vemos que aparece tanto el fichero encriptado como la librería.
![](assets/img/posts/Pasted%20image%2020260825182119.png)

4. On the file server, a remote file was downloaded by the attacker. What is the full path to that downloaded file on the FILES host?
	- Para esta pregunta voy a usar Splunk simplemente por darle un poco de dificultad y manejarme un poco más en el SIEM. He filtrado por SysmonEvent ID 1 en el FS:
![](assets/img/posts/Pasted%20image%2020260825183355.png)

5. After the downloaded file was unpacked on the Files server, what three files were written into the staging directory?
	- Filtramos por la carpeta que hemos visto en la pregunta anterior para ver exactamente qué se ha descomprimido.
![](assets/img/posts/Pasted%20image%2020260825184752.png)

## Command & Control
1. Shortly after the staged script ran, the host made an outbound connection to the attacker’s C2. What is the destination IP address and port used for that first C2 communication?
	- En este caso me he ido a Sysmon Event ID 3 en el Visor de Eventos y sobre las 9:57 (que es la hora que nos señala el enunciado: "Shortly after the staged script ran") aparece que rundll32.exe ha hecho una conexión, cosa que no es normal en este ejecutable.
![](assets/img/posts/Pasted%20image%2020260825191736.png)

2. After the initial C2 connection, the attacker deployed additional beacons. Which ports did these newly executed files use to connect to the attacker’s server?
	- Tras ver que se ha creado wof12.exe y wof14.exe, vemos hacia qué puertos se han conectado:
![](assets/img/posts/Pasted%20image%2020260826191416.png)

3. After deploying additional beacons on the files server, the attacker communicated with a secondary infrastructure. What is the domain name of the second C2 server used during the attack?ç
	- Primero buscamos el beacon adicional descargado en el FS para posteriormente filtrar la query.
![](assets/img/posts/Pasted%20image%2020260826194912.png)

	- En Splunk, filtramos por Sysmon Event ID 22 para ver los dominios contactados. 
![](assets/img/posts/Pasted%20image%2020260826195351.png)

	- Vemos que, efectivamente, el dominio es malicioso.
![](assets/img/posts/Pasted%20image%2020260826195508.png)

4. The attacker moved a beacon from the Files server to the Domain Controller. What exact command did they use to copy the file across the network?
	- Filtramos por Event ID 1 y buscamos comandos donde veamos claramente que se han copiado archivos; nos quedamos con la hora de copiado 12:33:44.
![](assets/img/posts/Pasted%20image%2020260826200110.png)

5. After executing the beacon on the Domain Controller in Q12, which IP address and port were used for C2 communication?
	- Filtrando por el beacon copiado desde FS, vemos que solo se conectó a esa IP y a ese puerto unas 103 veces
![](assets/img/posts/Pasted%20image%2020260826200618.png)

## Persistence
1. The attacker created scheduled tasks for persistence. What was the name of the first scheduled task created on the first compromised host?
	- Vemos que ha creado varias tareas programas pero nos piden la primera solamente.
![](assets/img/posts/Pasted%20image%2020260826201014.png)

2. On the file server, the attacker also used scheduled tasks for persistence. How many times per day will the attacker’s final scheduled task be executed?
	- Vemos que se va a ejecutar cada 720 minutos (cada 12 horas)
![](assets/img/posts/Pasted%20image%2020260826201303.png)

3. The attacker modified the Windows registry to achieve persistence. Which registry value name did they change?
	- Filtramos por reg add:
![](assets/img/posts/Pasted%20image%2020260826203808.png)

## Reconnaissance
1. During the reconnaissance phase the attacker enumerated domain trust relationships. Which command did they run to collect that information?
	- Filtramos por Sysmon Event ID 1 a partir de las 9:57
![](assets/img/posts/Pasted%20image%2020260826204900.png)

2. The attacker leveraged an automated script to gather system details. What was the complete URL of the remote script that was downloaded and executed?
	- Miramos más abajo y lo encontramos
![](assets/img/posts/Pasted%20image%2020260826205035.png)

## Credential Access
1. The attacker managed to dump hashes on the PC01 host. When did this dump occur?dsv
	- Filtramos por lsass.exe en el host PC01
![](assets/img/posts/Pasted%20image%2020260827104258.png)

2. The attacker attempted to dump credentials from LSASS on the Files server. What GrantedAccess code was observed when the attacker accessed lsass.exe?
	- Vemos que wof15 accedió a lsass con el control absoluto 
![](assets/img/posts/Pasted%20image%2020260827095854.png)

  
## Lateral Movement
1. The attacker laterally moved to the Files server. What was the username they used to log in during this movement?
	- Como sabemos que la ip del PC01 es 10.10.11.39, vemos que la primera persona que se logueó , a parte de James, fue liam
![](assets/img/posts/Pasted%20image%2020260827105721.png)

2. The attacker logged into the Domain Controller using RDP. What was the source IP address they connected from?
	- Filtramos por Event Code 4624 y Logon Type= 10
![](assets/img/posts/Pasted%20image%2020260827111207.png)

## Exfiltration
1. The attacker used a command-line utility to push collected files off-network. What was the name of the tool they used to perform the exfiltration?
	- Viendo Event Code 1, me fijo que han usado restic.exe
![](assets/img/posts/Pasted%20image%2020260827112337.png)

## Impact
1. The attacker modified Safe Mode settings so a service would be allowed to start when the system boots into Safe Mode with Networking. What is the exact registry subkey name that the attacker created ?
	- Filtramos por reg.exe
![](assets/img/posts/Pasted%20image%2020260827113414.png)

2. The attacker attempted to push a ransomware binary to every machine in the environment. What is the name of the executable the attacker distributed?
	- Filtro por Event Code 1 y copy para ver qué se ha copiado
![](assets/img/posts/Pasted%20image%2020260827115752.png)

3. The attacker executed a single command to run a script remotely on every host listed in their target file. What was the full command they used to launch the remote execution?
	- Para esta pregunta lo primero que se me ha venido a la cabeza ha sido PsExec así que he filtrado por ese nombre para ver si lo había usado. Y efectivamente, lo ha usado:
![](assets/img/posts/Pasted%20image%2020260827121311.png)

4. The attacker dropped a ransom note on the Domain Controller. What exact URL did the note instruct the victim to visit?
	- Filtramos por .txt y vemos la ubicación del .txt.
![](assets/img/posts/Pasted%20image%2020260827121618.png)

	- Como tenemos la imagen, nos metemos y sacamos la URL
![](assets/img/posts/Pasted%20image%2020260827121746.png)
