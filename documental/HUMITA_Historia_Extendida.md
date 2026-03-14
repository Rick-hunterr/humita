# HUMITA — Documento de Diseño Extendido
### Versión 2.0 | Diseño Narrativo Completo

---

> *"Hay personas que salen todos los días y no ven nada. Hay personas que salen una sola vez y lo cambian todo."*

---

## ÍNDICE

1. [Visión General y Filosofía de Diseño](#1-visión-general-y-filosofía-de-diseño)
2. [Historia Central y Estructura Narrativa](#2-historia-central-y-estructura-narrativa)
3. [Personajes — Desarrollo Completo](#3-personajes--desarrollo-completo)
4. [El Mundo — Mapa y Zonas](#4-el-mundo--mapa-y-zonas)
5. [Capítulos — Narrativa Detallada](#5-capítulos--narrativa-detallada)
6. [Sistema de Diálogos Completo](#6-sistema-de-diálogos-completo)
7. [Sistema de Mecánicas](#7-sistema-de-mecánicas)
8. [Sistema de Objetos e Inventario](#8-sistema-de-objetos-e-inventario)
9. [Sistema de Clima y Tiempo](#9-sistema-de-clima-y-tiempo)
10. [Sistema de Memoria y Resonancia](#10-sistema-de-memoria-y-resonancia)
11. [Música y Diseño Sonoro](#11-música-y-diseño-sonoro)
12. [Finales y Variables Narrativas](#12-finales-y-variables-narrativas)
13. [Notas de Implementación en Godot](#13-notas-de-implementación-en-godot)

---

## 1. VISIÓN GENERAL Y FILOSOFÍA DE DISEÑO

### 1.1 Concepto Central

**HUMITA** es un juego narrativo de exploración emocional ambientado en un barrio de clase media argentina, en un día de verano que se convierte en tarde lluviosa. El jugador encarna a **Uma Solís**, una joven de 22 años que lleva meses sin salir de su departamento sin una razón concreta. No está enferma. No está triste de forma aguda. Simplemente encontró que el mundo exterior requiere más energía de la que siente tener.

El juego no trata de curarla. No hay "missión" en el sentido clásico. Lo que existe es un recorrido: una caminata de aproximadamente dos horas por un barrio que Uma conoce de memoria pero que hoy, por razones que el jugador ayuda a descubrir, se ve de otra manera.

Al final del recorrido, Uma encuentra a **Pablo**. Pero Pablo no es el destino: es la consecuencia de haber salido.

### 1.2 Filosofía de Diseño

**Lo que HUMITA ES:**
- Una experiencia de ritmo lento donde la atención al detalle es recompensada
- Un retrato de la introversión sin patologizarla
- Un juego donde cada NPC tiene historia propia, independiente de Uma
- Un sistema donde las decisiones pequeñas construyen capas de significado
- Una exploración del espacio como extensión del estado interno

**Lo que HUMITA NO ES:**
- Un juego con enemigos, peligros o mecánicas punitivas
- Una historia de "superar la ansiedad social"
- Un romance lineal con final obligatorio
- Una experiencia donde el jugador puede fracasar

### 1.3 Tono y Registro

El tono de HUMITA es el de una tarde argentina de verano que se nubla: familiar, cálido pero con algo de peso en el aire. Los diálogos son naturales, con el ritmo del habla rioplatense. Nada suena a guión. Los silencios valen tanto como las palabras.

**Referencias de tono:** *Cleo de 5 à 7* (Varda), *Paterson* (Jarmusch), *Florence* (videojuego), *Her Story* (videojuego).

### 1.4 Duración y Estructura

| Tipo de Partida | Duración Estimada |
|---|---|
| Recorrido mínimo (sin exploración) | 45–60 minutos |
| Partida estándar | 90–120 minutos |
| Exploración completa (todos los secretos) | 150–180 minutos |
| Segunda partida (con memoria de decisiones previas) | 60–90 minutos |

---

## 2. HISTORIA CENTRAL Y ESTRUCTURA NARRATIVA

### 2.1 La Historia de Uma — Fondo Completo

Uma Solís tiene 22 años. Estudió dos años de Letras en la universidad pública, después dejó. No con drama: simplemente un día no fue, y después otro día tampoco, y en algún momento se dio cuenta de que había dejado. Vive con su madre Elena, su hermano menor Tomás (11 años) y, intermitentemente, con su padre Héctor, que trabaja en otra provincia y vuelve los fines de semana cuando puede.

Hace siete meses conoció a Pablo en una clase a la que fue por error (buscaba el aula de inscripciones y entró a una clase optativa de historia de la música popular). Pablo estaba dando una exposición sobre la cumbia santafesina. Uma no sabía nada de cumbia santafesina. Se quedó igual.

Se cruzaron tres veces después de eso. Hablaron dos. En la segunda conversación, Pablo le preguntó si alguna vez había escuchado el disco *Adentro* de un músico llamado **Celso Bardi**. Uma dijo que no. Pablo dijo que tenía que escucharlo, que era de esos discos que suenan distinto según dónde estés cuando los escuchás.

Tres semanas después, Uma dejó de ir a la universidad. También dejó de contestar mensajes. No porque quisiera desaparecer: simplemente se fue achicando el radio de lo que sentía que podía manejar.

Pablo no sabe nada de esto. Él cree que Uma se mudó o que simplemente se fue diluyendo, como pasa. Tiene el número de Uma pero no lo usó porque Uma nunca le dio señales de que quisiera que lo usara.

**El día del juego** es un martes de febrero. Hace calor. Tomás tuvo un sueño raro y se lo contó a Uma antes de desayunar. Elena hizo mate y no preguntó nada. Héctor dejó una nota antes de irse al trabajo porque sabe que Uma lee las notas más que escucha las palabras.

Algo —el jugador decide qué— hace que Uma salga.

### 2.2 La Historia de Pablo — Fondo Completo

Pablo Ferreyra tiene 24 años. Terminó la carrera de Comunicación Social hace un año y trabaja part-time en una radio comunitaria del barrio de Palermo, aunque vive en Villa Crespo. Es del interior —de Resistencia, Chaco— y llegó a Buenos Aires a los 18. La distancia le enseñó a estar cómodo con la soledad pero también a valorar los encuentros que se sienten específicos.

Con Uma tuvo dos conversaciones que se quedaron. La primera fue casual —la saludó porque la vio perdida—. La segunda duró cuarenta minutos y tocó más temas de los que suelen tocarse en cuarenta minutos.

Después de eso, Pablo siguió su vida. Pero el disco de Celso Bardi que le mencionó a Uma lo dejó cerca, como los objetos que uno guarda sin saber bien por qué. Un día, caminando por el barrio donde vive Uma (pasó por ahí de casualidad, visitaba a un amigo), encontró una tapa de disco rota en la vereda. La reconoció: era la tapa de *Adentro*, de Celso Bardi. La dejó ahí, en la esquina, porque le pareció que tenía que quedarse donde estaba. Pero la miró un buen rato antes de seguir.

Lo que Pablo no sabe es que ese barrio es el de Uma.

### 2.3 La Historia de Bardi — El Hilo Invisible

**Celso Bardi** fue un músico de folklore experimental que grabó dos discos en los años ochenta y después desapareció del circuito. No murió: simplemente dejó de grabar. Vivió en ese barrio durante años. Su disco *Adentro* es casi imposible de conseguir. Hay copias en vinilo que circulan de mano en mano.

La canción que atraviesa el juego —que suena fragmentada en distintos lugares antes de escucharse completa— se llama **"La vuelta"**. Habla de salir y volver, pero no al mismo lugar.

Bardi es un personaje que nunca aparece directamente. Pero su presencia está en el barrio: en una placa pequeña en la fachada de un edificio, en el nombre de una calle lateral, en el recuerdo de varios NPCs que lo conocieron o lo cruzaron. Conocerlo —o no conocerlo— cambia la forma en que Uma entiende lo que encontró.

### 2.4 Estructura en Cinco Actos

```
PRÓLOGO      → La Casa           (exploración libre, detonante)
ACTO I       → El Barrio         (familiaridad, primeros encuentros)
ACTO II      → La Plaza          (apertura, cambio climático, punto de quiebre)
ACTO III     → La Zona Interior  (umbral, resonancia, objetos activados)
ACTO IV      → La Antesala       (decisión, silencio, la última pausa)
ACTO V       → El Espacio Final  (música completa, Pablo, cierre)
```

---

## 3. PERSONAJES — DESARROLLO COMPLETO

### 3.1 UMA SOLÍS — Protagonista

**Edad:** 22 años  
**Apariencia:** Tez morena, pelo oscuro y corto que le llega al cuello, ojos grandes con la costumbre de mirar antes de hablar. Usa ropa cómoda: pantalón liviano, remera de algún color que no destaque. El sombrero de ala ancha es siempre el mismo: paja natural con una cinta desgastada.

**Historia personal completa:**

Uma fue una estudiante brillante pero callada. Sus profesores siempre pusieron en sus boletines alguna variante de "participa poco pero cuando lo hace sorprende". Nunca le molestó ese comentario hasta que, en la secundaria, una compañera le dijo que sonaba a insulto disfrazado. Uma pensó en eso durante semanas.

A los 18, cuando entró a Letras, creyó que por fin estaba en el lugar correcto. La carrera prometía gente que también prefería leer a hablar. Pero descubrió que incluso ahí había una performance de la intelectualidad que la cansaba. Se fue achicando dentro de esa institución hasta que dejar de ir fue más fácil que seguir fingiendo que encajaba.

No tiene diagnóstico, ni lo busca. Sabe que hay días en que el mundo exterior le pesa más y días en que pesa menos. Aprendió a navegar eso. El problema es que los últimos meses fueron una racha larga de días pesados.

**Lo que Uma ama:** Los discos de vinilo aunque no tiene tornamesa, el mate a cualquier hora, los edificios con historia, los gatos que se quedan quietos, las conversaciones que empiezan de casualidad y terminan siendo importantes.

**Lo que Uma evita:** El sol directo a la cara, los ruidos sin fuente clara, los grupos de más de cuatro personas, las conversaciones donde tiene que explicar qué está haciendo con su vida.

**Arco de transformación:**

Uma no "sana" durante el juego. Lo que hace es recordar cómo es estar afuera: el peso diferente del aire, la textura de las calles, la sensación de ser una persona entre personas. Al encontrar a Pablo no resuelve nada, pero se da cuenta de que el mundo siguió existiendo mientras ella no lo miraba, y que eso, curiosamente, la alivia.

**Voz interna — Tono y registro:**

Los pensamientos de Uma (que el jugador lee como texto en pantalla) son irónicos, observadores, a veces autocríticos pero sin dramatismo. No se compadece de sí misma. Cuando algo la incomoda lo nombra y lo deja pasar. Cuando algo la conmueve, raramente lo dice en voz alta.

*Ejemplo de monólogo interno (sol fuerte):*
> "Si los humanos evolucionaron durante miles de años, ¿por qué nadie resolvió lo del sol en los ojos? Eso tendría que haber tenido prioridad."

*Ejemplo de monólogo interno (tras una conversación que le llega):*
> "Hay personas que dicen exactamente lo que necesitás escuchar sin saber que lo necesitabas. Me pregunto si él sabe que hace eso."

---

### 3.2 PABLO FERREYRA — Personaje Secundario Principal

**Edad:** 24 años  
**Apariencia:** Alto, flaco, pelo oscuro con rulos que no domó nunca. Usa anteojos de marco fino. Siempre tiene algo en la mano: un café, un libro, una bolsa de tela. Camina despacio, como si tuviera tiempo.

**Historia personal completa:**

Pablo llegó a Buenos Aires desde Resistencia con una mochila y la certeza de que iba a hacer radio. No sabe bien de dónde viene esa certeza, pero nunca la cuestionó. Estudió comunicación, trabajó en tres radios distintas antes de encontrar la comunitaria donde ahora está, y en el camino aprendió que la única habilidad que realmente importa en cualquier trabajo creativo es saber escuchar.

Con Uma habló dos veces. Ambas veces sintió que ella prestaba una atención diferente a las cosas que decía: no el entusiasmo performativo de quien quiere caer bien, sino algo más quieto. Cuando ella dejó de aparecer, Pablo lo procesó como una pérdida ambigua: alguien que podría haber sido algo pero que no fue.

No vino hoy al barrio buscando a Uma. Vino porque tiene que devolver unos discos a un amigo que vive a tres cuadras de la casa de ella. El encuentro no fue planeado por nadie.

**Desarrollo durante el juego:**

Pablo aparece fragmentado antes de aparecer completo. El jugador lo ve sin reconocerlo: un chico que pasa caminando rápido por el barrio al inicio, una silueta bajo la galería de la plaza durante la llovizna, una voz que llega desde algún lugar en la zona intermedia. Cuando finalmente aparece en el espacio final, el jugador tiene la sensación de que siempre estuvo ahí.

**Lo que Pablo sabe que Uma no sabe:**
- Que fue él quien dejó el fragmento de tapa de disco en la esquina del barrio de Uma (lo reconoció, lo dejó sin saber bien por qué)
- Que escuchó la melodía de "La vuelta" de Celso Bardi en la radio comunitaria hace tres semanas y no se le fue de la cabeza
- Que Uma le importa más de lo que le dijo

---

### 3.3 ELENA SOLÍS — La Madre

**Edad:** 48 años  
**Apariencia:** Parecida a Uma pero con el pelo más largo y algo más de cansancio en la cara. Se mueve con eficiencia por la cocina. Nunca está completamente quieta.

**Historia personal:**

Elena trabajó veinte años en una empresa de contabilidad. Hace dos años empezó un curso de cerámica del que no habla mucho pero que claramente la hace bien: hay piezas pequeñas y torcidas en los estantes que nadie preguntó quién hizo. Crio a dos hijos con ayuda intermitente de Héctor y desarrolló la habilidad de ver qué necesita cada persona sin preguntarlo directamente.

Con Uma tiene una relación de respeto silencioso. Sabe que su hija necesita espacio, y lo da. Pero también sabe cuándo decir algo pequeño en el momento justo.

**Función narrativa:**

Elena aparece solo en el prólogo, pero su presencia resuena a lo largo del juego. Varias frases de NPCs (sin saberlo) repiten variaciones de cosas que Elena dijo. La frase final del juego es de ella.

**Diálogo completo — Prólogo:**

*(Si Uma entra a la cocina, Elena está haciendo mate. Levanta la vista un segundo.)*

> **Elena:** "¿Querés mate?"
> 
> *(Uma puede decir sí o no. Si dice sí, hay una pausa mientras Elena lo prepara. Si dice no, Elena asiente y sigue.)*
>
> **Elena:** "Tomás tuvo un sueño raro. No quiso contármelo a mí. Dice que los sueños hay que contárselos a alguien específico."
>
> *(Pausa. Sigue con lo que estaba haciendo.)*
>
> **Elena:** "A veces salir un ratito cambia las cosas. No siempre. Pero a veces."
>
> *(No mira a Uma mientras dice esto. No lo dice para convencerla. Lo dice como se dice algo que es verdad.)*

---

### 3.4 HÉCTOR SOLÍS — El Padre

**Edad:** 52 años  
**Apariencia:** Uma tiene sus ojos. Eso es lo primero que se nota cuando aparece en la foto de la mesita de noche de Uma (es la única foto que tiene en el cuarto).

**Historia personal:**

Héctor es técnico en redes y trabaja en una empresa con sede en Córdoba. No se fue por elección sino por necesidad: era el trabajo que había, y lo tomó. Lo que empezó como provisional se fue volviendo permanente de la manera silenciosa en que las cosas se vuelven permanentes. Vuelve los fines de semana. A veces no puede.

Con Uma tiene una relación construida más sobre los objetos que sobre las palabras: le dejó su colección de discos cuando se fue, le manda links de música que cree que le puede gustar, le deja notas cuando no hay tiempo para hablar. Es un padre que sabe que su presencia tiene límites y trabaja dentro de esos límites.

**Función narrativa:**

Héctor no aparece en el juego. Solo su letra. Las dos notas que deja son el único rastro de su voz, y sin embargo son las palabras que más directamente hablan con Uma a lo largo del recorrido.

**Las dos notas completas:**

*Primera nota (sobre la mesa del comedor):*
> *"Si ves algo que te llama la atención, seguí por ahí. Eso es todo lo que sé sobre encontrar cosas que valen. Vuelvo el viernes. Te quiero."*
> *— Papá*

*Segunda nota (banco de la antesala):*
> *"A veces llegar no es lo difícil. Es animarse a seguir cuando ya llegaste. Eso lo aprendí tarde. Vos lo estás aprendiendo bien."*
> *— H*

*(Solo la inicial. Como si esta nota fuera de otro momento, o la misma de antes reescrita.)*

---

### 3.5 TOMÁS SOLÍS — El Hermano

**Edad:** 11 años  
**Apariencia:** Pelo revuelto, una remera con algún dinosaurio, descalzo adentro de la casa. Dibuja todo el tiempo con la concentración de alguien que está haciendo algo importante.

**Historia personal:**

Tomás es el hermano menor que creció viendo a Uma desde adentro. Para él, Uma simplemente es Uma: la que está en su cuarto con música, la que le explica cosas cuando le pregunta, la que a veces sale a buscar medialunas y vuelve con tres cosas más. No le parece raro que Uma pase mucho tiempo adentro porque siempre fue así. No lo preocupa porque no tiene el marco para preocuparse. Lo que tiene es el sueño que tuvo anoche.

**Función narrativa:**

Tomás tiene una percepción que no es sobrenatural pero que se siente cercana: dice cosas verdaderas que no sabe que son verdaderas. El dibujo que hizo es la misma forma que Uma va a encontrar al final. No porque sea mágico, sino porque los chicos de once años a veces hacen eso.

**Diálogo completo — Prólogo:**

*(Tomás está en el living, dibujando en el suelo con marcadores de colores. No levanta la vista cuando Uma entra.)*

> **Tomás:** *(sin mirar)* "¿Ya desayunaste?"
>
> *(Uma puede responder o no. Tomás continúa igual.)*
>
> **Tomás:** "Soñé que ibas a encontrar algo. No sé qué era, pero estabas contenta. Esa cara que hacés cuando algo te gusta pero no querés que se note."
>
> *(Pausa. Termina un trazo. Lo mira.)*
>
> **Tomás:** "¿Querés ver?"
>
> *(Si Uma se acerca, Tomás le muestra el dibujo: una forma abstracta. Puede ser una radio. Puede ser una flor. Puede ser un disco. Puede ser el sol visto desde adentro de un cuarto.)*
>
> **Tomás:** "No sé qué es. Pero sé que es eso."
>
> *(Vuelve a dibujar. Conversación terminada.)*
>
> *(Si Uma pregunta qué quiso decir, Tomás encoge los hombros:)*
> **Tomás:** "No sé explicarlo. Pero era eso."

---

### 3.6 DOÑA NÉLIDA — La del Balcón

**Edad:** 71 años  
**Apariencia:** Bata floreada, pelo gris recogido, manguera en la mano. Tiene doce plantas en el balcón del primer piso. Las conoce a todas por nombre.

**Historia personal:**

Nélida Varga nació en ese edificio. Se casó, tuvo hijos, los hijos se fueron, el marido murió hace nueve años. No se fue porque no quiso. Riega sus plantas a la mañana y a la tarde. Conoce a todos los vecinos del bloque y a la mitad de la cuadra. Vio a Uma crecer desde ese balcón.

**Arco dentro del juego:**

Nélida tiene tres niveles de diálogo según cuánto la explore el jugador. En el primero es amable y breve. En el segundo revela que conoció a Celso Bardi, el músico del disco. En el tercero dice algo sobre su marido que conecta inesperadamente con lo que Uma está viviendo.

**Diálogos completos:**

*Primer contacto (Uma pasa, Nélida la saluda desde el balcón):*
> **Nélida:** "¡Uma! ¡Qué bueno verte por acá! Hace rato que no bajabas."
>
> *(Uma puede saludar, parar o seguir.)*
>
> **Nélida:** "No, no te digo por nada. Es que te veía desde acá a veces, en la ventana. Pensaba: hoy no, mañana quizás. Y hoy saliste. Qué bien."
>
> *(Pausa. Riega una planta.)*
> **Nélida:** "¿A dónde vas?"
>
> **Uma:** *(opciones)*
> - "No sé todavía." → Nélida: "Mejor así."
> - "A dar una vuelta." → Nélida: "Qué lujo, dar una vuelta. Yo ya no puedo mucho. Pero vos sí."
> - *(silencio)* → Nélida: "No importa. Igual es bueno salir."

*Segundo contacto (si Uma vuelve a interactuar):*
> **Nélida:** "¿Vos sabés que en esa esquina vivió un músico? Celso Bardi. Tus papás lo habrán conocido de nombre, si son de la época."
>
> **Uma:** "¿Un músico?"
>
> **Nélida:** "Sí. Grabó un disco acá, en esa época que grababan en los departamentos porque no había plata para estudio. Se escuchaba la música desde la calle. Yo vivía acá de siempre, lo escuché mil veces. Una canción en particular. *(pausa)* Ya no me acuerdo la letra, pero la melodía la tengo acá todavía." *(se toca el pecho)*
>
> *(Uma puede preguntar el nombre de la canción:)*
> **Nélida:** "No me acuerdo. Pero si la escuchás, la reconocés. Esas canciones son así."

*Tercer contacto (si Uma exploró la casa del músico o tiene el disco):*
> **Nélida:** *(mirando el disco que Uma lleva)* "Ese es él. ¿De dónde lo sacaste?"
>
> *(Uma explica.)*
>
> **Nélida:** "Mi marido lo escuchaba. Cuando murió, yo estuve mucho tiempo sin poder escuchar música. Después un día puse un disco de él sin querer y me di cuenta de que ya podía. Que no dolía igual. *(pausa)* Esas cosas se mueven solas, ¿sabés? Vos no las empujás. Se mueven."

---

### 3.7 CHARLY — El Kiosquero

**Edad:** 38 años  
**Apariencia:** Remera de algún equipo de fútbol que ya no es el que está siguiendo ahora, bigote, la radio siempre prendida en el mostrador.

**Historia personal:**

Carlos "Charly" Irurzun tiene el kiosco desde que se lo compró a su suegro hace diez años. No era lo que tenía planeado: iba a ser arquitecto pero dejó la facultad en tercer año. El kiosco terminó siendo suyo de una manera que la arquitectura nunca fue.

Escucha radio todo el día. Conoce a casi todos los que pasan porque el kiosco está en una esquina estratégica. Sabe quiénes son los de la cuadra y quiénes no. Sabe que Uma es de las de la cuadra aunque hace meses que no la ve.

**Diálogos completos:**

*Primera interacción:*
> *(La radio suena. La melodía es la misma que Uma escuchó —o pudo escuchar— desde la cocina. Charly la tiene prendida, está mirando el teléfono.)*
>
> *(Si Uma se detiene a escuchar, Charly levanta la vista.)*
>
> **Charly:** "¿Qué le doy?"
>
> **Uma:** *(opciones)*
> - "Nada, estaba escuchando la radio." → Charly: "Ah, sí. Buena esa, ¿no? No sé ni qué es pero la pasan siempre los martes."
> - "Un agua." → *(mientras la busca)* "Hace calor para estar en la calle. ¿Es nueva en el barrio?"
> - *(silencio)* → Charly la mira un segundo y vuelve al teléfono.
>
> *(Llega un cliente. Charly apaga la radio de golpe para escuchar el pedido. La canción se corta a la mitad.)*
>
> *(Monólogo interno de Uma):*
> > *"Justo ahí."*

*Segunda interacción (si Uma vuelve cuando no hay clientes):*
> **Charly:** "¿Volvió?"
>
> **Uma:** "¿La canción?"
>
> **Charly:** "Se me fue el programa. *(busca en la radio)* No sé qué era. Algo viejo. Un tipo del barrio, me parece. ¿No era que había un músico acá antes?"
>
> **Uma:** *(puede confirmar o no saber)*
>
> **Charly:** "Me parece que era ese. Mi vieja lo conoció. Dice que era raro pero buena gente. *(pausa)* Lo de raro no lo digo en sentido malo."
>
> **Uma:** *(monólogo interno):* *"Yo tampoco lo tomaría en sentido malo."*

---

### 3.8 DON MARIO — El del Banco de la Vereda

**Edad:** 77 años  
**Apariencia:** Traje de verano, sombrero de paja (parecido al de Uma, más viejo), el diario doblado en la mano.

**Historia personal:**

Mario Castellanos fue contador durante cuarenta años. Se jubiló a los 65 y descubrió que no sabía qué hacer con los días. Aprendió a estar en el banco de la vereda como una ocupación en sí misma: mirar, leer, saludar. Dice que aprendió más del barrio en los últimos doce años que en los cuarenta anteriores.

Tiene la costumbre de decir cosas directas a las personas que recién conoce. No porque sea raro: sino porque a su edad le parece que lo indirecto cuesta demasiado tiempo.

**Diálogos completos:**

*Primera interacción:*
> *(Don Mario baja el diario cuando Uma pasa.)*
>
> **Don Mario:** "Buen día."
>
> *(Uma puede parar o no. Si para:)*
>
> **Don Mario:** "No la vi antes por acá. ¿Es del barrio?"
>
> **Uma:** "Vivo a dos cuadras."
>
> **Don Mario:** "Ah. *(asiente)* Los que viven cerca a veces tardan más en llegar que los que vienen de lejos. *(pausa)* Yo tardé veinte años en conocer bien esta cuadra. Vivía en el séptimo piso y no bajaba."
>
> **Uma:** *(opciones)*
> - "¿Y qué pasó?" → Don Mario: "Me cansé de las escaleras. Tuve que bajar."
> - "¿Y ahora?" → Don Mario: "Ahora tengo este banco y el diario. Y veo todo."
> - *(silencio)*
>
> **Don Mario:** "Usted va a encontrar lo que está buscando. No sé qué es. Pero lo va a encontrar." *(sube el diario)* "Buen día."

*Segunda interacción (si Uma regresa):*
> **Don Mario:** *(sin bajar el diario)* "¿Encontró algo?"
>
> **Uma:** "Todavía no."
>
> **Don Mario:** "Entonces siga buscando." *(pausa)* "¿Sabe cuál es el error? Creer que buscar y encontrar son dos momentos distintos. Son el mismo."

---

### 3.9 LOS CHICOS DE LAS CARTAS — Ramiro y Nico

**Edades:** 15 y 14 años  
**Apariencia:** Sentados en el escalón de entrada de una casa, con un mazo de cartas españolas entre los dos.

**Historia personal:**

Ramiro y Nico son vecinos y amigos desde primer grado. Pasan los veranos así: en el escalón, con las cartas, hablando de cosas que no importan. A Ramiro le gusta hacer como que las cartas significan cosas aunque no cree que signifiquen nada. A Nico le parece una estupidez pero igual pregunta.

**Diálogos completos:**

*Interacción única:*
> *(Uma pasa. Ramiro la mira.)*
>
> **Ramiro:** "Señora, ¿quiere que le tiremos las cartas?"
>
> **Uma:** *(opciones)*
> - "No soy señora." → Ramiro: "Perdón. ¿Quiere que le tiremos las cartas, *joven*?"
> - "No creo en eso." → Ramiro: "Yo tampoco. Pero es entretenido."
> - "¿Cuánto cuesta?" → Ramiro: "Nada. Estamos aburridos."
>
> *(Si Uma acepta o sigue la conversación, Ramiro agarra una carta al azar y se la muestra.)*
>
> **Ramiro:** "Esta es la carta del que encuentra cosas." *(es una carta cualquiera, sin significado especial)* "Usted la sacó a ella."
>
> **Uma:** *(monólogo interno):* *"No la saqué. Él la eligió. Pero bueno."*
>
> **Nico:** *(sin levantar la vista)* "Eso lo dice con todas."
>
> **Ramiro:** "Con vos no lo digo porque ya sabés lo que buscás y no lo tenés."
>
> *(Pausa incómoda. Uma puede irse o preguntar.)*
>
> **Uma:** "¿Qué está buscando él?"
>
> **Ramiro:** "Una chica." *(dicho sin dramatismo, como un hecho)*
>
> **Nico:** *(sigue sin mirar)* "Cállate, Rami."

---

### 3.10 LA LECTORA DE LA PLAZA — Valentina

**Edad:** 26 años  
**Apariencia:** Pelo largo recogido en un rodete descuidado, lentes de sol, un libro grueso con la tapa cubierta con una bolsa de nylon para que no se moje.

**Historia personal:**

Valentina Mora es traductora freelance. Trabaja desde su casa, pero tiene la regla de salir al menos dos horas por día "aunque no haya por qué". La plaza es su oficina exterior. Lee ahí aunque llueva levemente, porque aprendió que la lluvia no moja si uno no le presta atención.

No conoce a Uma. Pero dice cosas que Uma necesita escuchar porque son cosas que Valentina dice siempre, a cualquiera que se siente cerca.

**Diálogos completos:**

*Primera interacción:*
> *(Uma se acerca al banco bajo el árbol. Valentina levanta la vista.)*
>
> **Valentina:** "Hola. ¿Te molesto si me quedo? Hay lugar."
>
> **Uma:** *(opciones)*
> - "No, es tu banco." → Valentina: "Es el banco de todos. Pero gracias."
> - "Me quedo yo, si no te molesta." → Valentina: "Para nada. Para eso está."
> - *(se sienta sin decir nada)* → Valentina asiente y vuelve al libro.
>
> *(Después de un momento, sin levantar la vista del libro:)*
>
> **Valentina:** "Cuando llueve todo suena más lindo, ¿no te parece? No sé si es el agua o que los otros ruidos bajan."
>
> **Uma:** *(monólogo interno):* *"Tomás dijo algo parecido esta mañana. No igual. Pero parecido."*

*Segunda interacción (si Uma espera en el banco):*
> **Valentina:** *(cerrando el libro por un momento)* "¿Estás esperando algo?"
>
> **Uma:** "No sé."
>
> **Valentina:** "Eso está bien. *(pausa)* A veces esperar algo que no sabés qué es es más honesto que esperar algo que sí sabés."
>
> **Uma:** "¿Qué estás leyendo?"
>
> **Valentina:** "Una novela donde nada pasa. Pero pasa todo." *(muestra la tapa; no se ve bien el título)* "¿Leés?"
>
> **Uma:** "Leía. Dejé."
>
> **Valentina:** "Los libros esperan. Son buenos en eso."

*Tercer contacto (si Uma tiene el disco de Bardi):*
> **Valentina:** *(mira el disco que asoma del bolso de Uma)* "¿Eso es de Bardi?"
>
> **Uma:** "¿Lo conocés?"
>
> **Valentina:** "De nombre. Mi viejo lo escuchaba. ¿Dónde lo conseguiste?"
>
> **Uma:** *(explica)*
>
> **Valentina:** "En la plaza, ¿en serio? *(pausa)* Tiene sentido. Él vivía acá cerca. *(pausa más larga)* Hay objetos que vuelven a las personas que los necesitan. No lo digo en sentido místico. Lo digo en el sentido de que alguien lo dejó ahí para alguien, y ese alguien eras vos."

---

### 3.11 EL SEÑOR DE LAS PALOMAS — Ernesto

**Edad:** 64 años  
**Apariencia:** Campera de verano (siempre usa campera, no importa el calor), una bolsa de tela, los ojos puestos en el espacio donde estaban las palomas.

**Historia personal:**

Ernesto Giménez viene a la plaza todos los días a las once. Tiene palomas que reconoce individualmente —les puso nombres que no le dice a nadie porque sabe que suena raro—. Con la llovizna se fueron. Él se quedó porque no tiene apuro.

Perdió a su mujer hace tres años. No habla de eso. Pero está presente en la forma en que mira el espacio donde estaban las palomas.

**Diálogos completos:**

*Interacción (Uma se acerca):*
> *(Ernesto no la mira. Sigue mirando el espacio vacío.)*
>
> **Ernesto:** "Se fueron con la lluvia."
>
> *(Uma puede responder o no.)*
>
> **Ernesto:** "Pero van a volver. Siempre vuelven. *(pausa larga)* Uno aprende a esperar. No es difícil. Lo difícil es aprender que esperar no es lo mismo que quedarse quieto."
>
> **Uma:** *(monólogo interno):* *"No sé bien qué quiso decir. Pero tampoco sé bien qué quiso decir Tomás esta mañana y los dos me sonaron igual."*
>
> *(Si Uma pregunta por las palomas:)*
> **Ernesto:** "Las tengo anotadas. Cuántas son, qué color. El de la pata torcida lo llamo Gardel aunque mi hija dice que es una hembra. Le digo que no importa, que Gardel era un nombre para una actitud, no para un sexo."
>
> *(Uma puede reírse —monólogo interno: "Eso sí lo entendí"— o seguir.)*

---

### 3.12 LOS ADOLESCENTES DE LA GALERÍA — Grupo sin nombre

**Edades:** Entre 16 y 18 años. Son cuatro.  
**Apariencia:** Variados. Todos están bajo la galería porque empezó a llover. Ninguno tiene paraguas.

No tienen historia individual elaborada. Son la textura del mundo: personas que existen por razones que no tienen nada que ver con Uma. Uno tiene el teléfono con música. Si Uma se acerca lo suficiente, la melodía que suena es una versión rap/electrónica de "La vuelta" de Bardi —alguien la sampleó—. Los adolescentes no lo saben. No interactúan con Uma a menos que ella los salude.

*Si Uma saluda:*
> **Adolescente (cualquiera):** "Hola." *(vuelve a su conversación)*
>
> *Monólogo interno de Uma:* *"Perfecto."*

---

## 4. EL MUNDO — MAPA Y ZONAS

### 4.1 Geografía General

El juego transcurre en un barrio de Buenos Aires que no se nombra directamente pero que tiene características de **Villa Crespo o Chacarita**: casas bajas mezcladas con edificios de tres o cuatro pisos, arbolado antiguo, calles adoquinadas en algunas cuadras, comercios de barrio que llevan décadas en el mismo lugar.

El recorrido de Uma es aproximadamente **1,8 km en línea real**, aunque el jugador los experimenta como un espacio más extenso porque puede explorar cada cuadra con detalle.

### 4.2 Mapa General — Zonas Principales

```
╔══════════════════════════════════════════════════════════════╗
║                    MAPA DE HUMITA                            ║
╚══════════════════════════════════════════════════════════════╝

  [ZONA 0] LA CASA
  Depto 3B, Edificio de 4 pisos
  Habitación Uma | Living | Cocina | Patio interior
         │
         ▼
  [ZONA 1] EL BARRIO INMEDIATO  ←──── Exploración libre: 4 cuadras
  ┌─────────────────────────────────────────────────────────┐
  │  Cuadra 1: Edificio de Uma → Kiosco de Charly           │
  │  Cuadra 2: Casas bajas → Don Mario en el banco          │
  │  Cuadra 3: Tienda cerrada → Ramiro y Nico               │
  │  Cuadra 4: Esquina del disco roto → Casa de Bardi (★)  │
  └─────────────────────────────────────────────────────────┘
         │
         ▼
  [ZONA 2] LA PLAZA   ←─────────────── Espacio abierto central
  ┌─────────────────────────────────────────────────────────┐
  │  Perímetro: Valentina (banco-árbol) | Ernesto (palomas) │
  │  Centro: Fuente seca + disco de vinilo                  │
  │  Galería lateral: Adolescentes + música fragmentada     │
  │  Banco del alero: Momento de pausa + dibujo (opcional) │
  └─────────────────────────────────────────────────────────┘
         │
         ▼
  [ZONA 3] LA ZONA INTERIOR  ←─────── Umbral, espacio liminal
  ┌─────────────────────────────────────────────────────────┐
  │  Pasaje A: Objetos-espejo (banco, radio, planta)        │
  │  Puerta: Voz de Elena                                   │
  │  Sala de activadores: ranura disco / gancho / marco     │
  │  Pasillo final: Primer plano de Uma                     │
  └─────────────────────────────────────────────────────────┘
         │
         ▼
  [ZONA 4] LA ANTESALA  ←──────────── Patio semiabierto
  ┌─────────────────────────────────────────────────────────┐
  │  Banco: Segunda nota de Héctor                          │
  │  Farola apagada                                         │
  │  Puerta al espacio final                               │
  └─────────────────────────────────────────────────────────┘
         │
         ▼
  [ZONA 5] EL ESPACIO FINAL  ←──────── Climax + Encuentro
  ┌─────────────────────────────────────────────────────────┐
  │  Radio antigua + ranura para disco                      │
  │  Cortina de lluvia / abertura lateral                   │
  │  Pablo: Encuentro final                                 │
  └─────────────────────────────────────────────────────────┘

  (★) = Punto de interés opcional con contenido extra
```

---

### 4.3 ZONA 0 — La Casa

**Descripción detallada:**

El departamento 3B es un tres ambientes de techo alto, baldosas de los años cincuenta y una distribución que no fue pensada para cuatro personas pero que con los años se acomodó. Hay un patio interior pequeño al que dan la cocina y el cuarto de Tomás.

**Subespacios explorables:**

**Cuarto de Uma:**
- Estante con 23 discos (todos inspeccionables, cada uno tiene una descripción de Uma)
- El disco dado vuelta: *Adentro*, de Celso Bardi. Uma no sabe por qué está dado vuelta, no recuerda haberlo hecho
- Ventana con persiana a media asta — detonante 1 disponible (brillo en la vereda)
- Escritorio: cuaderno de bocetos abierto
- Cama sin hacer
- Foto de Héctor en la mesita de noche (inspeccionar activa monólogo)
- Silla con ropa encima (inspeccionar activa monólogo)

**Living:**
- Tomás dibujando — diálogo disponible
- Televisión apagada
- Estante con libros de Elena (inspección disponible)
- Mesa ratona con revistas viejas

**Cocina:**
- Elena haciendo mate — diálogo disponible
- Detonante 2 disponible: melodía desde la radio de la calle, audible si Uma está parada cerca de la ventana de la cocina
- Foto familiar imantada en la heladera (inspeccionar activa monólogo)
- Nota de Héctor sobre la mesa

**Patio interior:**
- Macetas de Elena (algunas con plantas, una con tierra vacía)
- Silla plástica
- Ropa colgada
- Detonante 3: en el patio, Uma puede abrir el cuaderno y ver la página misteriosa

---

### 4.4 ZONA 1 — El Barrio

**Descripción atmosférica:**

El barrio tiene esa calidad de los barrios conocidos: Uma lo recorre en piloto automático, pero hoy algo la hace detenerse. La cámara (perspectiva lateral con profundidad) muestra edificios en los que nunca se prestó atención: el relieve en la fachada del número 847, la puerta verde que siempre estuvo verde y que alguien debió elegir verde alguna vez.

**Cuadra 1 — El inicio:**
- Salida del edificio: Nélida en el balcón
- Kiosco de Charly a mitad de cuadra
- Árbol grande con raíces que rompen la vereda (inspección disponible)
- Buzón de correo viejo que ya no funciona (inspección disponible)

**Cuadra 2 — La cuadra tranquila:**
- Don Mario en el banco de la vereda
- Casa con persianas metálicas cerradas que tiene stickers políticos de tres épocas distintas
- Un gato en un umbral (interacción disponible: Uma puede detenerse y el gato la ignora)
- Una heladería que abre a las 14:00 (está cerrada, tiene el menú en la vidriera)

**Cuadra 3 — La cuadra de los eventos:**
- Tienda cerrada: foto polaroid disponible
- Ramiro y Nico con las cartas
- Árbol que da suficiente sombra para que Uma pueda quedarse parada un momento
- Una radio que suena desde una ventana en el primer piso (es la misma melodía, fragmentaria)

**Cuadra 4 — La esquina de Bardi:**
- Fragmento de tapa de disco en la vereda (objeto clave)
- Edificio donde vivió Bardi: placa pequeña en la fachada (inspección revela historia del músico)
- Un vecino en la entrada que no sabe nada de la placa aunque vive ahí hace quince años
- Esquina donde doblar hacia la plaza

**Objeto secreto de la zona:** En la entrada del edificio de Bardi hay un timbre con el apellido "BARDI" que sigue en la botonera aunque nadie de ese apellido vive hace décadas. Si Uma lo toca, no responde nadie. Monólogo interno: *"Alguien tendría que actualizar eso. O quizás es mejor así."*

---

### 4.5 ZONA 2 — La Plaza

**Descripción atmosférica:**

La plaza tiene nombre en el cartel de la entrada —"Plaza Domingo Faustino Sarmiento"— pero nadie la llama así. La llaman "la plaza" o "la del barrio". Tiene dos tipos de árboles: los viejos y grandes de antes, y los nuevos plantados hace cinco años que todavía no dan sombra suficiente.

**Áreas específicas:**

**El perímetro (zona segura para Uma):**
- Banco-árbol donde está Valentina
- Banco lateral donde Ernesto mira el espacio de las palomas
- Un mástil sin bandera
- Un cartel que anuncia un festival de teatro que ya pasó

**El centro:**
- Fuente seca de mármol con grafiti en la base
- El disco de vinilo apoyado en el borde (objeto clave)
- Si el jugador manda a Uma al centro en pleno sol: monólogo interno *"Esto es lo que se siente estar expuesta."* + cambio visual

**La galería lateral (zona cubierta):**
- Los adolescentes con el teléfono
- Un puesto de diarios cerrado
- Una máquina de café que no funciona desde 2019

**El banco del alero:**
- Zona donde Uma puede sentarse y activar el momento de pausa
- Si el jugador espera 3 minutos reales sin input: Uma saca el cuaderno y dibuja
- El dibujo aparece en el inventario del cuaderno

**Punto de interés secreto:** En la base de uno de los árboles viejos, tallado en la corteza, hay iniciales y una fecha de los años ochenta. No dicen "C.B." ni nada tan obvio. Pero si Uma las inspecciona, el monólogo interno dice: *"Alguien quiso dejar algo permanente en algo que sigue creciendo. No sé si eso funciona o si con los años el árbol simplemente se lo come."*

---

### 4.6 ZONA 3 — La Zona Interior

**Descripción atmosférica:**

Esta zona es la más ambigua geográficamente. El pasaje que lleva a ella desde la plaza podría ser el acceso lateral de un edificio de departamentos, el corredor de un mercado cerrado, la entrada de un conventillo reconvertido. El jugador no necesita entender qué es. Lo que importa es que la escala cambia: los techos son más bajos, las paredes más cercanas, la luz más difusa.

**Los objetos-espejo:**

Esta zona tiene la particularidad de que cada objeto central que Uma encontró en el mundo exterior tiene aquí una versión ligeramente alterada:

| Objeto original | Versión interior | Diferencia |
|---|---|---|
| Banco de la plaza (exterior, húmedo) | Banco igual (interior, seco) | Está marcado con la misma inicial de Uma |
| Radio de Charly (con dueño) | Radio sin nadie | Suena sola, sin enchufarse |
| Planta de Nélida (con maceta) | Planta igual (sin maceta) | Florece aunque no hay luz |

**La puerta entreabierta:**

A mitad de la zona hay una puerta madera oscura, entreabierta. Detrás hay oscuridad y la voz de Elena. El audio es de una conversación real que pudo haber ocurrido en cualquier momento de la infancia de Uma —el juego no aclara cuándo—.

*Audio completo de la puerta:*

> **Elena (voz):** "... no porque sea fácil. Es que hay cosas que no se entienden hasta que las sentís. Por eso no sirve que te las expliquen. Nadie te puede explicar cómo es. Tenés que ir."
>
> *(Pausa. Voz de una niña —Uma de pequeña— que no se escucha con claridad.)*
>
> **Elena (voz):** "No, no da miedo. O sí da miedo, pero de un tipo distinto. El miedo que da antes es diferente al que da después. El de antes es más grande."
>
> *(La puerta se cierra sola. Suave. Sin drama.)*

**La sala de activadores:**

Una habitación con tres puntos de interacción: ranura, gancho, marco. Ver Sistema de Mecánicas para detalles completos de activación.

**El pasillo final:**

Corredor angosto que lleva a la Antesala. Antes de que Uma entre, el juego hace un primer plano de su cara desde el frente. Es el único momento en que la cámara rompe la perspectiva lateral.

*Lo que Uma ve: el pasillo. Lo que el jugador ve: Uma viendo el pasillo.*

---

### 4.7 ZONA 4 — La Antesala

**Descripción atmosférica:**

Patio interno de un edificio antiguo. Techo de cielo abierto, rodeado de paredes de tres pisos que no tienen ventanas que den ahí. La llovizna cae, silenciosa. Una farola ornamental en el centro, apagada. Dos plantas grandes en macetas de cemento. Un banco de madera.

Este espacio parece no pertenecer a nadie. No hay señalización, no hay carteles, no hay timbre ni número. Simplemente es un lugar que existe entre otros lugares.

**La segunda nota:**

Sobre el banco, protegida del agua por una bolsa plástica transparente, está la segunda nota de Héctor. Véase sección de personajes para texto completo.

**El momento de decisión:**

Cuando Uma llega a la puerta del fondo, el juego pausa el movimiento. La puerta está entreabierta —la misma puerta de madera oscura de la zona interior, pero desde otro lado—. El jugador debe presionar un botón para avanzar. No hay tiempo límite. Si el jugador espera más de dos minutos sin moverse, Uma dice:

> *"Ya llegué hasta acá."*

Y avanza sola.

---

### 4.8 ZONA 5 — El Espacio Final

**Descripción atmosférica:**

Una sala grande, de techo alto. Podría ser el interior de una casona antigua reconvertida, un espacio de ensayo abandonado, la planta baja de un edificio que perdió su función original. Uno de los lados —el que da al oeste, donde el sol ya bajó— está abierto: una reja de hierro cubierta por plantas trepadoras separa el interior del patio exterior. A través de las plantas se ve el cielo de la tarde, gris y lluvia fina.

La luz es cálida y viene de algún lugar que no se ve directamente: probablemente una claraboya filtrada, o lámparas que alguien dejó encendidas y que nadie apagó.

**Elementos del espacio:**

- Radio antigua: mesa de madera, radio de válvulas con manopla de baquelita
- Ranura para el disco: parte de la radio, aunque no es así como funciona una radio de válvulas; el juego no hace un punto de esto
- Silla de madera en el centro
- Pared del fondo con manchas de humedad que tienen formas
- Los objetos que Uma trajo (si los trajo) están visibles, como si el espacio los esperara

---

## 5. CAPÍTULOS — NARRATIVA DETALLADA

### 5.1 PRÓLOGO — El Último Día Adentro

**Hora del día:** 9:30 AM  
**Clima:** Sol intenso afuera. Adentro, la persiana filtra una luz naranja.  
**Duración estimada:** 10–20 minutos

**Estructura:**

El prólogo no tiene objetivos. Tiene *posibilidades*. El jugador puede explorar cada centímetro del departamento. Cada objeto tiene al menos una línea de respuesta de Uma. Los objetos más cargados tienen tres o cuatro.

**El orden sugerido de exploración (no obligatorio):**

1. Uma se despierta en su cuarto. La cámara está sobre el techo, mirando hacia abajo. Uma abre los ojos.
2. Puede quedarse en la cama o levantarse. Si se queda más de 90 segundos, se levanta sola con un monólogo: *"Hoy no es un día de quedarse."*
3. El cuarto es lo primero. Estante, ventana, escritorio.
4. Después el living (Tomás).
5. Después la cocina (Elena, nota de Héctor).
6. Después el patio.
7. Cualquiera de los tres detonantes puede activarse en cualquier momento después del punto 3.

**Los tres detonantes — detalles completos:**

**Detonante 1 — El brillo en la vereda:**
> Uma se acerca a la ventana de su cuarto. La persiana filtra el sol pero hay un espacio en el borde inferior. Si Uma se inclina (el jugador inclina la cámara), puede ver la vereda. Hay algo que brilla: no se puede distinguir qué es. *(Es el fragmento de tapa de disco que Pablo dejó.)*
>
> *Monólogo de Uma:* *"Algo refleja. No sé qué. Probablemente nada. Pero brilla."*
>
> *Pausa. Uma se endereza. Mira la puerta.*

**Detonante 2 — La melodía:**
> En la cocina, con Elena, hay un momento de silencio en el que se escucha la calle. Desde alguna radio en el exterior llega una melodía. Uma para de moverse. Se queda escuchando. La melodía se corta (pasa un colectivo, o alguien cierra una ventana).
>
> *Monólogo de Uma:* *"La conocí. No de ahora. De antes. No sé de cuándo."*
>
> *Pausa. Uma mira hacia la puerta de entrada.*

**Detonante 3 — La página del cuaderno:**
> En el patio, Uma puede abrir el cuaderno. Al pasar las páginas, encuentra una que no estaba antes (o que nunca miró): el mismo dibujo abstracto de Tomás pero más detallado. Abajo, con letra que no es la de Uma ni la de Tomás, dice: *"Ya estuviste cerca."*
>
> *Uma mira al living. Tomás dibuja, ajeno.*
>
> *Monólogo de Uma:* *"No es su letra. No es mi letra. No entiendo cuándo entró esto acá."*
>
> *Pausa. Uma cierra el cuaderno. Mira la puerta.*

**El momento de salida:**

Cualquiera que sea el detonante, el resultado es el mismo:

> Uma va al recibidor. Se pone el sombrero. Lo acomoda. Abre la puerta. Se queda un segundo en el umbral, con el sol de afuera en la cara.
>
> *Monólogo:* *"Bueno."*
>
> Sale.

---

### 5.2 ACTO I — El Barrio

**Hora del día:** 10:00–10:45 AM  
**Clima:** Sol intenso, cielo sin nubes  
**Duración estimada:** 20–30 minutos

El barrio se presenta como familiar pero levemente extraño: Uma no salió en meses y las cosas pueden haber cambiado. El kiosco tiene un cartel nuevo. Un árbol cayó y pusieron un bache de asfalto donde estaba. La heladería cambió el menú.

Estos detalles son puramente observacionales. Uma los nota. El jugador los nota con Uma.

**Progresión narrativa:**

La estructura del barrio es una espiral que se abre: Uma empieza cerca de su edificio y cada cuadra la lleva un poco más lejos de lo cómodo.

**Momentos obligatorios:**
1. Encuentro con Nélida (al salir del edificio)
2. Fragmento de tapa de disco (cuadra 4)
3. Llegada a la esquina que da hacia la plaza

**Momentos opcionales (que añaden capas):**
1. Conversación con Charly (y el corte de la melodía)
2. Conversación con Don Mario
3. Conversación con Ramiro y Nico
4. La tienda cerrada y la foto polaroid
5. La placa de Bardi en el edificio de la cuadra 4
6. El timbre con el apellido Bardi
7. El gato en el umbral
8. La radio desde el primer piso (melodía fragmentada)

**El descubrimiento del fragmento de disco:**

Uma llega a la esquina de la cuadra 4. En el suelo, contra el cordón de la vereda, hay el fragmento. No está dramatizado: es un objeto más en la calle. Si el jugador no lo nota, Uma tampoco lo nota y puede pasar de largo.

*Si Uma lo nota y se agacha:*
> *Monólogo:* *"La mitad de algo. Se puede ver parte de un dibujo. Las primeras letras de un nombre: CEL... No alcanza. Igual lo agarro. Por las dudas de qué, no sé."*

---

### 5.3 ACTO II — La Plaza

**Hora del día:** 10:45 AM–12:00 PM  
**Clima:** Sol → Nublado → Llovizna suave  
**Duración estimada:** 25–35 minutos

La plaza es el acto más largo y el más rico en posibilidades de exploración. El cambio climático es el evento central estructurante.

**El arco climático de la plaza:**

```
Llegada: Sol intenso
    │ (Uma bordea el perímetro buscando sombra)
    ▼
10 minutos de juego real: primeras nubes
    │ (Cambio de paleta — amarillo → blanco difuso)
    ▼
15 minutos: cielo completamente cubierto
    │ (Música cambia — más suave, más interna)
    ▼
20 minutos: primeras gotas
    │ (NPCs reaccionan: algunos se van, otros sacan paraguas)
    ▼
25 minutos: llovizna estable
    │ (Uma no se va)
```

**El disco de vinilo:**

En el centro de la plaza, en el borde de la fuente seca, está el disco de vinilo. Está mojado pero en buen estado. Si Uma tiene el fragmento de tapa, puede ver que es el mismo artista: **CELSO BARDI — ADENTRO**.

*Monólogo al recogerlo (sin fragmento previo):*
> *"Un vinilo en el borde de una fuente. Quién lo deja acá. Celso Bardi. No lo conozco. El disco se llama Adentro. Paradoja no buscada."*

*Monólogo al recogerlo (con fragmento previo):*
> *"El mismo artista. La mitad de la tapa que encontré era la mitad de esta tapa. Celso Bardi. El disco se llama Adentro. ¿Quién deja esto en una fuente? ¿Quién deja la mitad en la vereda a cuatro cuadras? No tiene sentido. Pero acá está."*

**El momento del banco (pausa voluntaria):**

Si Uma se sienta en el banco del alero y el jugador no hace nada durante 180 segundos reales, Uma saca el cuaderno.

*Animación:* Uma abre el cuaderno en la primera página en blanco. Mira la plaza. Empieza a dibujar. El dibujo aparece como si fuera dibujado a tiempo real: la fuente, los árboles, la lluvia.

*Monólogo mientras dibuja:*
> *"Nunca dibujo afuera. Adentro sí. Afuera hay demasiado. Hoy hay exactamente suficiente."*

---

### 5.4 ACTO III — La Zona Interior

**Hora del día:** 12:00–12:30 PM  
**Clima:** Nublado, sin lluvia  
**Duración estimada:** 15–20 minutos

Este acto es el más introspectivo. El ritmo baja. La música desaparece casi por completo y lo que queda es sonido ambiental: gotas en superficies, pasos de Uma, algo que cruje en las paredes.

**Los activadores — mecánica completa:**

**Activador 1 — La ranura del disco:**
> Uma puede poner el disco de vinilo en la ranura de la pared. La ranura es un objeto absurdo que existe ahí sin explicación. El disco encaja.
>
> Suena una porción de música: el primer verso de "La vuelta". Es la melodía que Uma reconoció desde la cocina, ahora con letra. La letra habla de alguien que fue y volvió y encontró que el lugar era diferente —no el lugar, quien volvió—.
>
> El disco se puede sacar y llevar al Acto V.

**Activador 2 — El gancho del sombrero:**
> Uma puede colgar el sombrero en el gancho. Si lo hace, la consecuencia es real: Uma sigue el recorrido sin sombrero. La luz del juego cambia levemente —no más intensa, sino diferente, sin el filtro del ala—. Uma se mueve de forma diferente: un poco más expuesta, un poco más presente.
>
> *Monólogo:* *"Sin el sombrero todo está un poco más cerca. No sé si eso es bueno o malo todavía."*
>
> El sombrero puede recuperarse antes de salir de la zona.

**Activador 3 — El marco vacío:**
> Si Uma tiene la foto polaroid de la tienda cerrada, puede colocarla en el marco.
>
> *Monólogo:* *"No sé quiénes son. Pero se ven contentos. Esa clase de contentos que no le están mostrando a nadie."*
>
> La foto puede recuperarse.

---

### 5.5 ACTO IV — La Antesala

**Hora del día:** 12:30–13:00 PM  
**Clima:** Llovizna suave persistente  
**Duración estimada:** 10–15 minutos

La antesala es el acto más corto. Es una pausa deliberada antes del clímax. El jugador puede explorar el espacio durante el tiempo que quiera, pero hay poco que hacer más allá de leer la nota, comparar las notas (si tiene la primera) y esperar.

**La comparación de las notas:**

Si Uma tiene la primera nota de Héctor (de la mesa del comedor), al leer la segunda nota en el banco puede compararlas. El juego muestra las dos notas en pantalla simultáneamente.

*Monólogo:*
> *"La misma letra. El mismo papel. Son del mismo momento o de momentos distintos. No sé. Él podría haberlas escrito juntas y dejado la segunda acá antes. O podría ser que el papel recuerda cosas que no pasaron todavía. No creo eso. Pero lo pienso igual."*

---

### 5.6 ACTO V — El Espacio Final

**Hora del día:** 13:00 PM  
**Clima:** Lluvia suave continua (no llega adentro, se escucha)  
**Duración estimada:** 15–25 minutos

**La música completa:**

Cuando Uma pone el disco en la radio, "La vuelta" suena completa por primera vez. Son cuatro minutos de música. El juego no hace nada durante esos cuatro minutos excepto existir: la lluvia de fondo, el espacio, la luz cálida.

**El texto que aparece durante la canción:**

Mientras la música suena, frases de los diálogos previos aparecen en distintos lugares del espacio —proyectadas, flotantes, sin explicación de mecanismo—. No todas en todas las partidas: solo las que el jugador activó.

*Posibles textos (selección según partida):*

> *"Soñé que ibas a encontrar algo."* — (aparece cerca de la entrada)
>
> *"A veces salir un ratito cambia las cosas."* — (aparece sobre la ranura de la radio)
>
> *"Vas a encontrar lo que buscás."* — (aparece en la pared del fondo)
>
> *"Las palomas van a volver."* — (aparece cerca de la abertura con la lluvia)
>
> *"Cuando llueve todo suena más lindo."* — (aparece en el techo)

---

## 6. SISTEMA DE DIÁLOGOS COMPLETO

### 6.1 Árbol de Diálogo — Estructura General

```
NODO RAÍZ
    │
    ├── [CONDICIÓN: Clima actual]
    │       ├── sol_intenso → variante A de diálogo
    │       ├── nublado     → variante B de diálogo
    │       └── llovizna    → variante C de diálogo
    │
    ├── [CONDICIÓN: Objetos en inventario]
    │       ├── tiene_disco     → línea adicional
    │       ├── tiene_polaroid  → línea adicional
    │       └── tiene_nota      → línea adicional
    │
    ├── [CONDICIÓN: Eventos completados]
    │       ├── habló_con_nelida    → referencia disponible
    │       ├── escuchó_melodía     → reconocimiento disponible
    │       └── dibujó_en_plaza     → mención disponible
    │
    └── [CONDICIÓN: Estado de Uma]
            ├── sin_sombrero → variante emocional distinta
            └── con_sombrero → variante emocional base
```

### 6.2 Monólogos Internos de Uma — Catálogo Extendido

**Al salir del edificio (sol intenso):**
> *"Acá está. El exterior. Con toda su claridad y su calor y sus personas que caminan hacia sus cosas. Veintitrés grados y sol directo. Mi combinación favorita para quedarme adentro. Bueno. Igual salí."*

**Al ver el arbolado (primera cuadra):**
> *"El arbolado viejo es lo mejor que tiene este barrio. Alguien pensó en eso hace cincuenta años y nosotros todavía lo agradecemos sin saberlo. Los sauces dan sombra, los jacarandás dan color. Los que los plantaron no los vieron crecer. Eso tiene algo."*

**Al pasar por el edificio de Bardi (sin información previa):**
> *"Un edificio cualquiera. Tres pisos, balcones con plantas que nadie regó en semanas, una placa en la pared que no llego a leer."*

**Al pasar por el edificio de Bardi (leyó la placa):**
> *"Aquí vivió alguien. Aquí grabó algo que ahora está en mi bolso. No sé qué significa eso pero no me parece casualidad. O sí me parece casualidad. La clase de casualidad que si te la contaran como ficción dirías que es demasiado prolija."*

**Al entrar a la plaza por primera vez:**
> *"La plaza de siempre. La misma plaza de cuando era chica, de cuando venía con mamá, de cuando vine con...*  *(pausa)* *Con quién. Vine sola. Vine con Tomás. Me confundí."*

**Al quedarse bajo la llovizna:**
> *"No me estoy yendo. Eso es nuevo. Normalmente ya estaba buscando el primer alero. Pero la lluvia está tibia y el cielo gris es exactamente el cielo que me gusta. Me quedo."*

**Al ver el disco en la fuente:**
> *(Ver sección Acto II — narrativa completa)*

**Sin sombrero en la Zona Interior:**
> *"Noto que no lo tengo. No que lo extraño — bueno, sí, pero no tanto. Noto que el espacio llega diferente. Como cuando sacás un filtro de la cámara y la imagen es un poco más de todo."*

**Antes de pasar por el pasillo final (Zona Interior):**
> *"Hay una cosa que pasa antes de entrar a lugares que no conocés. No es miedo exactamente. Es un tipo de atención que el cuerpo pone solo. Me gusta ese momento. Siempre me gustó ese momento. Me olvidé de que me gustaba."*

---

## 7. SISTEMA DE MECÁNICAS

### 7.1 Mecánicas de Exploración

**Movimiento:**
- Perspectiva lateral (2.5D con profundidad)
- Uma camina a velocidad constante. No corre. No se puede forzar que corra.
- Hay una tecla/botón de "paso lento" que Uma usa automáticamente en momentos de reflexión pero el jugador también puede activar
- Colisiones con todos los objetos del entorno, incluyendo los que no tienen interacción

**Sistema de Interacción:**
- Botón de inspección: activa monólogo interno de Uma sobre cualquier objeto
- Botón de interacción: activa diálogos con NPCs o usos de objetos
- Botón de pausa / cuaderno: abre el cuaderno de bocetos en cualquier momento
- Los NPCs tienen un radio de detección: dentro de él, Uma los saluda automáticamente (si es la primera vez) o los reconoce (si ya habló)

**Zonas de Confort:**
```
ZONA SOMBRA       → Uma se mueve normal, monólogos neutrales/positivos
ZONA SOL DIRECTO  → Paleta más cálida/saturada, monólogos de incomodidad
ZONA LLUVIA       → Paleta más fría/azulada, monólogos más abiertos/positivos
ZONA INTERIOR     → Paleta difusa, sin sombras marcadas, monólogos más reflexivos
```

Ninguna zona bloquea el avance. Los efectos son puramente expresivos.

### 7.2 Sistema del Sombrero

El sombrero es el objeto de identidad central de Uma. Tiene cuatro estados:

```
PUESTO          → Estado base. Uma cómoda.
PERDIDO/COLGADO → Uma lo menciona. Efectos visuales/sonoros sutiles.
RECUPERADO      → Uma lo pone de nuevo con un gesto específico de alivio.
DEJADO VOLUNTARIAMENTE → Uma elige dejarlo. Consecuencias diferentes.
```

**Estados posibles según decisiones:**

| Situación | Estado | Consecuencia |
|---|---|---|
| Inicio del juego | Puesto | Base |
| Activador del gancho (Zona III) | Voluntariamente colgado | Uma sin sombrero — el jugador debe recuperarlo activamente |
| Si Uma no recupera el sombrero antes del Acto V | Sin sombrero al final | Diálogo de Pablo levemente diferente |
| Si Uma lo pierde (evento de viento, no implementado en v1) | Perdido | Uma busca hasta encontrarlo |

### 7.3 Sistema de Clima — Mecánicas Detalladas

**Transiciones:**

El clima cambia de forma gradual. No hay "cortes" entre estados. Cada transición tiene una duración de 3 a 5 minutos de juego real.

```
PARÁMETROS DEL SISTEMA DE CLIMA:
├── Intensidad de luz: 0–100 (100 = sol máximo)
├── Temperatura de color: cálido → frío
├── Opacidad de nubes: 0–100
├── Intensidad de lluvia: 0–100
└── Factor de viento: 0–10 (afecta árboles, ropa, pelo de Uma)
```

**Efectos en gameplay:**

- **Sol intenso (> 80):** NPCs buscan sombra, Charly tiene la radio más baja, el fragmento de disco brilla
- **Nublado (40–60):** Transición, algunos NPCs sacan paraguas preventivos, la música de fondo cambia
- **Llovizna (lluvia < 30):** NPCs que no tienen paraguas se refugian bajo galerías, Uma no se refugia (mecánica de identidad), los sonidos del ambiente cambian
- **Sin lluvia (Zona III):** El silencio sonoro es más marcado, el único audio es ambiental interno

### 7.4 Sistema de Resonancia

**Concepto:** Cada vez que Uma escucha un fragmento de la melodía de "La vuelta", el juego registra el evento. Al final, en el Espacio Final, la música que suena la radio está "construida" por todos los fragmentos que Uma escuchó: el orden, la versión, los instrumentos que predominan dependen de qué versiones escuchó.

| Fragmento | Dónde | Instrumento predominante |
|---|---|---|
| Fragmento 1 | Cocina (detonante 2) | Guitarra sola |
| Fragmento 2 | Radio de Charly (si escucha) | Guitarra + voz |
| Fragmento 3 | Radio en ventana primer piso | Guitarra + percusión |
| Fragmento 4 | Adolescentes en la galería | Versión electrónica sampleada |
| Fragmento 5 | Activador del disco (Zona III) | Versión completa sin letra |
| CANCIÓN COMPLETA | Espacio Final | Construcción de todos los escuchados |

Si Uma no escuchó ninguno (no activó detonante 2, no se detuvo con Charly, pasó de largo por la galería), la canción final suena igual pero Uma lo menciona: *"No la conocía. Y sin embargo suena como si siempre hubiera estado ahí."*

### 7.5 Sistema de Tiempo Interno

**Estructura temporal:**

El juego transcurre en tiempo narrativo entre las 9:30 AM y las 2:00 PM aproximadamente. El tiempo narrativo no es igual al tiempo real de juego.

```
TIEMPO NARRATIVO vs TIEMPO REAL:
├── Prólogo (9:30 AM): 10–20 min reales
├── Acto I - Barrio (10:00 AM): 20–30 min reales
├── Acto II - Plaza (10:45 AM): 25–35 min reales
├── Acto III - Zona Interior (12:00 PM): 15–20 min reales
├── Acto IV - Antesala (12:30 PM): 10–15 min reales
└── Acto V - Final (1:00 PM): 15–25 min reales
```

**Indicadores de tiempo:**

El tiempo no aparece en pantalla. Se infiere por:
- La posición del sol (cuando hay sol)
- Los comentarios de Uma ("ya debe ser el mediodía")
- Los NPCs ("a esta hora no hay nadie acá normalmente")
- La apertura/cierre de locales (la heladería abre en el Acto II si Uma pasa segunda vez)

**Tiempos de espera:**

Si el jugador se queda inactivo, Uma tiene comportamientos distintos según la zona:
- **Cuarto propio:** Se recuesta en la cama (no duerme, solo está)
- **Vereda:** Mira la calle con los brazos cruzados
- **Plaza (banco):** Saca el cuaderno (si no lo sacó antes) o lo abre y pasa páginas sin dibujar
- **Zona Interior:** Se sienta en el suelo, apoyada contra la pared
- **Antesala:** Mira el cielo

---

## 8. SISTEMA DE OBJETOS E INVENTARIO

### 8.1 El Cuaderno de Bocetos — Objeto Permanente

El cuaderno siempre está con Uma. Se puede abrir desde el menú en cualquier momento.

**Contenido inicial (objetos pre-cargados):**

```
Página 1: Boceto de un banco de plaza. Vacío.
Página 2: Boceto de un árbol. Con sombra marcada.
Página 3: Boceto de una figura con sombrero. De espaldas.
Página 4: [EN BLANCO — espacio para dibujos de Uma durante el juego]
Página ???: La página misteriosa (solo visible desde el patio)
```

**Dibujos que Uma puede hacer durante el juego:**

| Evento | Dónde | Qué dibuja |
|---|---|---|
| Banco de la plaza (3 min inactivo) | Plaza | Vista de la plaza bajo la lluvia |
| Inspección de la placa de Bardi | Cuadra 4 | El edificio, con la placa marcada |
| Momento en la Antesala (2 min inactivo) | Antesala | El patio vacío con la farola |
| Después de la canción completa (opcional) | Acto V | La radio encendida |

**El cuaderno como registro:**

Al terminar el juego, el cuaderno se "congela" con todo lo que Uma dibujó. El jugador puede volver a abrirlo en el menú de fin para ver el recorrido en dibujos.

---

### 8.2 Objetos Recolectables — Catálogo Completo

**Objeto 1: FRAGMENTO DE TAPA DE DISCO**

- **Dónde:** Cuadra 4, contra el cordón de la vereda
- **Descripción inicial:** *"La mitad de algo. Se ve parte de un dibujo. Las letras CEL y algo más que no se puede leer."*
- **Descripción tras encontrar el disco completo:** *"Era la mitad de esto. La mitad que faltaba estaba en una fuente a cuatro cuadras. Alguien los separó o los perdió. O los dispersó adrede. No sé para qué."*
- **Uso:** Permite leer el nombre del artista en el disco completo + activa línea de diálogo con Valentina

---

**Objeto 2: DISCO DE VINILO — CELSO BARDI / ADENTRO**

- **Dónde:** Fuente seca de la plaza
- **Descripción inicial:** *"Celso Bardi. Adentro. Disco de vinilo. Mojado pero en buen estado. No tengo tornamesa en casa."*
- **Descripción al activar en Zona III:** *"Suena aunque no debería poder sonar acá. Pero suena."*
- **Descripción al llegar al Acto V:** *"La radio va a aceptar esto. No sé cómo lo sé. Pero lo sé."*
- **Uso:** Activa fragmento musical en Zona III + activa canción completa en Acto V + cambia diálogo de Pablo

---

**Objeto 3: FOTO POLAROID**

- **Dónde:** Tienda cerrada, Barrio
- **Descripción inicial:** *"Dos personas en un parque. No se ven las caras. Una tiene sombrero. No sé si es mi sombrero o es otro sombrero."*
- **Descripción al colocar en el marco (Zona III):** *"No sé quiénes son. Pero se ven contentos. La clase de contentos que no le están mostrando a nadie."*
- **Descripción al llegar al Acto V:** *"La dejé en el marco. O la traje. Está acá igual."*
- **Uso:** Activa el marco vacío en Zona III + activa línea de diálogo con Valentina

---

**Objeto 4: PRIMERA NOTA DE HÉCTOR**

- **Dónde:** Mesa del comedor, Prólogo (opcional — puede dejarla)
- **Descripción inicial:** *"La letra de papá. No pone la fecha. Podría ser de esta mañana o de hace tres semanas. A veces hace eso."*
- **Descripción en Antesala (al compararla con la segunda):** *"Mismo papel. Misma letra. El mismo momento o momentos distintos."*
- **Uso:** Activa escena de comparación en Antesala + cambia diálogo de Pablo en el Acto V

---

**Objeto 5: SEGUNDA NOTA DE HÉCTOR**

- **Dónde:** Banco de la Antesala (siempre disponible, no se puede perder)
- **Descripción:** *"A veces llegar no es lo difícil. Es animarse a seguir cuando ya llegaste."*
- **Uso:** Activa la escena de comparación si Uma tiene la primera nota + activa línea específica de Pablo

---

**Objeto 6: DIBUJO DE UMA (variable)**

- **Dónde:** Activo en plaza, Cuadra 4 o Antesala (el primero que Uma dibuje queda como objeto)
- **Descripción:** Varía según qué dibujó
- **Uso:** Aparece en el resumen final del cuaderno + Pablo lo menciona si Uma lo hizo en la plaza

---

### 8.3 Objetos de Inspección (No Recolectables)

Objetos que Uma puede inspeccionar pero no llevar:

| Objeto | Ubicación | Monólogo de Uma |
|---|---|---|
| Cada disco del estante (23 discos) | Cuarto | Respuestas individuales para cada uno |
| Foto de Héctor | Mesita de noche | *"Me parezco a él en los ojos. En el temperamento, dice mamá. No sé si es un cumplido."* |
| Plantas del patio | Patio | *"Una está floreciendo. Esa que mamá dijo que no iba a florecer más. Acá está."* |
| Buzón viejo | Cuadra 1 | *"Hace años que no funciona. Pero nadie lo sacó. No sé si eso es optimismo o descuido."* |
| Gato en el umbral | Cuadra 2 | *"Me miró, decidió que no era interesante y volvió a dormir. Correcto."* |
| Iniciales en el árbol | Plaza | *(ver Zona 2 — punto de interés secreto)* |

---

## 9. SISTEMA DE CLIMA Y TIEMPO

### 9.1 Paletas de Color por Estado Climático

**Sol Intenso (Prólogo y Barrio):**
```
Cielo:      #87CEEB → #FFF3B0 (azul → amarillo cálido en el horizonte)
Sombra:     #4A5568 con tono naranja
Luz directa: Temperatura 5500K, sombras duras
Saturación: +20% respecto a base
```

**Transición a Nublado (Plaza temprana):**
```
Cielo:      #B0C4DE → #D3D3D3 (azul claro → gris perla)
Sombra:     #6B7280, difusa
Luz:        Temperatura 4500K, sombras suaves
Saturación: Base
```

**Nublado pleno (Plaza tardía, inicio Zona III):**
```
Cielo:      #9EAFC2 → #8899AA
Ambiente:   Temperatura 4000K, sin sombras marcadas
Saturación: -10% respecto a base
```

**Llovizna (Zona III tardía, Antesala, Acto V):**
```
Cielo:      #708090 → #5F6B7C
Partículas: lluvia leve, 30–40% opacidad
Ambiente:   Temperatura 3800K, luz difusa y cálida en interiores
Saturación: -15% exterior, base en interiores
```

### 9.2 Capas de Sonido por Estado

**Sol intenso:**
- Capa base: ambiente de ciudad (tráfico lejano, pájaros, voces distantes)
- Capa situacional: radio de Charly (si cerca), obras (si hay en la cuadra)
- Música: ausente o muy baja, solo en momentos específicos

**Nublado:**
- Capa base: ambiente más bajo, viento suave
- Capa situacional: movimiento de hojas, una paloma
- Música: ingresa suavemente, cuerda o piano solo

**Llovizna:**
- Capa base: lluvia constante (no fuerte), goteo en superficies
- Capa situacional: paraguas, pisadas en charcos, puertas que se cierran
- Música: más completa, más capas

**Interior (Zona III):**
- Capa base: silencio casi total
- Capa situacional: crujidos de madera, eco de los pasos de Uma
- Música: solo en momentos de activación

---

## 10. SISTEMA DE MEMORIA Y RESONANCIA

### 10.1 Flags de Memoria — Lista Completa

El juego registra todos los eventos en un sistema de flags booleanos. Estos flags determinan:
- Qué diálogos se activan
- Qué dice Pablo al final
- Qué textos flotantes aparecen durante la canción
- Cómo queda el cuaderno de bocetos

**Flags principales:**

```
[PRÓLOGO]
├── flag_detonante_brillo       → Activó el brillo desde la ventana
├── flag_detonante_melodia      → Escuchó la melodía desde la cocina
├── flag_detonante_cuaderno     → Encontró la página misteriosa
├── flag_habló_tomas            → Tuvo conversación completa con Tomás
├── flag_habló_elena            → Tuvo conversación completa con Elena
└── flag_guardó_nota_padre_1    → Guardó la primera nota de Héctor

[ACTO I - BARRIO]
├── flag_habló_nelida_1/2/3     → Nivel de conversación con Nélida
├── flag_charly_cancion_cortada → Escuchó la canción antes del corte
├── flag_habló_mario            → Habló con Don Mario
├── flag_habló_ramiro_nico      → Habló con los chicos de las cartas
├── flag_recogió_fragmento      → Recogió el fragmento de tapa
├── flag_tomó_polaroid          → Tomó la foto de la tienda
├── flag_leyó_placa_bardi       → Leyó la placa del edificio
└── flag_timbró_bardi           → Tocó el timbre de Bardi

[ACTO II - PLAZA]
├── flag_fue_al_centro          → Uma fue al centro de la plaza bajo el sol
├── flag_recogió_disco          → Recogió el disco de vinilo
├── flag_habló_valentina_1/2/3  → Nivel de conversación con Valentina
├── flag_habló_ernesto          → Habló con Ernesto
├── flag_escuchó_adolescentes   → Se acercó lo suficiente para escuchar
└── flag_dibujó_plaza           → Uma dibujó en el banco de la plaza

[ACTO III - ZONA INTERIOR]
├── flag_escuchó_voz_elena      → Escuchó el audio de la puerta
├── flag_activó_disco           → Puso el disco en la ranura
├── flag_dejó_sombrero          → Colgó el sombrero en el gancho
├── flag_recuperó_sombrero      → Recuperó el sombrero antes de seguir
└── flag_colocó_polaroid        → Puso la foto en el marco

[ACTO IV - ANTESALA]
├── flag_comparó_notas          → Comparó las dos notas de Héctor
└── flag_esperó_sola            → Uma avanzó sola (sin input del jugador)

[ACTO V - FINAL]
├── flag_puso_disco_radio       → Puso el disco en la radio
├── flag_escuchó_cancion_completa → Escuchó la canción sin moverse
└── flag_respuesta_uma_pablo    → Qué respondió Uma a Pablo (3 opciones)
```

### 10.2 Cómo los Flags Modifican el Final

**Diálogo de Pablo — Variables:**

Pablo tiene una línea base que siempre dice. Sobre esa línea, agrega referencias a todo lo que vio de Uma ese día —porque estuvo en el barrio, la vio sin que ella lo supiera—.

```
flag_recogió_fragmento + flag_recogió_disco:
→ Pablo: "Sabía que lo ibas a reconocer. Yo lo dejé ahí."
  Uma puede preguntar: "¿Cuál de los dos?"
  Pablo: "Los dos. Fue sin pensar. Caminé por el barrio y los dejé."

flag_dibujó_plaza:
→ Pablo: "Te vi dibujar en el banco. La lluvia ya caía."
  Uma: "No te vi."
  Pablo: "Ya sé. Estaba del otro lado de la galería."

flag_dejó_sombrero + flag_no_recuperó_sombrero:
→ Pablo: "¿Dónde dejaste el sombrero?"
  Uma: "En algún lado."
  Pablo: "¿Y?"
  Uma: "Y nada. Igual estoy acá."

flag_habló_nelida_3 (nivel máximo):
→ Pablo: "¿Hablaste con la señora del balcón?"
  Uma: "¿La conocés?"
  Pablo: "Vivo acá cerca. Me crucé con ella."

flag_guardó_nota_padre_1 + flag_nota_padre_2:
→ Pablo: "¿Comparaste las dos notas?"
  Uma: "¿Cómo sabés que hay dos?"
  Pablo: "No sé. Lo adiviné." (No lo adivina. Lo supo porque conoce a Héctor. Pero el juego no lo aclara.)
```

---

## 11. MÚSICA Y DISEÑO SONORO

### 11.1 "La Vuelta" — Celso Bardi

La canción que atraviesa el juego es una composición original para el videojuego que suena como si hubiera sido compuesta en los años ochenta en Argentina. Folklore experimental con influencias del tango contemporáneo y la canción de autor.

**Estructura de la canción:**

```
INTRO: 32 compases. Guitarra sola.
ESTROFA 1: Voz + guitarra.
PUENTE: Sube percusión leve.
ESTROFA 2: Voz + guitarra + bandoneón.
ESTRIBILLO: Todo + voz más alta.
OUTRO: Retorno a guitarra sola. Se desvanece.
Duración total: 4 minutos 12 segundos.
```

**Letra completa de "La Vuelta":**

```
Salí una tarde sin razón precisa
con el sombrero y sin destino cierto
el sol me dijo lo que no precisaba
y la sombra me dijo que siguiera.

Crucé la plaza como quien la cruza
sin saber que estaba siendo visto
encontré en el borde lo que no buscaba
y lo llevé conmigo sin permiso.

Volver no es llegar al mismo lugar,
volver es llegar siendo diferente.
El barrio es el mismo, la plaza es la misma,
pero yo ya no soy quien era antes.

La lluvia que llega cuando menos se espera
no es la lluvia de siempre, es otra lluvia.
Quedarse bajo el cielo que se abre
es el principio de todo lo que sigue.
```

### 11.2 Diseño Sonoro — Principios

**Capas de audio:**

1. **Música narrativa:** Fragmentos de "La vuelta" y música incidental. Se mezcla con el ambiente.
2. **Ambiente sonoro:** Grabaciones reales de barrio porteño. Cada zona tiene su set específico.
3. **Foley de Uma:** Pasos, el sombrero al ponérselo, el cuaderno al abrirse, el disco al recogerse.
4. **Voces de NPCs:** Sin voice acting completo — solo las líneas emocionales clave tienen voz. El resto es texto con sonido de "habla" estilizado.

**Momentos de silencio diseñado:**

El juego usa el silencio activamente:
- Cuando Uma recoge el disco completo: 3 segundos de silencio antes de que vuelva el ambiente
- Cuando se cierra la puerta de la voz de Elena: silencio de 5 segundos
- Antes de que Pablo hable por primera vez: silencio de 8 segundos

---

## 12. FINALES Y VARIABLES NARRATIVAS

### 12.1 Estructura de Finales

El juego no tiene "final bueno" ni "final malo". Tiene finales **más completos** y finales **más escuetos**. La diferencia es cuántas capas de significado se activan, no la calidad emocional del cierre.

**Clasificación de finales por completitud:**

| Nombre | Condición | Descripción |
|---|---|---|
| **Final Completo** | 6+ flags narrativos clave | Todos los diálogos de Pablo activos, textos flotantes completos durante la canción, cuaderno lleno |
| **Final Resonante** | 3–5 flags narrativos clave | Pablo hace referencias parciales, algunos textos flotantes, cuaderno con algunos dibujos |
| **Final Escueto** | Menos de 3 flags | Versión base de Pablo, sin textos flotantes, cuaderno vacío |
| **Final del Sombrero Ausente** | flag_no_recuperó_sombrero activo | Uma llega sin sombrero. Diálogo de Pablo diferente. Cierre idéntico pero con monólogo adicional de Uma. |

### 12.2 Las Tres Respuestas de Uma a Pablo

**Respuesta A — Irónica:**
> **Uma:** "Tarde mucho, lo sé."
>
> **Pablo:** *(pausa)* "No importa el cuánto. Importa que estás acá."
>
> **Uma:** *(monólogo interno):* *"Eso suena a frase hecha. Pero lo dice en serio. Noto la diferencia."*

**Respuesta B — Directa:**
> **Uma:** "También me alegra."
>
> **Pablo:** *(asiente. No dice nada. La canción termina)*
>
> **Uma:** *(monólogo interno):* *"Eso fue todo. Y fue suficiente."*

**Respuesta C — Silencio:**
> **Uma:** *(no dice nada. Se queda.)*
>
> **Pablo:** *(tampoco dice nada. También se queda.)*
>
> *La canción termina. La lluvia sigue. Nadie se va.*
>
> *(Monólogo final de Uma, solo en esta variante):* *"El silencio que no incomoda es el más honesto que existe."*

### 12.3 El Cierre Universal

Las tres respuestas llevan al mismo cierre:

La canción termina. La lluvia de fondo continúa. Pablo y Uma están en el espacio. La cámara se aleja despacio. La pantalla se oscurece en 15 segundos.

Aparece en tipografía pequeña, centrada:

> *"A veces salir un ratito cambia las cosas."*

Silencio. Negro.

**Pausa de 10 segundos.**

Después aparece el resumen:
- Número de objetos recolectados
- Número de NPCs con los que habló
- El cuaderno de bocetos completo (los dibujos que Uma hizo)
- La melodía de "La vuelta" suena de fondo, completa, una última vez

---

## 13. NOTAS DE IMPLEMENTACIÓN EN GODOT

### 13.1 Estructura de Escenas

```
res://
├── scenes/
│   ├── zones/
│   │   ├── Zone0_Casa.tscn
│   │   ├── Zone1_Barrio.tscn
│   │   ├── Zone2_Plaza.tscn
│   │   ├── Zone3_Zona_Interior.tscn
│   │   ├── Zone4_Antesala.tscn
│   │   └── Zone5_Final.tscn
│   ├── npcs/
│   │   ├── NPC_Nelida.tscn
│   │   ├── NPC_Charly.tscn
│   │   ├── NPC_Mario.tscn
│   │   ├── NPC_Ramiro_Nico.tscn
│   │   ├── NPC_Valentina.tscn
│   │   ├── NPC_Ernesto.tscn
│   │   └── NPC_Adolescentes.tscn
│   ├── ui/
│   │   ├── UI_Inventario.tscn
│   │   ├── UI_Cuaderno.tscn
│   │   ├── UI_Dialogo.tscn
│   │   └── UI_Monolog_Interno.tscn
│   └── systems/
│       ├── GameManager.tscn
│       ├── WeatherSystem.tscn
│       ├── DialogueSystem.tscn
│       ├── MemorySystem.tscn
│       └── AudioManager.tscn
├── scripts/
│   ├── Uma.gd
│   ├── NPCBase.gd
│   ├── InteractableObject.gd
│   ├── WeatherController.gd
│   ├── DialogueTree.gd
│   └── GameFlags.gd
├── resources/
│   ├── dialogues/
│   ├── items/
│   └── audio/
└── assets/
    ├── sprites/
    ├── backgrounds/
    └── music/
```

### 13.2 GameFlags.gd — Singleton

```gdscript
extends Node

# Objeto global que persiste entre escenas
# Contiene todos los flags del sistema de memoria

var flags := {
    # PRÓLOGO
    "detonante_brillo": false,
    "detonante_melodia": false,
    "detonante_cuaderno": false,
    "habló_tomas": false,
    "habló_elena": false,
    "guardó_nota_padre_1": false,
    
    # ACTO I
    "nelida_nivel": 0,  # 0, 1, 2, 3
    "charly_cancion": false,
    "habló_mario": false,
    "habló_chicos_cartas": false,
    "recogió_fragmento": false,
    "tomó_polaroid": false,
    "leyó_placa_bardi": false,
    "timbró_bardi": false,
    
    # ACTO II
    "fue_al_centro": false,
    "recogió_disco": false,
    "valentina_nivel": 0,  # 0, 1, 2, 3
    "habló_ernesto": false,
    "escuchó_adolescentes": false,
    "dibujó_plaza": false,
    
    # ACTO III
    "escuchó_voz_elena": false,
    "activó_disco_zona3": false,
    "dejó_sombrero": false,
    "recuperó_sombrero": false,
    "colocó_polaroid": false,
    
    # ACTO IV
    "comparó_notas": false,
    "esperó_sola": false,
    
    # ACTO V
    "puso_disco_radio": false,
    "escuchó_completa": false,
    "respuesta_pablo": ""  # "ironica", "directa", "silencio"
}

var fragmentos_melodia_escuchados := []

func set_flag(flag_name: String, value) -> void:
    if flags.has(flag_name):
        flags[flag_name] = value
    else:
        push_warning("Flag no encontrado: " + flag_name)

func get_flag(flag_name: String):
    return flags.get(flag_name, null)

func add_fragmento(fragmento_id: String) -> void:
    if fragmento_id not in fragmentos_melodia_escuchados:
        fragmentos_melodia_escuchados.append(fragmento_id)

func get_completitud() -> int:
    # Retorna 0-3 para determinar tipo de final
    var puntos := 0
    var flags_clave := [
        "recogió_disco", "tomó_polaroid", "guardó_nota_padre_1",
        "habló_mario", "valentina_nivel", "dibujó_plaza",
        "activó_disco_zona3", "escuchó_voz_elena", "comparó_notas"
    ]
    for f in flags_clave:
        if flags.get(f, false):
            puntos += 1
    
    if puntos >= 7:
        return 3  # Final completo
    elif puntos >= 4:
        return 2  # Final resonante
    else:
        return 1  # Final escueto
```

### 13.3 WeatherController.gd

```gdscript
extends Node

# Controla todos los parámetros del clima
# Se conecta al VisualEnvironment, AudioManager y NPCManager

enum WeatherState {
    SOL_INTENSO,
    NUBLANDO,
    NUBLADO,
    LLOVIZNA_SUAVE,
    LLUVIA_INTERIOR  # Para Acto V
}

var current_state: WeatherState = WeatherState.SOL_INTENSO
var transition_progress: float = 0.0
var transition_target: WeatherState

@export var sun_intensity: float = 1.0
@export var cloud_opacity: float = 0.0
@export var rain_intensity: float = 0.0
@export var color_temperature: float = 5500.0

# Paletas definidas por estado
const PALETTES = {
    WeatherState.SOL_INTENSO: {
        "color_temp": 5500.0,
        "saturation": 1.2,
        "sky_color": Color(0.53, 0.81, 0.92),
        "ambient_energy": 1.0
    },
    WeatherState.NUBLADO: {
        "color_temp": 4500.0,
        "saturation": 1.0,
        "sky_color": Color(0.62, 0.68, 0.75),
        "ambient_energy": 0.7
    },
    WeatherState.LLOVIZNA_SUAVE: {
        "color_temp": 4000.0,
        "saturation": 0.9,
        "sky_color": Color(0.44, 0.5, 0.56),
        "ambient_energy": 0.6
    }
}

func transition_to(target: WeatherState, duration: float = 180.0) -> void:
    # Transición gradual en `duration` segundos reales
    transition_target = target
    # ... lógica de interpolación
```

### 13.4 Sistema de Diálogos — Estructura de Datos

```gdscript
# Formato de un nodo de diálogo en JSON (cargado como Resource)
# res://resources/dialogues/nelida.json

{
    "id": "nelida_nivel_1",
    "speaker": "Nélida",
    "conditions": [],
    "lines": [
        {
            "text": "¡Uma! ¡Qué bueno verte por acá! Hace rato que no bajabas.",
            "voice_clip": "nelida_01.ogg"  # Opcional
        },
        {
            "text": "No, no te digo por nada. Es que te veía desde acá a veces, en la ventana.",
            "voice_clip": null
        }
    ],
    "player_choices": [
        {
            "text": "No sé todavía.",
            "next_id": "nelida_respuesta_a",
            "sets_flag": null
        },
        {
            "text": "A dar una vuelta.",
            "next_id": "nelida_respuesta_b",
            "sets_flag": null
        },
        {
            "text": "(silencio)",
            "next_id": "nelida_respuesta_c",
            "sets_flag": null
        }
    ],
    "on_complete": {
        "set_flag": "nelida_nivel",
        "value": 1
    }
}
```

### 13.5 Sistema de Inventario

```gdscript
# Uma.gd — fragmento del inventario

class_name Uma extends CharacterBody2D

const MAX_ITEMS = 6

var inventory: Array = []
var has_hat: bool = true  # El sombrero es especial, no va en el inventario
var notebook: Resource  # Siempre presente, no ocupa slot

func pick_up_item(item: GameItem) -> bool:
    if inventory.size() >= MAX_ITEMS:
        return false
    inventory.append(item)
    _update_item_description(item)
    return true

func _update_item_description(item: GameItem) -> void:
    # Actualiza la descripción del item según flags actuales
    var context = GameFlags.flags
    item.update_description(context)

func use_item_on_activator(item: GameItem, activator_id: String) -> void:
    match activator_id:
        "ranura_disco":
            if item.id == "disco_bardi":
                _activate_music_fragment()
                GameFlags.set_flag("activó_disco_zona3", true)
        "gancho_sombrero":
            if item.id == "sombrero" or has_hat:
                has_hat = false
                GameFlags.set_flag("dejó_sombrero", true)
                _trigger_hat_off_sequence()
        "marco_vacio":
            if item.id == "foto_polaroid":
                _display_photo_in_frame()
                GameFlags.set_flag("colocó_polaroid", true)
```

### 13.6 Triggers Narrativos por Zona

Cada zona tiene exactamente **3 triggers**: 1 obligatorio + 2 opcionales.

| Zona | Trigger Obligatorio | Opcional A | Opcional B |
|---|---|---|---|
| Casa | Cualquier detonante (salir) | Conversación completa con Tomás | Exploración del patio + nota Héctor |
| Barrio | Recoger fragmento de disco | Conversación nivel 2+ con Nélida | Conversación con Don Mario |
| Plaza | Recoger disco de vinilo | Dibujar en el banco | Conversación nivel 3 con Valentina |
| Zona Interior | Escuchar voz de Elena | Activar los 3 activadores | Encontrar el espejo de objetos |
| Antesala | Leer segunda nota | Comparar las dos notas | Dibujar el patio |
| Final | Poner el disco en la radio | Escuchar la canción completa sin moverse | Explorar el espacio durante la canción |

---

## APÉNDICE A — LÍNEA DE TIEMPO COMPLETA DEL DÍA

```
09:15 — Tomás tuvo el sueño. Le contó a Uma antes del desayuno.
09:30 — Elena hizo mate. Héctor ya no estaba (se fue a trabajar antes).
09:30 — El jugador toma control. Prólogo comienza.
10:00 — Uma sale. Pablo llegó al barrio de Uma hace veinte minutos.
         (Pablo visitó a su amigo, ya está volviendo caminando.)
10:10 — Pablo pasa por la cuadra 4 y ve el edificio de Bardi.
         (El fragmento de tapa de disco lo dejó él, hace una semana.)
10:20 — Uma está en el barrio. Pablo está en la plaza (sin que Uma lo sepa).
10:45 — Uma llega a la plaza. Pablo ya no está: se metió por el pasaje
         que lleva a la Zona Interior.
11:00 — Empieza a nublarse.
11:30 — Llovizna.
12:00 — Uma entra a la Zona Interior. Pablo está en la Antesala.
12:30 — Uma llega a la Antesala. Pablo ya pasó.
13:00 — Uma entra al Espacio Final. Pablo está ahí.
         No llegó antes que ella. Llegaron casi al mismo tiempo.
         Ninguno lo organizó.
```

---

## APÉNDICE B — LO QUE PABLO SABE Y UMA NO SABE

*(Este apéndice es para el equipo de diseño. No aparece en el juego de forma directa.)*

1. Pablo vive a cinco cuadras del barrio de Uma. No lo sabe ella.
2. Pablo dejó el fragmento de tapa de disco hace una semana, caminando. Lo reconoció en el suelo, lo miró y lo dejó. No sabe por qué.
3. Pablo escuchó la melodía de Bardi en la radio comunitaria donde trabaja. La pusieron sin aviso. Se quedó a escucharla completa.
4. Pablo pasó por el barrio de Uma varias veces en los últimos meses. Nunca tocó el timbre porque no tenía razón concreta para hacerlo.
5. Pablo vio a Uma esta mañana desde lejos. La reconoció. No la llamó porque no sabía si ella quería que la llamaran.
6. El disco en la fuente de la plaza: Pablo lo dejó esta mañana antes de ir a lo de su amigo. Compró el vinilo en un mercado hace tres semanas, teniendo a Uma en mente. No lo usó.

---

## APÉNDICE C — PREGUNTAS QUE EL JUEGO DEJA SIN RESPONDER

*(Intencionalmente.)*

- ¿Quiénes son las dos personas de la foto polaroid?
- ¿La tienda cerrada tiene dueño? ¿Volvió?
- ¿La página del cuaderno la puso Tomás? ¿O alguien más?
- ¿Las dos notas de Héctor son del mismo momento?
- ¿La radio de la Zona Interior funciona sin enchufarse porque el espacio es simbólico o porque es un viejo que sí funciona a batería?
- ¿Celso Bardi sigue vivo?
- ¿Uma va a volver a salir mañana?

---

*HUMITA — Documento de Diseño Extendido v2.0*
*Última revisión: Documento completo*

*"No es lo mismo llegar que animarse a seguir."*