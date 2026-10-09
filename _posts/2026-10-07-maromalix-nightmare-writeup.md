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
	- 