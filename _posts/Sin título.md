
**Wowza Technology** has suffered another breach, and this time security awareness training was not enough to stop it. The victim is **Patricia Kyrielight**, a document controller responsible for files tied to a classified government project still under development. Because her role gives her broad access to sensitive design and infrastructure documentation, compromising her workstation was enough to expose the inner workings of the project.

The incident came to light when an endpoint security alert flagged unauthorized remote-access software running on Patricia's machine, a direct violation of company policy. A closer look revealed far more than a policy breach: the attacker had already established persistence early in the day, then returned hours later for a hands-on session while Patricia was away from her desk. During that session they searched her files, pulled confidential material from her cloud-synced storage, and worked methodically to erase their tracks before disconnecting.

You have been handed a triage image of the affected workstation. Reconstruct the intrusion from initial access through exfiltration, account for the attacker's anti-forensic activity, and determine the full extent of what was accessed and taken.

## Initial Access
1. Tracing back to the source, according to the user's testimony, a weird pop-up kept appearing after she pasted a command from a suspicious site asking her to run something. It wouldn't stop until she clicked "Yes". Determine the URL responsible for hosting this command and the Microsoft product impersonated by that page.
	- En la tabla visited_links de Brave nos aparece este enlace muy sospechoso porque es http siendo, se supone, un servicio de Windows
![](assets/img/posts/Pasted%20image%2020261020132113.png)

## Persistence
1. During evidence acquisition, a remote desktop application was discovered running on this employee's machine, which violated company policy. Determine when this application was installed on the system.
	- En el HIVE System, nos vamos a Services y apuntamos la hora
![](assets/img/posts/Pasted%20image%2020261020133119.png)

2. Windows Defender flagged the installation of this application. Identify the process ID of the installation process and the full command line used to execute it.
	- Buscamos en el archivo MPLog por la fecha de instalación del servicio (16:32 más o menos).
	- Más abajo aparece el PID.
![](assets/img/posts/Pasted%20image%2020261020134605.png)

![](assets/img/posts/Pasted%20image%2020261020135453.png)

  
## Privilege Escalation
1. Upon execution, several commands were run on the system including persistence attempts. Identify the RID of the account that was enabled, and provide the paths of the scripts assigned to it on logon and logoff.
	- El RID lo podemos ver en el SAM y el script de logon y logoff en SOFTWARE\Microsoft\Windows\CurrentVersion\Group Policy\Scripts\
![](assets/img/posts/Pasted%20image%2020261020201901.png)


![](assets/img/posts/Pasted%20image%2020261020201510.png)

2. Provide the full command found in the logon script discovered in the previous question.
	- Con el parámetro --dr podemos sacar los archivos pequeños borrados cambiandole el .bin y dejandolo en .bat
![](assets/img/posts/Pasted%20image%2020261020203104.png)

## Defense Evasion
1. Exclusion paths and processes were also added to Windows Defender. How many entries were added in total, and what are the processes added to the exclusion list?
	- Se encuentra en SOFTWARE\Microsoft\Windows Defender\Exclusions\Paths y SOFTWARE\Microsoft\Windows Defender\Exclusions\Processes

![](assets/img/posts/Pasted%20image%2020261020203752.png)

![](assets/img/posts/Pasted%20image%2020261020203803.png)

2. After establishing persistence, several event logs were cleared. How many logs were cleared? Identify the first and last event log cleared during this process. (answer as filename)
	- En J$, filtramos por .evtx y en Update Reasons: Data Truncated|Data Overwrite o 
DataOverwrite|DataExtend|DataTruncation:
![](assets/img/posts/Pasted%20image%2020261020205804.png)

## Command and Control
1. The threat actor took advantage of the user stepping away and gained access to the system via the installed remote desktop application. Identify when this access occurred.
	- Volvemos a usar el parámetro --dr de MFTCmd para ver los archivos Resident. En este caso tenemos que ir en busca del archivo connection_trace.txt.
![](assets/img/posts/Pasted%20image%2020261021181919.png)

2. The remote desktop application assigns unique IDs to both ends of a connection. Identify the ID assigned to the threat actor and the ID assigned to the user's machine.


---
## Discovery
1. After gaining access, the threat actor browsed both the file system and browser looking for interesting data, and came across a confidential folder synced from cloud storage. Identify the file the threat actor opened during this phase.
	- Con los ficheros LNK podemos ver qué archivos abrió el atacante
![](assets/img/posts/Pasted%20image%2020261022135324.png)

  
## Collection
1. Proceeding with exfiltration, the threat actor used the user's browser to download a file compression utility. Browser history was cleared, but traces remain. Identify the full URL of the page used to obtain the files.
	- 