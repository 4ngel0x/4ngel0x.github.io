---
title: CyberDefenders - Crossed Signals Lab
date: 2026-09-01 10:00:00 +0100
categories:
  - Writeups
tags:
  - windows
  - sysmon
  - EZ_Tools
description: "Writeup · Crossed Signals Lab"
image:
  path: assets/img/posts/crossed-signals.png
---
# ESCENARIO
Maromalix Legal is a litigation firm representing a client in a lawsuit against Meridian Holdings. The firm's Office Manager, Sarah Mitchell (smitchell), reported unusual contact from someone claiming to be a security auditor. Weeks later, opposing counsel appeared to know details of the firm's privileged case strategy — details that were never disclosed. The firm suspects their systems were compromised and confidential legal documents were stolen. Analyze the provided forensic artifacts to reconstruct the full attack chain — from initial contact to data theft.

## HERRAMIENTAS
- BrowsingHistoryView, EZ Tools

---
## Victim Environment
1. The scenario mentions that someone claiming to be a security auditor contacted smitchell via email and instructed her to move the conversation to a different platform. Based on the victim's browser activity, what browser was she using and what messaging application was she directed to install?
	- Usamos BrowsingHistoryView y vemos qué navegador usa
![](assets/img/posts/Pasted%20image%2020260830185108.png)

  
## Credential Access
1. The attacker lured the victim into authenticating on a site that turned out to be a credential phishing page. What is the full URL of that page?
	- En la misma herramienta que antes, vemos que 24 minutos después se mete en esta web.
![](assets/img/posts/Pasted%20image%2020260830185418.png)

	- Compruebo en Virustotal y efectivamente, la web es una página de phising
![](assets/img/posts/Pasted%20image%2020260830185822.png)

2. The domain used in the phishing attack has been documented in threat intelligence reporting. What is the name of the phishing campaign this domain is associated with?
	- Con una búsqueda rápida lo vemos
![](assets/img/posts/Pasted%20image%2020260830190117.png)

3. As part of the fake security assessment, the attacker convinced the victim that documenting her credentials locally was required for compliance. The victim fell for it and saved her credentials to a file. What is the full path of that file?
	- Viendo un poco más abajo en BrowsingHistoryView, podemos ver donde guradó las credenciales
![](assets/img/posts/Pasted%20image%2020260830190434.png)

4. What is the victim's password found inside that file?
	- Si usamos la herramienta MFTECmd con el parámetro --dr, podremos ver los archivos pequeños borrados. Lo único que hay que hacer es rneombrar el fichero por .txt y listo.
![](assets/img/posts/Pasted%20image%2020260830193650.png)

5. During the fake assessment, the victim grew suspicious and turned to her browser to verify the auditor's claims. What search query reveals the victim's doubt?
	- Podemos ver la búsqueda en BrowsingHistoryView
![](assets/img/posts/Pasted%20image%2020260830193848.png)

## Remote Access & Persistence
1. Using the stolen credentials, the attacker remotely authenticated to the victim's machine. What is the source IP address and at what time (UTC) did the logon occur?
	- En los Security logs, filtramos por Event ID 4624 y nos fijamos en inicios de sesión a partir de las 8:47 PM que fue cuando la victima entró en la página de phising. Además, hay que fijarse en el tipo de login. Al ser una autneticación remota, nos fijamos en Event Code 3 y 10.
![](assets/img/posts/Pasted%20image%2020260830201034.png)

2. What is the hostname of the attacker's machine, and based on the naming convention, what cloud provider is it likely hosted on?
	- En el mismo log que la pregunta anterior podemos verlo. Al ser un EC2, sabemos que pertenece a AWS.
![](assets/img/posts/Pasted%20image%2020260830201132.png)

3. Right after gaining access, the attacker attempted to install a remote management tool multiple times but failed. What is the name of that tool?
	- Ahora nos vamos a System.evtx y filtramos por Event Code 7000 (servicio no pudo iniciarse)
