---
name: subatomizer
description: >-
  Atomiza un artículo ya publicado en cinco notas de Substack escritas con la voz del propio
  autor, destilada del artículo, y ancladas a fragmentos exactos del texto. Estudia cómo
  escribe quien lo firmó y qué material da el texto, escribe nueve candidatas, comprueba una
  por una con Grep que sus cifras y sus citas estén de verdad en el original, y entrega las
  cinco mejores —hasta dos de ellas marcadas como lectura del texto y no como el texto— más las
  cuatro descartadas con su motivo. Úsalo cuando alguien pase la URL de
  un artículo o de un post de Substack y quiera sacarle notas; cuando pida repartir, trocear,
  atomizar o reaprovechar algo que ya publicó; cuando quiera contenido para Notes a partir de
  su newsletter; o cuando pregunte qué notas salen de un texto suyo. Vale con Substack y con
  cualquier otra URL. La señal es que HAY un artículo del que anclar: si lo que hay es una idea
  suelta y ningún texto previo, eso es escribir una nota desde cero y no es este skill. Lo que
  no está en el artículo, no sale en la nota.
---

# subatomizer — cinco notas que el artículo ya contiene

Un modelo atomizando un artículo hace dos destrozos. Redondea la cifra, retoca la cita y se
inventa el ejemplo. Y escribe las notas con su propia voz de modelo, que suena a cualquiera.
Quien publica eso lo hace con su nombre encima.

Contra lo primero, una comprobación mecánica que no se salta: **el artículo se guarda en un
fichero y cada cifra y cada cita se buscan ahí con Grep**. Contra lo segundo, el propio
artículo: lo escribió él, así que ahí está su voz.

Cinco fases: **traer el texto, estudiarlo, escribir nueve, verificarlas, elegir cinco.**

---

## 1. Traer el artículo y guardarlo

### Primero, con `agent-browser`

Invoca el skill **agent-browser** para abrir la URL y sacar el texto del artículo tal cual está
en la página. Es la vía buena por tres motivos: devuelve el texto **literal** —que es lo que
permite comprobar una cita de verdad—, funciona con páginas montadas con JavaScript, y usa la
sesión del navegador, así que un post de pago propio se lee entero en vez de cortarse en el
muro.

En Substack el cuerpo del artículo vive en `.available-content`, y eso son dos órdenes:

```bash
agent-browser open "<url>"
agent-browser get text ".available-content"
```

Sale el texto limpio, sin menú, sin barra lateral y sin el pie de suscripción. En otros sitios,
prueba `article` y, si no, mira el `snapshot` para ver dónde está el cuerpo. Lo que **no** vale
es quedarse con la página entera: el menú y los formularios meten palabras que no escribió él y
falsean tanto el recuento como el perfil de voz.

### Si no hay navegador, WebFetch

Pidiéndolo literal. El prompt importa: si pides un resumen te devuelve un resumen, y sobre un
resumen no se puede comprobar una cita.

> Devuelve el texto íntegro del artículo, literal y sin resumir: título, subtítulo y todos los
> párrafos en orden, con sus cifras, sus nombres y sus frases entrecomilladas tal cual están.
> No interpretes, no acortes, no reformules.

**Y dilo**: en este modo el texto llega pasado por otra herramienta, así que la fase 4 es tan
buena como fiel sea lo que volvió. Si una cita no aparece, puede ser que te la inventaras o que
te la reformularan. En la duda, la nota cae.

### Guardarlo, siempre

```
~/.claude/subatomizer/<fecha>-<slug>/articulo.txt
```

Texto plano, el artículo entero, sin tocar. **Este fichero es el patrón de medida**: sin él no
hay Grep, y sin Grep la fase 4 es el modelo dándose la razón a sí mismo. Si la sesión se cae,
también es lo único que se salva.

### Antes de seguir

Tres cosas que, si pasan, se dicen en voz alta:

- **Menos de 300 palabras**: de ahí no salen cinco notas ancladas. Se dice y se para.
- **Corte de pago** («sigue leyendo con una suscripción»): llegó truncado. Se avisa **antes** de
  las notas, no al final, y se trabaja con lo visible.
- **Solo menú y cuatro frases**: no se sacó el artículo. Pídele al usuario que lo pegue.

---

## 2. Estudiar el artículo

