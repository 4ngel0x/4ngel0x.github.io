---
title: Review Certificación CCDL2 (Certified CyberDefenders)
date: 2026-07-23 11:00:00 +0100
categories:
  - Review
tags:
  - Review
description: Experiencia con la certificación y plataforma CCD
image:
  path: assets/img/posts/sdds.png
  width: 300
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

Aclaro también los tiempos: la certificación la saqué a finales de 2025, aunque esta review la estoy escribiendo y publicando a mediados de 2026. Lo digo porque, como veréis más adelante, CyberDefenders ha hecho cambios (incluido un rebranding de la propia certificación) desde entonces, así que parte de lo que cuento aquí refleja mi experiencia de aquel momento sobre el examen y no necesariamente el estado actual al 100% (ya que me parece que ahora está prohibido usar la IA para los exámenes y creo que querían hacer la certificación supervisada, no sé de qué manera exactamente) aunque la opinión sobre la plataforma en general sí es de ahora ya que la uso todos los días.

Sobre ese rebranding: la CCD que yo hice ahora se ha dividido en dos certificaciones separadas, la CCDL1 y la CCDL2. Yo no he hecho la CCDL1 (sería la nueva de nivel más de entrada), y la que hice yo es lo que ahora se llama CCDL2, aunque en su momento, cuando la hice, para mí era simplemente "la CCD normal". Lo aclaro para que no haya confusión con el nombre a lo largo de la review.

## 2. ¿Qué es la CCD?

