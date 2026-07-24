---
title: CyberDefenders - Gh0stNet Intrusion Lab
date: 2026-07-17 10:00:00 +0100
categories:
  - Writeups
tags:
  - windows
  - sysmon
  - EZ_Tools
description: Writeup · Gh0stNet Intrusion Lab
image:
  path: assets/img/posts/ghostnet-intrusion.png
---
# ESCENARIO
In late May 2026, Kevin Briggs, an analyst at the Chicago investment firm Vireo Financial Group, attended the FinServ Innovation Summit and picked up a branded giveaway at the "DataVault Analytics" booth: a small USB flash drive along with some stickers

One evening at home he plugged it into his personal PC to explore what was on it, he double-clicked what looked like a spreadsheet on the drive. A window flickered and vanished; nothing else seemed to happen, so he thought no more of it. In the background, the disguised launcher had quietly deployed a remote-access trojan that took root on the PC and called out to its operators.

Weeks later, Vireo's security team flagged anomalous activity inside the corporate domain VireoFinancial.local - a valid account behaving strangely, off-hours, from an unfamiliar source. The trail led back to Kevin, and from there to a home PC nobody had been monitoring. You have been brought in to reconstruct the incident across its two stages: the initial compromise on Kevin's home PC, and the hands-on-keyboard intrusion that followed inside the corporate network.

You are given three artifacts:

-  homepc.E01 : Disk image of Kevin's home PC (distributed into **homepc-part.E01** through **.E26**).
-  homepc.pcap : Network capture from the home PC during the infection.
-  Splunk logs : Windows Security + Sysmon telemetry from the corporate domain (**DC01**, **FS-01**, **WS-KBRIGGS**).
## HERRAMIENTAS
- PECmd, 

---
## Act 1 - Initial Access
1. A removable USB mass-storage device was the entry point onto the home PC. What friendly name did Windows record for the mounted volume of this device?
	- El primer acto se va a centrar en el ataque cuando el usuario estaba en su casa por lo que vamos a tomar de referencia todas las partes de homepc.E01-homepc.E26.
	- Metemos todas las partes en ftk imager para visualizar todo en uno y extraemos el hive SYSTEM para después irnos a  HKLM\SYSTEM\ControlSet001\Enum\SWD\WPDBUSENUM y ver las etiquetas de los drivers
![](assets/img/posts/Pasted%20image%2020260716203730.png)

2. What is the unique serial number that Windows recorded for that USB mass-storage device?
	- Nos dirigimos a HKLM\SYSTEM\ControlSet001\Enum\USBSTOR para ver el serial number del USB.
![](assets/img/posts/Pasted%20image%2020260716204315.png)

3. Provide the exact UTC date and time the USB device was first connected to the host.
	- En USBSTOR aparece también este dato
![](assets/img/posts/Pasted%20image%2020260716204652.png)

## Act 1 - Execution

1. The user double-clicked a file on the USB that masqueraded as a spreadsheet and triggered the infection. What is the full path of that file (including drive letter) as recorded in the execution artifact?
	- Necesitamos el UserAssist ya que registra todas las interacciones realizadas a través de la interfaz gráfica. También vemos que se ejecutó el archivo malicioso 13 segundos después de introducir el USB.
![](assets/img/posts/Pasted%20image%2020260721183222.png)

1. The shortcut launched a loader that immediately copied itself into the user's %Temp% directory under a randomized name and ran from there. What is the filename of that copied executable?
	- Ya que no está activado Sysmon (no podemos buscar por Event ID 11) he optado por ir directamente a $UsnJrnl para ver la creación de archivos justo después de las 17:45:40 (hora de la ejecución del archivo malicioso).
	- En la imagen podemos ver que justo en ese momento se ejecutó CMD y CONHOST (la interfaz gráfica de cmd). Además, vemos que un archivo con nombre sospechoso se creó.
![](assets/img/posts/Pasted%20image%2020260721201624.png)

	- Para comprobar que se ejecutó desde Temp, analizamos su Prefetch con PECmd para reafirmarnos en nuestra hipótesis.
![](assets/img/posts/Pasted%20image%2020260721201833.png)

## Act 1 - Payload Analysis

1. The loader decrypted and dropped the resident RAT payload to disk as a hidden file. What is its full path (including filename and extension)?
	- En el mismo Prefetch de LPNPYDZ podemos ver un archivo .LFU bastante sospechoso.
![](assets/img/posts/Pasted%20image%2020260721204345.png)

	- También podemos ver en $UsnJrnl que se crea la carpeta gpbot y el arhivo al segundo después de la creación del loader