Dos cosas salen de leerlo, y ninguna se puede saltar: **qué material da** y **cómo escribe
quien lo firmó**. Sin lo primero, las nueve notas salen del mismo tercio del texto —siempre los
dos primeros párrafos, que es lo que se recuerda—. Sin lo segundo, suenan a modelo.

### El inventario

Rellena esta tabla leyendo, y **copiando literal**: aquí no se interpreta nada todavía.

| Qué | Cómo se recoge |
|---|---|
| **Tesis** | Una frase: qué defiende el artículo |
| **Ideas** | De tres a seis, una línea cada una |
| **Cifras** | Cada número, con la frase entera donde aparece |
| **Frases que aguantan solas** | Las citables, **literales**, con su puntuación |
| **Escenas** | Momentos concretos: qué pasó, cuándo, a quién |
| **Nombres** | Personas, productos y trabajos de otros que el texto cita |
| **Tensiones** | Lo que el artículo deja abierto, lo que se contradice, lo que incomoda |
| **Instrucciones** | Pasos o consejos que el lector puede ejecutar |

Una casilla vacía es información, no un hueco que rellenar: si no hay cifras, no habrá nota de
cifra. **Y si el inventario sale casi vacío —un artículo de enlaces, una nota de prensa, una
introducción— dilo antes de escribir**: ese texto da para dos o tres notas, no para cinco, y es
mejor saberlo ahora que fingirlo después.

### El perfil de voz

Seis rasgos, y **cada uno con su prueba citada del artículo**. Un rasgo sin cita es una
impresión tuya, y las impresiones producen imitaciones de imitación:

| Rasgo | Qué mirar |
|---|---|
| **Persona y registro** | ¿Tutea, trata de usted, escribe en primera persona, en plural? ¿Coloquial o formal? Cita la frase que lo demuestra |
| **Largo de frase** | ¿Frases cortas y secas, o largas con subordinadas? Cuenta un par de ellas |
| **Léxico propio** | De cinco a diez palabras o expresiones suyas, **literales**. Las que usaría él y no usaría otro |
| **Cómo abre** | ¿Entra por una escena, por un dato, por una afirmación, por una pregunta? |
| **Cómo remata** | ¿Cierra con una acción, con una vuelta de tuerca, en seco, con una pregunta? |
| **Puntuación y forma** | ¿Usa guiones largos, paréntesis, cursivas, listas, negritas? ¿Párrafos de una línea? |

Y una séptima cosa, que es la que más se nota cuando falta: **la lista anti-imitación**. Qué
**no** hace nunca en el artículo, punto por punto. Si no usa emojis, no hay emojis. Si no hace
preguntas retóricas, no hay preguntas retóricas. Si no usa superlativos, ninguna nota lleva
«increíble». Esta lista se usa dos veces: al escribir y al puntuar.

**El único ajuste permitido.** Un artículo se lee sentado y una nota bajando con el pulgar. Así
que se conserva el registro, el léxico y la puntuación, y **se acorta la frase**. Nada más. Si
de un ensayista sale una nota que suena a community manager, la voz se perdió por el camino.

Enseña el perfil al usuario en cinco o seis líneas antes de escribir las notas. Es su voz: si
te has equivocado, lo ve en dos segundos, y corregirlo ahí cuesta mucho menos que corregir
nueve notas.

---

## 3. Escribir nueve candidatas

Nueve para tener de dónde elegir. Se entregan cinco.

Cada candidata declara cinco cosas, y el `ancla` sostiene todo:

| Campo | Qué es |
|---|---|
| `formato` | Uno de los nueve de abajo |
| `objetivo` | Uno solo: enseñar, convencer, conectar, conversar o dar crédito |
| `tipo` | `anclada` o `inferida` — ver más abajo |
| `texto` | La nota entera, tal como se publicaría |
| `ancla` | El fragmento **exacto** del artículo del que nace, copiado literal |

### Los nueve formatos