![](assets/img/posts/Pasted%20image%2020260830201627.png)

4. What was the C2 endpoint (address and port) and the authentication token configured for the RMM agent?
	- Nos vamos al ConsoleHost_history.txt en PSReadline y lo localizamos.
![](assets/img/posts/Pasted%20image%2020260902194619.png)

5. After the RMM installation failed, the attacker pursued an alternative persistence method. What is the MITRE ATT&CK technique ID?
	- La clave está al final del ConsoleHost_history.txt, en la creación del usuario
![](assets/img/posts/Pasted%20image%2020260902195509.png)

![](assets/img/posts/Pasted%20image%2020260902195440.png)

  
## Discovery & Collection
1. Now that the attacker ensured he could come back, he started searching File Explorer for documents related to the ongoing lawsuit. What are the last two keywords the attacker searched for?
	- Nos dirigimos a NTUSER.dat\Software\Microsoft\Windows\CurrentVersion\Explorer\WordWheelQuery  y vemos lo último que ha bsucado
![](assets/img/posts/Pasted%20image%2020260903190550.png)

2. What case folder did the attacker ultimately target?
	- Ahora nos vamos a Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs para ver archivos y carpetas abiertos recientemente por el Explorador

![](assets/img/posts/Pasted%20image%2020260903191040.png)

3. We need to scope what files the attacker actually got his hands on. How many files did the attacker open from the Meridian case folder, and what was the first file he accessed?
	- Para este caso, tenemos que ver los archivos LNK que registra cualquier archivo que el usuario abrió, independientemente de con qué aplicación.
	- Como el atacante accedió a la carpeta a las 21:44:11 (visto en el ejercicio anterior), tenemos que ver a partir de ese entonces
![](assets/img/posts/Pasted%20image%2020260903192554.png)

  
## Encrypted Communications Recovery
1. We need to recover the conversation between the victim and the attacker to profile the threat actor. Signal Desktop encrypts its database using a key protected by Windows DPAPI. What is the DPAPI Master-Key GUID associated with Signal for this victim?
	- He tirado de IA para sacar esto. He necesitado el archivo config.json y Local State
![](assets/img/posts/Pasted%20image%2020260903200620.png)
![](assets/img/posts/Pasted%20image%2020260903200630.png)

2. After decrypting the DPAPI master key, the next step is to decrypt the auxiliary key stored in Signal's Local State file. What is the decrypted auxiliary key (hex)?
	- Esta pregunta me ha costado la salud al no entender lo que estaba haciendo. 
	- Primero tienes que quitar la cabecera de 5 bytes que corresponde a DPAPI en la key de LocalState y guardarlo en un fichero. 
	- Luego sacas la master key usando mimikatz con la contraseña de las preguntas anteriores y con el path Protect: dpapi::masterkey /in (path) /password: (contraseña)
	- Por último, se usa impacket-dpapi (lo tuve que hacer en kali) con la key sacada de mimikatz para desencriptar.
![](assets/img/posts/Pasted%20image%2020260906130140.png)

3. What is the final SQLCipher key needed to open Signal's db.sqlite?
	- Teniendo la key, ejecuto un script de python para desencriptar la clave dento de config.json y accedo, por fin, a la db.sqlite.
![](assets/img/posts/Pasted%20image%2020260906132905.png)

4. With the database now decrypted — when did the attacker first reach out to Sarah, and what name did he use?
	- Nos tenemos que ir a la tabla messages.
![](assets/img/posts/Pasted%20image%2020260906180131.png)

5. What regulatory standard did the attacker cite to build credibility and pressure the victim into complying?
	- Simplemente hay que ir mirando las conversaciones hasta encontrar la respuesta
![](assets/img/posts/Pasted%20image%2020260906180459.png)

6. What was the last message sent by the attacker?
	- Lo mismo que en la pregunta anterior.
![](assets/img/posts/Pasted%20image%2020260906180642.png)
