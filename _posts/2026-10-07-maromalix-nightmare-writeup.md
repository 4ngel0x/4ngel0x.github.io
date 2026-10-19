---
title: CyberDefenders - Maromalix Nightmare Lab
date: 2026-10-19 10:00:00 +0100
categories:
  - Writeups
tags:
  - windows
  - malware
  - ELK
  - EZ_Tools
description: "Writeup · Maromalix Nightmare Lab"
image:
  path: assets/img/posts/maromalix-nightmare.png
---
# ESCENARIO
Maromalix is a mid-sized data analytics firm that spent eighteen months competing for a global sports-data partnership worth more than any contract in their history. The kind of deal that changes a company's trajectory. They prepared thoroughly, priced carefully, and submitted with confidence. They lost. To a competitor whose proposal matched their internal figures with a precision that cannot be explained by research or coincidence.

Leadership suspects the bid was stolen before it was submitted. A workstation in the finance team logged unusual activity in the hours before the deadline, and the deeper the initial review went, the more deliberate that activity looked. Someone moved through this network quietly and patiently, working from one machine to the next and opening file after file not seizing everything within reach, but searching, discarding most of what they found, until they reached the one thing they had been sent to take.

You have been brought in to reconstruct what happened. The logs remember. Start there.


---
## Initial Access
1. In the weeks before the World Cup, promoted ads on Facebook were pushing users toward fake FIFA 2026 ticketing and fan-services sites. One of Maromalix's finance employees took the bait, landed on one of these pages, and downloaded what looked like an official FIFA fan-ID verification tool. What is the domain that served the lure, and what is the file name of the downloaded executable?
	- Para saber qué archivo se descargó, filtramos por EventCode: 3 y por "FIFA" ya que sabemos que se hicieron pasar por ellos.
	- Para saber el dominio desde donde se descargó, usamos BrowsingHIstoryView señalando a la WKSTN-01y al usuario omar.hassam sobre las 11:42 del 21/06/2026
![](assets/img/posts/Pasted%20image%2020261009123035.png)

![](assets/img/posts/Pasted%20image%2020261009123243.png)

2. This lure belongs to a much larger fraud operation that abused promoted Facebook ads and thousands of pixel-perfect fake FIFA ticketing sites to target millions of 2026 World Cup fans. Search open threat intelligence for reporting tied to this lure domain and identify the campaign it is associated with. What is the campaign name?
	- En la primera búsqueda nos aparece el nombre de la campaña.
![](assets/img/posts/Pasted%20image%2020261009123625.png)

3. Which employee fell for the lure and executed the downloaded file, and at what time (UTC) did they run it?
	- Parseamos el ejecutable malicioso y nos fijamos en la columna Last Run
![](assets/img/posts/Pasted%20image%2020261009124623.png)

---
## Credential Access
1. Once on the host, the loader started hunting for stored secrets, first sweeping the user directories for files with credential-related names then checking which web browsers were currently running. What is the exact command the attacker used to enumerate the running browsers?
	- Tenemos que ver el EventCode:1 después de la hora de ejecución del malware.

![](assets/img/posts/Pasted%20image%2020261009125644.png)

2. Having identified the running browser, the attacker dropped a purpose-built tool to steal its saved credentials. What is the name of that executable, and what is the GitHub username of the researcher behind this tool?
	- Más abajo nos aparece el nombre de la herramienta la cual buscamos por internet para que nos aparezca el creador de la herramienta.

![](assets/img/posts/Pasted%20image%2020261009130115.png)

![](assets/img/posts/Pasted%20image%2020261009130126.png)

3. The dumper accessed the running browser process to steal its saved credentials. What is the process ID of that browser instance, and what was the granted access (granted access in hex)?
	- Filtramos por EventCode:10 y por EdgeSaved para que nos busque procesos accediendo a otros.
![](assets/img/posts/Pasted%20image%2020261009131146.png)

4. We can't confirm exactly which credentials the attacker walked away with, but right after the theft he began setting up a tunnel so he could RDP into the private network. What tool did he use to build this tunnel, and what address did it first connect out to?
	- Seguimos con EventCode:1 y localizamos la herramienta chisel en los logs.

![](assets/img/posts/Pasted%20image%2020261009133354.png)

5. The attacker then RDP'd back into the same workstation, and the first thing he did was dump the browser process to look for more credentials. On which workstation did this happen, and what is the final full path of that dump file?

![](assets/img/posts/Pasted%20image%2020261018192805.png)

6. The employee didn't save his workstation credentials in the browser password manager, but we suspect he kept them in one of his Notion notes. Recover and provide the credentials the attacker used to access the employee's Notion.
	- Usamos strings: strings.exe -accepteula -n 6 Bookmarks > s.txt
	- El comando saca todo el texto legible (ASCII y UTF-16, de 6 o más caracteres) del archivo `Bookmarks` y lo guarda en `s.txt`.
	- Filtramos por Notion y nos aparecen las claves
![](assets/img/posts/Pasted%20image%2020261018202053.png)

---
  
## Privilege Escalation
1. The first compromised user had only normal permissions, so the attacker dropped a binary to elevate his privileges. What is the full path of this executable, and which Microsoft Defender DLL did it load?
	- Tras encontrar el path del ejecutable con event.code:11, filtramos por event.code:7 y RoguePlanet.exe
![](assets/img/posts/Pasted%20image%2020261019122727.png)

![](assets/img/posts/Pasted%20image%2020261019124411.png)