| Formato | Qué es | Qué necesita del inventario |
|---|---|---|
| **arreglo** | Un problema concreto y su solución en sesenta segundos | Una instrucción ejecutable hoy |
| **historia-dato** | Un momento real pegado al número que lo mide | Una escena **y** una cifra. Con una sola, no es esto |
| **contraria** | Desmontar algo que el público da por sentado | Una tensión, y que la sostengas en los comentarios |
| **confesion** | Lo que salió mal, lo que costó, lo que da apuro | Una escena que le deje en mal lugar |
| **tras-bambalinas** | Cómo se hizo algo por dentro, con sus números | Proceso más cifras |
| **generosidad** | Señalar el trabajo de otro que el artículo cita | Un nombre. Lo que más convierte y lo que menos se usa |
| **pregunta** | Una pregunta genuina sobre lo que el texto deja abierto | Una tensión sin resolver |
| **sentencia** | Una frase del artículo que aguanta sola, comentada | Una frase citable. El formato más difícil de fingir |
| **lista-corta** | De tres a cinco puntos, una línea cada uno | Ideas de verdad enumerables |

Uno por candidata. **Si el inventario no da material para un formato, esa candidata no se
escribe**: ocho honestas valen más que nueve con una inventada.

### Anatomía

1. **Gancho** — primera línea, máximo 120 caracteres, con algo concreto dentro: una cifra, un
   nombre, un problema que el lector reconozca. Si podría encabezar cualquier otra nota, no es
   un gancho. Ábrelo **como abre él**.
2. **Sustancia** — dos a cinco líneas: la historia, el dato o el arreglo.
3. **Cierre** — una acción que el lector pueda probar, o una pregunta que quieras que te
   respondan. **Como remata él.**

**50–250 palabras**, uno a cinco párrafos cortos, con blanco entre ellos.

### El ancla

Cada nota nace de un fragmento del artículo, y ese fragmento se copia **literal**: mismas
palabras, mismas cifras, misma puntuación. No es el tema de la nota ni su resumen; es el trozo
de texto del que sale. «Habla del alcance» no es un ancla; «el alcance cayó un 37% en marzo»
sí.

**Si no puedes señalar el fragmento, no hay nota.**

### Ancladas e inferidas

Una **anclada** dice lo que el artículo dice, con otra forma. Una **inferida** no inventa ni un
dato, pero afirma algo que el artículo **no llega a decir**: la consecuencia, la objeción, el
límite, la pregunta abierta.

| Tipo de inferencia | Qué hace |
|---|---|
| **Consecuencia** | «Si pasó esto, entonces aquello» — lo que se sigue de sus propios datos |
| **Objeción** | La pregunta incómoda que el artículo no responde |
| **Límite** | Dónde deja de valer lo que defiende: con qué tamaño, con qué público, en qué caso |
| **Pregunta abierta** | Lo que habría que saber y no se sabe |

Son las que generan respuestas: una nota anclada se lee y se asiente; una inferida se lee y
alguien contesta. Por eso entran. Y por eso llevan tope.

**La línea que no se cruza.** Una inferida saca una **conclusión** nueva, nunca un **hecho**
nuevo. «Si una de dieciséis hizo el trabajo, el volumen no es la palanca» es una lectura de sus
cifras y vale. «Como le pasa al 80% de los escritores» es un dato inventado y cae — da igual
que suene razonable, da igual que probablemente sea cierto. Si lo afirmas y no está en el
artículo, no se publica.

De las nueve candidatas, **hasta cuatro pueden ser inferidas**. De las cinco finales, **dos como
mucho**.

### Tres ejemplos: anclada, inferida, inventada

Del artículo:

> Publiqué 16 notas la semana pasada. Una me trajo 514 suscriptores y las otras quince no
> llegaron a veinte entre todas. No era la mejor escrita: era la única que contaba un número
> mío.

Salen tres notas distintas. La primera se entrega. La segunda se entrega etiquetada. La
tercera no se escribe nunca.

**1 · Anclada.** Dice lo que el artículo dice, con otra forma. Todo está en el texto.

`formato`: historia-dato · `objetivo`: enseñar · `tipo`: anclada
`ancla`: «Una me trajo 514 suscriptores y las otras quince no llegaron a veinte entre todas»

```
Publiqué 16 notas la semana pasada.

Una me trajo 514 suscriptores. Las otras quince, menos de veinte entre todas.

No era la mejor escrita. Era la única que contaba un número mío.
```

El gancho es una cifra suya y cabe en una línea, y la nota se entiende entera sin haber leído
el artículo. Cada palabra sale del fragmento: la fase 4 la pasa sin discusión.

**2 · Inferida.** No inventa ni un dato, pero afirma algo que el artículo no llega a decir.

