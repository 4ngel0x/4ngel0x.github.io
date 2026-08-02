---
title: Review Certificación CCDL2 (Certified CyberDefenders)
date: 2026-07-23 11:00:00 +0100
categories:
  - Review
tags:
  - Review
description: Experiencia con la certificación y plataforma CCD
image:
  path: assets/img/posts/diseño2.png
---
## Resumen rápido de la CCDL2

| Aspecto                 | Valoración |
| ----------------------- | ---------- |
| Precio                  | 7/10       |
| Calidad del contenido   | 10/10      |
| Dificultad              | 8/10       |
| Realismo                | 8/10       |
| Relación calidad-precio | 10/10      |

La recomiendo 100% si tienes una base de sistemas y redes, y notas que ya sabes algo de ciberseguridad defensiva pero quieres dar el paso a un nivel intermedio. Si vienes de cero, probablemente te vas a agobiar; para eso está más pensada la CCDL1 como paso previo.

## 1. Mi punto de partida y contexto

Antes de entrar en materia creo que es importante situar desde dónde escribo esto, porque no es lo mismo valorar una certificación intermedia viniendo de cero que viniendo con algo de bagaje.

Yo vengo de estudiar Administración de Sistemas, hice una especialización en Ciberseguridad y llevo un año de experiencia en soporte IT. Además, antes había pasado la BTL1 (100/100) y después la CCD (80/100), así que me presenté con cierto nivel y base.

Te voy a dar mi opinión de lo que aprendes aquí llevado a la práctica con otros laboratorios. No puedo comentar si es útil o no para el trabajo ya que no he tenido la oportunidad de verlo pero cuando lo haga, lo editaré y lo comentaré desde el punto de vista laboral.

Aclaro también los tiempos: la certificación la saqué a finales de 2025, aunque esta review la estoy escribiendo y publicando a mediados de 2026. Lo digo porque CyberDefenders ha hecho cambios desde entonces, así que parte de lo que cuento aquí refleja mi experiencia de aquel momento sobre el examen y no necesariamente el estado actual al 100% (ya que me parece que ahora está prohibido usar la IA para los exámenes y creo que querían hacer la certificación supervisada, no sé de qué manera exactamente) aunque la opinión sobre la plataforma en general sí es de ahora ya que la uso todos los días.

La CCD que yo hice, ahora se ha dividido en dos certificaciones separadas, la CCDL1 y la CCDL2. Yo no he hecho la CCDL1 (sería la nueva de nivel más de entrada), y la que hice yo es lo que ahora se llama CCDL2, aunque en su momento, cuando la hice, para mí era simplemente "la CCD normal". Lo aclaro para que no haya confusión con el nombre a lo largo de la review.

## 2. ¿Qué es la CCD?

