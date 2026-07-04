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
## Initial Access
1. The attack started with a malicious document that the victim opened with Microsoft Word. What is the name of this malicious document?
	- Se puede ver dentro de Sysmon Event ID 1 y filtrando por .doc. Vemos un archivo sospechoso ya que Word normal nunca debería lanzar rundll32.exe como proceso hijo.
![](assets/img/posts/Pasted%20image%2020260704105958.png)

2. Opening the malicious document triggered the execution of a child process. What is the name and PID of this process?
	- Lo podemos ver dentro de la respuesta anterior. 
![](assets/img/posts/Pasted%20image%2020260704111210.png)

## Command and Control
1. Opening the document file triggered a connection back to C2. What is the C2 IP Address
	- En este punto sabemos que al abrir el Word, se ejecutó rundll32 con el PID 2064 por lo que vamos a filtrar en Sysmon Event ID 3 por ese PID.
	- Vemos que hay muchas conexiones a través de diferentes puertos.
![](assets/img/posts/Pasted%20image%2020260704112702.png)

## Discovery
1. After gaining remote shell in victim machine, attacker started his malicious activity by some discovery commands, what's the first command executed and it's time stamp?
	- En Sysmon Event ID 1 filtramos por cmd y por fecha y hora para que nos muestre los eventos posteriores a las 14:19 del 8 de abril, que es cuando se accedió por primera vez al archivo.
![](assets/img/posts/Pasted%20image%2020260704115029.png)

2. After executing some reconnaissance, threat actor dropped and executed malicious file to perform intensive enumeration, what's the full path of this tool, and it's original file name?
	- Como el atacante dropeó un archivo, nos movemos al Event ID 11 y filtramos por el nombre christian para afinar más el resultado
	- Sabiendo que el ejecutable malicioso se llama hh.exe, filtramos el Event ID 1 por ese nombre y vemos el nombre original.

![](assets/img/posts/Pasted%20image%2020260704125439.png)

![](assets/img/posts/Pasted%20image%2020260704120205.png)

## Privilege Escalation
1. After completing enumeration successfully. The attacker escalated privileges by spawning a new rundll32.exe process running with administrative rights, giving them an elevated foothold on the host. When did this first malicious privileged rundll32.exe process run?
	- Para esta pregunta tenemos que buscar a partir de las 14:35 ya que en ese momento se ejecutó el último comando de reconocimiento. A partir de ahí buscamos en Sysmon Event ID 1 filtrando por rundll32 y buscando por el nivel de integridad HIGH con el usuario christian.
![](assets/img/posts/Pasted%20image%2020260704133712.png)

2. During privilege escalation, the attacker dropped a binary onto a share to spawn additional rundll32.exe processes for later post-exploitation activity. What is the full path of this binary?
	- Seguimos en Sysmon Event ID 1 y filtramos por el share ADMIN$ (una carpeta compartida típica). También vemos que se sigue usando rundll32 como proceso malicioso.
![](assets/img/posts/Pasted%20image%2020260704134153.png)
## Persistence
1. After elevating privileges, the attacker dropped a persistence binary into a system directory, a location only writable from a SYSTEM-level context. What is the full path of this file?
	- Como nos dicen que ha dejado un archivo accesible solo a través de directorios de SYSTEM, filtramos en Sysmon Event ID 11 por NT AUTHORITY\SYSTEM.
![](assets/img/posts/Pasted%20image%2020260704193306.png)

2. The attacker registered a scheduled task that re-launches the persistence binary at regular intervals. What is the name of this task?
	- En Sysmon EVent ID 1 filtramos por schtask y vemos la tarea programda que, de hecho, venía codificada.
![](assets/img/posts/Pasted%20image%2020260704114532.png)

## Discovery
1. Threat actor started a second phase of discovery, what is the 2 powershell cmdlets used by threat actor in network discovery?
	- Filtramos en Sysmon Event ID 1 por el nombre del ejecutable que nos hemos encontrado anteriormente y nos aparecen dos comandos codificados muy seguidos. Decodificándolos en CyberChef nos aparecen ambos comandos.
![](assets/img/posts/Pasted%20image%2020260704194424.png)

## Credential Access
1. During Credential Access, the actor targeted LSASS memory. The threat actor dropped a known system admin tool to dump the lsass. What's the original tool name used, and what's the file that stores the output?
	- Filtramos por lsass y nos aparecen los datos.
![](assets/img/posts/Pasted%20image%2020260704195444.png)

2. The LSASS dumping binary was renamed before it was executed against lsass.exe. What's the file path of this lsass dumper tool?
	- Si nos fijamos en la imagen anterior, nos aparece la ruta del ejecutable pc.exe, previamente llamado ProcDump.

## Command and Control
1. After credential dumping, the attacker used the persistence executable to deploy a new client beacon to continue command-and-control?
	- La clave de esta pregunta está en que el ejecutable ha desplegado un beacon nuevo, por lo que tenemos que irnos a Sysmon Event ID 11 filtrando por schshell.exe.
![](assets/img/posts/Pasted%20image%2020260704202148.png)

## Execution
1. The process in the previous question started another process, which is the child process path?
	- Nos vamos a Sysmon Event ID 11 y filtramos por jobUpdater.exe.
![](assets/img/posts/Pasted%20image%2020260704202838.png)

2. What's the SHA256 hash of the child process?
	- Filtrando por el ejecutable malicioso en Sysmon Event ID 1 nos aparece.
![](assets/img/posts/Pasted%20image%2020260704203024.png)

3. The last dropped file, flagged as a Rhadamanthys-family sample, was executed repeatedly during this phase. Using Windows execution artifacts, how many times was this file executed?
	- Cambiamos de origen de los logs y nos vamos en busca del archivo INTERESTELLER.PF. Paseándolo con PECmd y visualizándolo con TimelineExplorer (bendito Eric Zimmerman) nos sale que se ha ejecutado 4 veces.
![](assets/img/posts/Pasted%20image%2020260704203848.png)

4. For threat-intelligence enrichment of the Rhadamanthys Loader sample, look up its hash on VirusTotal. What is the Compilation Timestamp of this malware, and the First Seen In The Wild date?
	- Corta y al pie
![](assets/img/posts/Pasted%20image%2020260704204105.png)

5. The repeated executions of the Rhadamanthys Loader suggest it was retrying to fetch its next-stage payload. Some time later, the actual Rhadamanthys Stealer DLL appeared on the host and was executed. What is the path of this last dropped file?
	- Filtramos en Sysmon Event ID 1 por Powershell a partir de las 16:47 ya que esa hora fue en la que apareció el ejecutable intersteller.exe.

![](assets/img/posts/Pasted%20image%2020260704205410.png)

- En VirusTotal podemos ver que el formato del dll coincide con el comportamiento típico del stealer.
![](assets/img/posts/Pasted%20image%2020260704214334.png)

6. What's the SHA256 hash of this file on dropped on system
	- Nos vamos a la carpeta donde se encuentra el .dll y sacamos el hash
![](assets/img/posts/Pasted%20image%2020260704215805.png)

7. The final stealer DLL was invoked through rundll32.exe with an exported function name. What's the command line that tried execute this file?
	- Nos fijamos en la misma linea de comando que antes y ahí nos aparecerá con la función exportada
![](assets/img/posts/Pasted%20image%2020260704232412.png)