`formato`: contraria · `objetivo`: convencer · `tipo`: inferida (consecuencia)
`ancla`: «Una me trajo 514 suscriptores y las otras quince no llegaron a veinte entre todas»

```
Publiqué 16 notas la semana pasada. Una trajo 514 suscriptores. Las quince restantes, menos
de veinte entre todas.

Si una de dieciséis hizo casi todo el trabajo, publicar más no es la palanca.

La palanca es tener números propios que contar. Y esos no salen de escribir: salen de hacer
cosas y medirlas.
```

Las cifras son suyas. La conclusión —«publicar más no es la palanca»— es una lectura: el
artículo la insinúa y no la afirma. Por eso se entrega **con la etiqueta puesta fuera del
bloque**, y por eso van dos como mucho entre las cinco.

Fíjate en «casi todo el trabajo» donde la tentación era escribir «el 96% del trabajo». El 96
es correcto —514 de unos 534— pero es una **cifra derivada**: no está en el artículo, así que
`cifras-reales` la tumba en la fase 4. Ver *Cifras derivadas*, aquí abajo.

**3 · Inventada.** Se saca un hecho de la nada. Prohibida siempre, y no hay discusión.

```
Publiqué más de 15 notas y una me trajo cientos de suscriptores, como le pasa al 80% de los
escritores que llevan más de un año en la plataforma.
```

Ese 80% no existe en ninguna parte. Y de paso redondea lo que el texto daba exacto: «más de
15» y «cientos» donde el artículo dice 16 y 514. Da igual que el porcentaje suene razonable y
da igual que probablemente sea cierto: si lo afirmas y no está en el artículo, no se publica.

**La diferencia entre la 2 y la 3 es la única que hay que tener clara**: la inferida saca una
**conclusión** nueva de hechos viejos; la inventada mete un **hecho** nuevo. La conclusión la
firma el autor y se discute en los comentarios. El hecho falso no se discute: se desmiente.

Y el dato inventado casi nunca viaja solo. La versión completa de esa tercera nota suele salir
así:

```
¿Sabías que una sola nota puede cambiar tu Substack? 🚀

Publiqué más de 15 notas y una me trajo cientos de suscriptores. La clave está en la
autenticidad.

Suscríbete para más consejos como este.
```

Cuatro fallos en seis líneas: la cifra redondeada, el emoji y la pregunta retórica —que el
autor no usa—, la generalidad hueca («autenticidad» no aparece en el artículo) y la captación
del final. Ninguno se ve si no se compara con el texto. Los cuatro caen en la fase 4.

### Cifras derivadas

Un número calculado a partir de las cifras del artículo —un porcentaje, una suma, una media,
un «X veces más»— **no está en el artículo, así que cae por `cifras-reales`**. Es la vía más
fina por la que se cuela un dato falso: nace de una cuenta correcta, no levanta sospecha, y si
la cuenta tenía una suposición dentro («menos de veinte» no es un número) el porcentaje se
publica como si fuera exacto.

La salida es escribir la relación sin el número: «una de dieciséis», «casi todo», «la quinta
parte». Dice lo mismo, es igual de contundente y sí está anclada.

Si el autor quiere el porcentaje, lo pone él al publicar. Es su cuenta y es su nombre.

### Prohibido, y por qué

- **Todo lo de la lista anti-imitación.** Va primero porque es lo que delata.
- **Enlaces dentro de la nota** — se lee como un canal de RSS y hunde el alcance.
- **«Suscríbete», «sígueme», «enlace en bio»** — se lee como manipulación y no convierte.
- **Emojis de flecha, «este truco», «no vas a creer»** — dan likes, no suscriptores.
- **Dos objetivos en la misma nota** — se diluyen y no cumple ninguno.
- **Redondear una cifra** — «más de 500» donde el texto dice 514 es peor nota y además falsa.
- **Retocar una cita «para que suene mejor»** — una coma cambiada sigue siendo una cita falsa, y
  es la que nadie comprueba.
- **Afirmar un hecho que no está en el artículo** — porcentajes del sector, lo que «suele
  pasar», lo que «hace la gente». Una inferida saca conclusiones, no datos.

### Las nueve juntas

Míralas en bloque antes de verificar: **nueve anclas distintas** —dos notas del mismo fragmento
son la misma nota escrita dos veces— y **como mucho dos candidatas por objetivo**.

