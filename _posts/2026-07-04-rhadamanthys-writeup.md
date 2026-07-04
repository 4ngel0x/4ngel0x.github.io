---
title: Rhadamanthys Lab
date: 2026-07-04 10:00:00 +0100
categories:
  - Writeups
tags:
  - windows
  - sysmon
  - malware
description: Writeup Rhadamanthys Lab
---
# ESCENARIO
On 8 April 2026, a targeted social-engineering campaign tricked the user christian into executing a malicious document from the internet. Once the document was opened, it triggered remote code execution and kicked off a full attack chain through privilege escalation, persistence, LSASS dumping, and ultimately the Rhadamanthys Stealer. Because the beachhead host sits outside SIEM coverage, you have been provided with a disk image of the compromised machine to perform your analysis and reconstruct the attack.

---
# Initial Access
1. The attack started with a malicious document that the victim opened with Microsoft Word. What is the name of this malicious document?
	- Se puede ver dentro de Sysmon Event ID 1 y filtrando por .doc. Vemos un archivo sospechoso ya que Word normal nunca debería lanzar rundll32.exe como proceso hijo.
![](assets/img/posts/Pasted%20image%2020260704105958.png)

2. Opening the malicious document triggered the execution of a child process. What is the name and PID of this process?
	- Lo podemos ver dentro de la respuesta anterior. 
![](assets/img/posts/Pasted%20image%2020260704111210.png)

# Command and Control
1. Opening the document file triggered a connection back to C2. What is the C2 IP Address
	- En este punto sabemos que al abrir el Word, se ejecutó rundll32 con el PID 2064 por lo que vamos a filtrar en Sysmon Event ID 3 por ese PID.
	- Vemos que hay muchas conexiones a través de diferentes puertos.
![](assets/img/posts/Pasted%20image%2020260704112702.png)

# Discovery
1. After gaining remote shell in victim machine, attacker started his malicious activity by some discovery commands, what's the first command executed and it's time stamp?
	- En Sysmon Event ID 1 filtramos por cmd y por fecha y hora para que nos muestre los eventos posteriores a las 14:19 del 8 de abril, que es cuando se accedió por primera vez al archivo
![](assets/img/posts/Pasted%20image%2020260704115029.png)

2. After executing some reconnaissance, threat actor dropped and executed malicious file to perform intensive enumeration, what's the full path of this tool, and it's original file name?
	- Como el atacante dropeó un archivo, nos movemos al Event ID 11 y filtramos por el nombre christian para afinar más el resultado
	- Sabiendo que el ejecutable malicioso se llama hh.exe, filtramos el Event ID 1 por ese nombre y vemos el nombre original-

![](assets/img/posts/Pasted%20image%2020260704125439.png)

![](assets/img/posts/Pasted%20image%2020260704120205.png)

# Privilege Escalation
1. After completing enumeration successfully. The attacker escalated privileges by spawning a new rundll32.exe process running with administrative rights, giving them an elevated foothold on the host. When did this first malicious privileged rundll32.exe process run?
	- Para esta pregunta tenemos que buscar a partir de las 14:35 ya que en ese momento se ejecutó el último comando de reconocimiento. A partir de ahí buscamos en Sysmon Event ID 1 filtrando por rundll32 y buscando por el nivel de integridad HIGH con el usuario christian.
![](assets/img/posts/Pasted%20image%2020260704133712.png)

2. During privilege escalation, the attacker dropped a binary onto a share to spawn additional rundll32.exe processes for later post-exploitation activity. What is the full path of this binary?
	- Seguimos en Sysmon Event ID 1 y filtramos por el share ADMIN$ (una carpeta compartida típica). También vemos que se sigue usando rundll32 como proceso malicioso.
![](assets/img/posts/Pasted%20image%2020260704134153.png)
# Persistence
1. After elevating privileges, the attacker dropped a persistence binary into a system directory, a location only writable from a SYSTEM-level context. What is the full path of this file?

2. The attacker registered a scheduled task that re-launches the persistence binary at regular intervals. What is the name of this task?
	- En Sysmon EVent ID 1 filtramos por schtask y vemos la tarea programda que, de hecho, venía codificada
![](assets/img/posts/Pasted%20image%2020260704114532.png)

3. 