Certified CyberDefender (CCDL2) es la certificación más difícil de la plataforma [**CyberDefenders**](https://cyberdefenders.org/), una plataforma centrada en Blue Team: SOC, threat hunting, forense digital y respuesta ante incidentes. A diferencia de otras certificaciones de entrada, la CCD ya está pensada como un salto de nivel: asume que traes una base y no te trata como si fuera tu primer contacto con seguridad defensiva.

## 3. El salto de nivel respecto a BTL1

No va a ser justa la comparación ya que debería compararse más bien con la CDSA de Hack The Box pero es la otra certificación de ciberseguridad defensiva que tengo así que lo usaré como punto de referencia. 
La diferencia se nota casi de inmediato. Todo se siente más completo, incorpora herramientas realmente útiles y profundiza mucho más en términos técnicos. No es una certificación que te vaya explicando lo básico de la mano, da por hecho cierto conocimiento previo aunque si no traes experiencia, puedes ir más o menos enterándote, aunque no lo recomiendo de entrada.

## 4. Contenido: teoría y laboratorios

La teoría de la CCDL2 es completa y robusta, y eso se nota desde los primeros módulos: no te sueltan cuatro conceptos sueltos para que luego el lab haga el trabajo por ti, sino que te explican el contexto, el porqué de cada técnica y cómo encaja dentro de una investigación real, no solo el paso concreto que necesitas para resolver el ejercicio de turno.

 Con la teoría bien asentada, cuando te enfrentas a un caso distinto al que has practicado (por ejemplo en laboratorios de otras plataformas ), sabes por dónde tirar, qué preguntarte primero y qué herramienta encaja mejor. Y esa es, para mí, la diferencia real entre haber aprobado un examen y haber aprendido de verdad.

Los laboratorios me sorprendieron para bien. Están bien diseñados, son realistas y encajan con lo que luego se te va a pedir. Y hay un detalle muy chulo en cada sección: hay un laboratorio de investigación bastante más exigente que el resto, no tiene write-ups ni ayuda de ningún tipo, y si lo completas te suma un 5% extra si te quedas a las puertas de aprobar el examen (por ejemplo 65/100)

Un punto favorable que hicieron fue el de activar de nuevo el copiar y pegar. Al principio estaba capado y te gustaba, porque te obligaba a hacer los apuntes de forma más manual y sentías que iban más acorde con lo que realmente querías tener. Pero cuando llevas ya bastantes días de estudio, se hace cansino. Además, hoy en día la IA te puede ayudar muchísimo a mejorar los apuntes y hacerlos más interactivos (NotebookLM, por ejemplo), y con esta restricción no podías aprovechar eso.

## 5. El examen

Vamos con la chicha, que es lo que de verdad interesa. El examen de la CCDL2 es 100% práctico. Nada de test con preguntas cerradas: te meten en un escenario de intrusión real, tienes que investigar qué ha pasado, ir analizando los artefactos del ataque y responder preguntas basadas en lo que vas encontrando por el camino. No hay que entregar informe, todo son preguntas de tipo texto sobre la propia investigación donde puedes dar una pequeña explicación de cómo has sacado la respuesta.

Tienes 48 horas para completarlo, dentro de un entorno dedicado al que accedes desde el navegador (con una instancia de Elastic incluida), así que no hay que montar VPN ni instalar herramientas externas. Para aprobar necesitas un 70%, y el curso viene con dos intentos de examen incluidos.

El examen se apoya en cuatro módulos "core" del curso: forense de red, forense de disco, forense de memoria y threat hunting. Hay otros cuatro módulos opcionales (fundamentos de SecOps, respuesta a incidentes, seguridad de email y análisis de malware) que están bien para completar el perfil, pero no entran en el examen, así que no hace falta agobiarse con ellos antes de presentarte. 
Una cosa que me gustó del examen es que cada parte corresponde al mismo caso, es decir, verás los mismos usuarios y artefactos pero a la vez están separados en los distintos módulos para que, si te has quedado atrancado en el módulo de forense de red, por ejemplo, puedas seguir con el resto de módulos sin ningún problema.

El examen es open-book, puedes usar tus apuntes, cheatsheets y buscar en internet con total normalidad mientras investigas, eso forma parte del trabajo real. Lo único que está prohibido ahora (y que no lo estaba en el momento que me la saqué yo) es usar IA de cualquier tipo. Si te pillan te pueden descalificar.

Y hablando de apuntes, haz apuntes de cada página, apunta comandos, apunta la ubicación de las Registry Keys y Events IDs. En resumen, apunta todo porque lo vas a necesitar.

El examen es completo, exigente y largo, pero también muy gratificante. Cuando terminas tienes la sensación de que no solo has hecho un examen, sino que realmente has aprendido algo y sales con bastante más conocimiento del que tenías antes de empezar.

En mi caso, el primer día estuve en torno a 10 horas y el segundo día unas 7/8.
Los resultados del examen te los envían por correo entre una semana y semana y media después.

## 6. Soporte y organización del curso

- **Navegación y organización**: la plataforma es fácil de usar y todo (lecciones, labs, recursos) está estructurado de forma lógica.
- **Arranque de los laboratorios**: es muy directo, las instrucciones son claras y el entorno está listo para usarse desde el primer momento, así que te centras en aprender y no en pelearte con la configuración.
- **Soporte**: rápido y útil de verdad. Cada vez que tuve una duda o un problema, obtuve una respuesta clara en un tiempo razonable. Se me agotó el tiempo de acceso a los labs y, aun así, tuvieron el detalle de darme unos días más (ya había aprobado el examen para entonces).

## 7. Lo que no me ha convencido tanto

- Una vez terminas el examen no puedes ver en qué fallaste. Entiendo el motivo, evitar que las respuestas se compartan entre estudiantes, pero aun así te quedas con la duda de qué se te escapó.
- Algunos vídeos no aportan gran cosa a la explicación y se sienten más como una voz en off repitiendo teoría que ya has estudiado por tu cuenta.
- Me habría gustado ver un módulo de Linux, aunque fuese opcional. No es imprescindible viendo todo lo que ya ofrece el curso, pero habría sido un buen extra.

## 8. Precio y conclusión

Cuando yo me la saqué costaba 800$, y ahora ronda los 1.199. Es una subida que se nota, no nos engañemos, pero que va orientada a todo el contenido nuevo que viene. Actualmente han agregado una nueva sección de Active Directory y Kerberos muy completa e irán agregando nuevos módulos con el paso del tiempo así que la subida de precio está justificada aunque siga siendo alta, una cosa no quita la otra.

Y dicho esto, lo que sí puedo decir con seguridad es que después de acabar los ocho dominios sales con una base muy superior a la que tenías al empezar. Notas el cambio en cómo te enfrentas a un caso, en qué preguntas te haces primero y en qué herramientas coges de forma casi automática. 

Así que, precio aparte, para mí ha sido una inversión que ha merecido la pena. Si dudas por el coste, mi consejo es que valores primero tu nivel de partida (con la CCDL1 como opción más asequible) antes de descartarla solo por el número.
![](assets/img/posts/Pasted%20image%2020260724201317.png)

![](assets/img/posts/Pasted%20image%2020260724201345.png)

---

## EXTRA. La plataforma CyberDefenders

Como extra, voy terminar hablando de la plataforma CyberDefenders en general, centrándome tanto en los laboratorios premium de CyberRange como en su blog:

- Los laboratorios premium son muy buenos, y van añadiendo uno nuevo cada 3-4 semanas basados en ataques reales ocurridos recientemente, lo cual está genial para no quedarte con contenido desactualizado. Cuestan 20$ al mes, con opción a pagar 200$ al año si lo tienes claro. Aparte de los premium también están los laboratorios gratuitos, que están muy bien para entrar en contacto con el tipo de herramientas que se usan en el día a día. Pero si te lo puedes permitir, yo recomiendo pagar los premium. Hay muchos más y profundizan bastante más en cada tema. En cuanto a contenido, los labs cubren forense digital, análisis de malware, threat intelligence y threat hunting, con distintos niveles de dificultad. Y luego hay una distinción interesante entre dos tipos: los **retirados**, que vienen con su write-up y con pistas para guiarte si te atascas, y los **activos**, que no tienen nada de eso y son los que te sirven para subir puestos en el leaderboard.
![](assets/img/posts/Pasted%20image%2020260724201223.png)

- **El blog** funciona como un recurso aparte, con artículos y guías sobre detección, forense, threat hunting y herramientas de SOC (EDR, análisis de memoria, PowerShell ofuscado, etc.). Mucho contenido del propio certificado te lo encuentras ahí (aunque obviamente no se puede comparar). Es bastante útil.
![](assets/img/posts/Pasted%20image%2020260724212525.png)

---