---

## 4. Verificar con Grep

No a ojo. Releer una nota que te acabas de inventar no te dice que te la inventaste: te sigue
pareciendo bien. Se busca en `articulo.txt`, que para eso se guardó.

De cada nota salen tres listas: **su ancla**, **sus cifras** y **sus frases entrecomilladas**.
Cada elemento, una búsqueda.

**Cifras** — con límite de palabra, o «514» se daría por bueno porque el artículo diga «5140»:

```
Grep  pattern: \b514\b   path: articulo.txt
```

**Anclas y citas** — busca un trozo distintivo de cuatro a seis palabras **sin comillas, sin
guiones largos y sin puntuación final**. Es lo que evita que una cita correcta caiga por un
carácter tipográfico, que es el falso negativo más común:

```
Grep  pattern: me trajo 514 suscriptores   path: articulo.txt
```

**Con la herramienta Grep del agente, no con `grep` por consola.** No es manía: en Git Bash sobre
Windows, `grep` aborta con «core dumped» y el shell devuelve lo mismo que devolvería si no
hubiera encontrado nada. Se comprobó en una ejecución real y dio veinticuatro falsos «no está»
seguidos. Si te hubieras fiado, habrías tumbado nueve notas correctas.

De ahí la regla que va con ella: **cero resultados y herramienta rota no son lo mismo**. Antes
de dar por caída ninguna nota, busca un fragmento que sepas que está —el título del artículo
sirve—. Si ese tampoco aparece, lo que falla es la búsqueda, no las notas: se arregla y se
repite. Una tanda entera cayendo a la vez casi nunca significa que las escribieras mal.

Con la herramienta sana, cero resultados es una caída. No se interpreta, no se da el beneficio de la duda, no se «ajusta
la búsqueda hasta que salga».

### Ocho motivos de caída

| Fallo | Qué lo dispara |
|---|---|
| **ancla-existe** | El `ancla` no aparece en `articulo.txt` |
| **citas-literales** | Una frase entrecomillada de la nota no está literal en el artículo |
| **cifras-reales** | Un número con decimales, con %, con moneda o con unidad detrás —o cualquier entero mayor que diez— no está en el artículo. Incluye las **cifras derivadas**: el porcentaje o la suma que tú calculaste a partir de las suyas no está en el texto y cae igual |
| **hecho-nuevo** | La nota afirma como cierto algo comprobable que el artículo no dice: una estadística, lo que hace «la gente», lo que pasa «siempre». Se aplica igual a las ancladas y a las inferidas, y es el único freno de las segundas |
| **longitud** | Menos de 50 o más de 250 palabras |
| **gancho** | La primera línea pasa de 120 caracteres |
| **sin-enlaces** | Hay una URL o un dominio dentro de la nota |
| **sin-captación** | Aparece «suscríbete», «sígueme», «enlace en bio» o equivalente |

Siete de los ocho se comprueban con Grep. **`hecho-nuevo` no**: exige leer la nota preguntándose
«¿esto lo dice el artículo o lo estoy dando por sabido?». Es el único que depende de tu criterio,
así que aplícalo con dureza — en la duda, cae.

### Un aviso

**cifras-menores**: un entero de uno a diez que no está en el artículo. Casi siempre es
legítimo —«van 3 cosas que aprendí» cuenta los puntos de la propia nota—, así que no tumba
nada. Pero **si llega hasta la entrega se dice al usuario nombrando el número**: es por donde
se cuela un dato falso pequeño.

La nota que cae **se arregla y se vuelve a verificar**. No se pasa a la selección con fallos
pendientes, ni se entrega «avisando de que igual el dato no está».

---

## 5. Elegir cinco

Solo entre las que pasaron. Si una te gusta y falló, arréglala, verifícala y entonces compite.

### La rúbrica

Seis ejes, de 0 a 3. Máximo 18.

| Eje | 3 puntos | 0 puntos |
|---|---|---|
| **Voz** | No incumple **ni un punto** de la lista anti-imitación, y usa léxico suyo del perfil | Incumple dos o más, o no hay una sola palabra que sea suya |
| **Ancla** | Un hecho duro: cifra, nombre, frase literal | Una generalidad que saldría de cualquier artículo |
| **Autonomía** | Se entiende sin haber leído el artículo | Solo tiene sentido si ya lo leíste |
| **Gancho** | La primera línea dice algo concreto | Podría encabezar otra nota cualquiera |
| **Conversación** | Alguien tendría algo que responder | Solo cabe asentir |
| **Utilidad** | El lector se lleva algo hoy | El lector se lleva una impresión |