![](assets/img/posts/Pasted%20image%2020260721204449.png)

	- Analizando el archivo en un Hex Editor, vemos que es un ejecutable ya que empieza por MZ:
![](assets/img/posts/Pasted%20image%2020260722193725.png)
 
1. Carve that dropped payload from the image and compute its SHA-256 hash. WARNING: the file is a live Gh0st/Farfli DLL - export and handle it only inside an isolated analysis VM.
- Con Get-FileHash sacamos el hash del payload malicioso
![](assets/img/posts/Pasted%20image%2020260722190151.png)

1. rundll32.exe loads the payload DLL and calls a single named export. What is the name of that exported function?
	- He de decir que el análisis de malware es lo más flojo que llevo ya que no he porgramado mucho a bajo nivel por lo que me pierdo un poco con todas estas interfaces pero sí sabía que con CFF Explorer se pueden visualizar binarios internamente.
	- Nos dirigimos a Export Directory y nos aparece la función exportada
![](assets/img/posts/Pasted%20image%2020260723135424.png)

	- Nos aseguramos de que esta función es usada por loaders maliciosos haciendo una búsqeuda rápida por Internet
![](assets/img/posts/Pasted%20image%2020260723135609.png)

1. What is the value NAME the malware created for autostart persistence under the user's Run key?
	- Nos vamos a NTUSER.dat\SOFTWARE\Microsoft\Windows\CurrentVersion\Run y vemos la que apunta al dropper malicioso junto con la función exportada anteriormente.
![](assets/img/posts/Pasted%20image%2020260723180129.png)

2. What is the PE compilation timestamp (TimeDateStamp) of the dropped payload DLL, in UTC?
	- Teniendo el hash, VirusTotal nos hace el resto del trabajo.
![](assets/img/posts/Pasted%20image%2020260723181729.png)

3. Standard compiler/MFC builds do not produce it, but the payload DLL's PE carries one extra, non-standard section that is a hallmark of this RAT family. What is the name of that section?
	- En CFF Explorer nos dirigimos a la parte Section Headers para ver si nos descuadra algo y efectivamente, sk no se ajusta al formato de esta parte.
![](assets/img/posts/Pasted%20image%2020260723182118.png)

## Act 1 - Command and Control

1. What is the primary hardcoded HTTP beacon C2 the RAT used (IP address and port)?
	- Nos vamos a Wireshark para inspeccionar la trama. Filtramos por HTTP y nos ajustamos a la hora del incidente. Vemos un GET hacia la ip 107.163.56.250/main.php. Bastante sospechoso considerando que ocurre a las 17:46:46. Para asegurarnos de que es esta IP la usada, nos vamos a VirustTotal y vemos que es una de las IPs que el loader contacta, junto con el puerto correspondiente.

![](assets/img/posts/Pasted%20image%2020260723190544.png)
![](assets/img/posts/Pasted%20image%2020260723192716.png)

1. What is the full User-Agent string sent by that primary /main.php beacon channel?
	- En el paquete de main.php aparece.

![](assets/img/posts/Pasted%20image%2020260723193805.png)

1. The RAT used a separate HTTP channel specifically to exfiltrate captured keystrokes. What is the endpoint of that keylog channel (IP address and port)?
	- Dos minutos después de establecer contacto con la IP anterior, aparece esta IP muy parecida a la anterior y que me hace sospechar por el parámetro no estándar.
![](assets/img/posts/Pasted%20image%2020260723200552.png)

  
## Act 1 - Credential Theft

1. The keylogger captures credentials typed into RDP sessions and exfiltrates them to its keylog C2 as a single HTTP GET parameter. What is the full encoded value it exfiltrated from this host?
	- Seguimos en Wireshark filtrando por HTTP. La respuesta está en el mismo paquete de la pregunta anterior.
![](assets/img/posts/Pasted%20image%2020260723202603.png)

2. That value is obfuscated, not plaintext - a straight Base64 decode yields garbage. Using the payload DLL you carved from the disk image, locate the routine that builds this exfil parameter, recover the encoding it applies, and use it to decode the captured value. What is the full decoded keylog record?
	- La verdad que lo he hecho a fuerza bruta con Claude ya que el método correcto descrito en el Discord de Cyberdefenders consistía en extraes la DLL que has encontrado en las preguntas anteriores -> la descomprimes (la ejecutas y obtienes un volcado de memoria con x64dbg o pe-sieve) -> buscas las cadenas que contienen el parámetro deseado y, a continuación, realizas ingeniería inversa de su subrutina para determinar el esquema de codificación/decodificación. (este paso lo supe después de hacerlo a fuerza bruta aunque no se me hubiese ocurrido)