Certified CyberDefender (CCD) es la certificación de la plataforma[**CyberDefenders**](https://cyberdefenders.org/), una plataforma centrada en Blue Team: SOC, threat hunting, forense digital y respuesta ante incidentes. A diferencia de otras certificaciones de entrada, la CCD ya está pensada como un salto de nivel: asume que traes una base y no te trata como si fuera tu primer contacto con seguridad defensiva.

## 3. El salto de nivel respecto a BTL1

Voy a compararlo con el BTL1 porque es la otra certificación de ciberseguridad defsniva que tengo aunque el CCD debería compararse más bien con el CDSA de Hack The Box.
La diferencia se nota casi de inmediato. Todo se siente más completo, incorpora herramientas realmente útiles y queda claro que están apuntando a un nivel intermedio. No es una certificación que te vaya explicando lo básico de la mano, da por hecho cierto conocimiento previo y construye sobre eso.

## 4. Los laboratorios

Aquí es donde más gratamente me sorprendieron. Los laboratorios están bien diseñados, son realistas y encajan con lo que luego se te va a pedir. Y hay un detalle que me parece de lo mejor que he visto en una certificación: uno de los laboratorios de investigación es bastante más exigente que el resto, no tiene write-ups ni ayuda de ningún tipo, y si lo completas te suma un 5% extra sobre la nota del examen. Es un incentivo muy bien pensado para empujarte a ir más allá de lo mínimo.

Vale, aquí tienes el texto para que lo pegues tú:

Un punto favorable que hicieron fue el de desactivar el copiar y pegar. Al principio está capado y te gusta, porque te obliga a hacer los apuntes de forma más manual y sientes que van más acorde con lo que realmente quieres tener. Pero cuando llevas ya bastantes días de estudio, se hace cansino. Además, hoy en día la IA te puede ayudar muchísimo a mejorar los apuntes y hacerlos más interactivos (NotebookLM, por ejemplo), y con esta restricción simplemente no puedes aprovechar eso.

## 5. El examen

Vamos con la chicha, que es lo que de verdad interesa. El examen de la CCDL2 es 100% práctico. Nada de test con preguntas cerradas: te meten en un escenario de intrusión real, tienes que investigar qué ha pasado, ir analizando los artefactos del ataque y responder preguntas basadas en lo que vas encontrando por el camino. No hay que entregar informe, todo son preguntas de tipo texto sobre la propia investigación.

Tienes 48 horas para completarlo, dentro de un entorno dedicado al que accedes desde el navegador (con una instancia de Elastic incluida), así que no hay que montar VPN ni instalar herramientas externas. Para aprobar necesitas un 70%, y el curso viene con dos intentos de examen incluidos.

El examen se apoya en cinco módulos "core" del curso: forense de red, recolección de evidencias, forense de disco, forense de memoria y threat hunting. Hay otros cuatro módulos opcionales (fundamentos de SecOps, respuesta a incidentes, seguridad de email y análisis de malware) que están bien para completar el perfil, pero no entran en el examen, así que no hace falta agobiarse con ellos antes de presentarte.

Un dato que me pareció muy honesto por su parte: el examen es open-book, puedes usar tus apuntes, cheatsheets y buscar en internet con total normalidad mientras investigas, eso forma parte del trabajo real. Lo único que está terminantemente prohibido es usar IA (ChatGPT, Copilot o similares); si te pillan, es motivo de descalificación. Y aquí enlaza justo con lo que comentaba antes de la restricción de copiar y pegar durante el curso: al final tiene sentido, porque te están entrenando para depender de tus propios apuntes y tu propio criterio, no de que una IA razone por ti el día del examen.

Y hablando de apuntes: si algo saqué en claro es que tus notas de los labs son tu mejor arma para el examen. No es una frase motivacional random, lo dicen ellos mismos con datos: los estudiantes que completan las cuatro investigaciones (disco, red, threat hunting y memoria) mejoran un 30% su rendimiento en el examen. Además, si completas las cuatro te llevas un 5% extra sobre la nota, que puede ser justo lo que te haga pasar de suspenso a aprobado si te quedas rozando el corte.

El examen es completo, exigente y largo, pero también muy gratificante. Cuando terminas tienes la sensación de que no solo has hecho un examen, sino que realmente has aprendido algo y sales con bastante más conocimiento del que tenías antes de empezar.

En mi caso, el primer día estuve en torno a 10 horas y el segundo día unas 7/8.
Los resultados del examen te los envían por correo a la semana/semana y media


## 6. Más allá del contenido técnico

Hay varias cosas que no tienen que ver directamente con lo técnico pero que hacen que el curso se disfrute más y se le saque más partido:

- **Navegación y organización**: la plataforma es fácil de usar y todo (lecciones, labs, recursos) está estructurado de forma lógica.
- **Arranque de los laboratorios**: es muy directo, las instrucciones son claras y el entorno está listo para usarse desde el primer momento, así que te centras en aprender y no en pelearte con la configuración.
- **Soporte**: rápido y útil de verdad. Cada vez que tuve una duda o un problema, obtuve una respuesta clara en un tiempo razonable. Se me agotó el tiempo de acceso a los labs y, aun así, tuvieron el detalle de darme unos días más (ya había aprobado el examen para entonces).


## 7. Lo que no me ha convencido tanto

No todo es perfecto, así que van también los puntos flojos:

- Una vez terminas el examen no puedes ver en qué fallaste. Entiendo el motivo, evitar que las respuestas se compartan entre estudiantes, pero aun así te quedas con la duda de qué se te escapó.
- Algunos vídeos no aportan gran cosa a la explicación y se sienten más como una voz en off repitiendo teoría que ya has estudiado por tu cuenta.
- Me habría gustado ver un módulo de Linux, aunque fuese opcional. No es imprescindible viendo todo lo que ya ofrece el curso, pero habría sido un buen extra.
- Cuando acabas con los 4 meses de acceso al curso, ya no puedes


## 8. Precio y conclusión

CyberDefenders me ha ayudado a confirmar, todavía más, que la ciberseguridad es el camino profesional que quiero seguir. Aunque a día de hoy no estoy trabajando en el sector, el curso me dio claridad y confianza sobre la dirección que quiero tomar.

Me ha dado experiencia práctica con herramientas y escenarios reales, lo que me ha ayudado a entender mejor qué implica trabajar en un entorno de Blue Team. Más allá del aprendizaje técnico, reforzó mi motivación y me dejó con la certeza de que este es el camino que quiero seguir.

Sigo usando la plataforma para practicar labs y reforzar lo aprendido. Y si algún día sacan una certificación más avanzada (¿CCD 2, tal vez?), seré de los primeros en apuntarme.
![](assets/img/posts/Pasted%20image%2020260724201317.png)

## EXTRA. La plataforma CyberDefenders

Para terminar voy a hablar ahora sobre el blog y los laboratorios de la plataforma en general, la cual  me ha servido para tener una muy buena base sobre Blue Team y con la que estoy a las puertas de entrar a trabajar como S0C N1:

- **El blog** funciona como un recurso aparte, con artículos y guías sobre detección, forense, threat hunting y herramientas de SOC (EDR, análisis de memoria, PowerShell ofuscado, etc.). Mucho contenido del propio certificado te lo encuentras ahí (aunque obviamente no se puede comparar). Es bastante útil.

- Los laboratorios premium son muy buenos, y van añadiendo uno nuevo cada 3-4 semanas basado en ataques reales ocurridos recientemente, lo cual está genial para no quedarte con contenido desactualizado. Cuestan 20$ al mes, con opción a pagar 200$ al año si lo tienes claro.

Aparte de los premium también están los laboratorios gratuitos, que están muy bien para entrar en contacto con el tipo de herramientas que se usan en el día a día. Pero si te lo puedes permitir, yo recomiendo pagar los premium: hay muchos más y profundizan bastante más en cada tema.

En cuanto a contenido, los labs cubren forense digital, análisis de malware, threat intelligence y threat hunting, con distintos niveles de dificultad. Y luego hay una distinción interesante entre dos tipos: los **retirados**, que vienen con su write-up y con pistas para guiarte si te atascas, y los **activos**, que no tienen nada de eso y son los que te sirven para subir puestos en el leaderboard.
![](assets/img/posts/Pasted%20image%2020260724201223.png)

![](assets/img/posts/Pasted%20image%2020260724201345.png)