El eje de voz **se puntúa contando incumplimientos de la lista anti-imitación, no por
impresión**: las escribiste tú, así que tu impresión siempre te dará un 3. Y va primero a
propósito, porque una nota impecable que no suena a él no la va a publicar, y si la publica, le
resta.

### Dos restricciones, por encima de la puntuación

- **Cinco objetivos distintos** entre las cinco finales.
- **Como mucho dos del mismo formato.**
- **Como mucho dos inferidas.** Tres ancladas es el suelo: lo que entregas tiene que ser, en su
  mayor parte, lo que él ya escribió.

Una nota de 16 sobre 18 se queda fuera si su objetivo ya está cogido: entra la siguiente. Cinco
notas que hacen cinco cosas distintas reparten mejor que cinco buenas que hacen lo mismo.

Si el artículo no daba para inferir bien, salen cinco ancladas y no pasa nada. El tope es un
techo, no una cuota.

### Cómo se entrega

1. El aviso de truncado, si lo hubo.
2. **El perfil de voz**, en cinco o seis líneas.
3. Las cinco, **cada una en su propio bloque de código, lista para copiar y pegar**. Encima del
   bloque —nunca dentro— una línea con formato, objetivo y **por qué está esa y no otra**: el
   motivo, no el elogio («es la única con una cifra propia», no «muy potente»).

   Y si es **inferida**, se dice ahí mismo, en esa línea: *«inferida: esto es una lectura tuya
   del texto, no algo que el artículo diga»*. La etiqueta va fuera del bloque, nunca dentro —en
   la nota publicada quedaría ridícula—. Es para ti, que eres quien la firma, y por eso no se
   omite ni se suaviza aunque la nota te guste.

   Dentro del bloque va **solo el texto de la nota**, tal y como se publicaría: sin el título
   del formato, sin comillas que la envuelvan, sin numeración, sin asteriscos de negrita ni
   almohadillas de markdown —Substack Notes no los interpreta y se pegan como basura—, y con los
   saltos de línea en su sitio. Nada de meter las cinco en un bloque común: se copian de una en
   una, según se van publicando.
4. Las cuatro descartadas, una línea cada una con su motivo real («cayó por cita no literal»,
   «el ancla se repetía con la 3», «16 sobre 18 pero el objetivo ya estaba cogido»). Ahí es
   donde el autor rescata la que le duela perder.
5. Los avisos que quedaran vivos, nombrando el número.

El `ancla` no se enseña salvo que la pidan: es la costura, no el producto.

---

## Lo que no se hace

- **No se pregunta al usuario qué tono quiere.** Está en el artículo. Preguntarlo es admitir que
  no lo has leído.
- **No se rellena hasta cinco.** Si solo pasan cuatro, se entregan cuatro y se dice por qué. El
  número cinco no es un compromiso con el usuario; que ninguna nota lleve un dato falso, sí.
- **No se enseña una nota que no pasó la verificación**, ni «de muestra», ni «para que veas la
  idea».
- **No se escribe desde la memoria del artículo.** Si no se pudo traer y guardar, no hay notas:
  se dice.

---

## De dónde salen estas reglas

Los formatos, la longitud y la anatomía están destilados de tres fuentes que miden, consultadas
el 2026-09-14: `thrivewithcarrie.substack.com/p/substack-notes-strategy-2026` (los formatos que
convierten, y que un artículo de dos mil palabras da para tres a cinco notas),
`pubstacksuccess.substack.com/p/how-to-write-high-performing-substack-notes` (50–250 palabras,
uno a cinco párrafos) y `writebuildscale.substack.com/p/the-2026-substack-notes-playbook` (un
objetivo por nota).

Se deja fuera a conciencia el repertorio de curiosidad que esas mismas fuentes recomiendan.

Si quien lo usa tiene instalado **subnotes**, hay un salto más: su histórico de notas publicadas
dice qué formatos le funcionan **a él** con sus propios números, y avisa de si ya había dicho
esto. Aquí no se asume que esté.
