# JavaScript y el Navegador

{{quote {author: "Tim Berners-Lee", title: "The World Wide Web: A very short personal history", chapter: true}

El sueño detrás de la Web es el de un espacio común de información
en el cual nos comunicamos compartiendo información.
Su universalidad es esencial: el echo de que un enlace pueda apuntar a cualquier cosa,
ya sea personal, local o global, ya sea un borrador o este muy pulido.

quote}}

{{index "Berners-Lee, Tim", "World Wide Web", HTTP, [JavaScript, "history of"], "World Wide Web"}}

{{figure {url: "img/chapter_picture_13.jpg", alt: "Picture of a telephone switchboard", chapter: "framed"}}}

Los próximos capítulos del libro hablarán del navegador web. Sin
((navegador))es web, no existiría JavaScript. Incluso, si existieran,
nadie le habría puesto atención.

{{index decentralization, compatibility}}

La tecnología web ha estado descentralizada desde el principio,
no sólo técnicamente, también en la forma en que ha evolucionado.
Distintos proveedores de nagevadores han agregado nueva funcionalidad
adhoc y algunas veces ha sido de una forma deficiente, y entonces,
termina siendo adoptada por otros-y finalmente es aceptada como
((estándar)).

Esta es a la vez una bendición y una maldición. Por un lado, empodera el no tener
un control central, sino que es mejorado por distintas partes trabajando
en ((colaboración)) (o a veces hostilidad abierta). Por otro lado, la
forma aleatoria en la que la web fue desarrollada significa que el sistema
resultante no es exactamente un ejemplo de ((consistencia)) interna. Algunas
partes son francamente confusas y pobremente concebidas.

## Redes e Internet

Las ((red))es de computadoras han existido de los 50s. Si pones cables
entre dos o más computadoras y les permites enviar y recibir información
a través de esos cables, puedes hacer todo tipo de cosas increíbles.

Y si conectar dos computadoras en el mismo edificio nos permite hacer
cosas increíbles, conectar computadoras alrededor del mundo debería ser
incluso mejor. La tecnología para iniciar la implementación de esta visión
fue desarrollada en los 80s, y la red resultante es llamada _((Internet))_.
Ha cumplido su promesa.

Una computadora puede usar esta red para mandar bits a otra computadora. Para
cualquier ((comunicación)) efectiva, las computadoras en ambos lados deben
saber qué se supone que representan los bits. El significado de cualquier
secuencia de bits depende enteramente del tipo de cosas que se está tratando
de expresar y del mecanismo de ((codificación)) utilizado.

{{index [network, protocol]}}

Un _((protocolo)) de red_ describe un estilo de comunicación sobre una ((red)).
Hay protocolos para mandar correos electrónicos, para recibir correos electrónicos,
para compartir archivos, e incluso para controlar computadoras que resultan estar
infectadas por software malicioso.

{{indexsee "Hypertext Transfer Protocol", HTTP}}

Por ejemplo, el Protocolo de Transferencia de Hipertexto
(((HTPP del inglés _Hypertext Transfer Protocol_))) es un protocolo
para obtener ((recurso))s nombrados (fragmentos de información, como páginas web
o imágenes). Especifica que el lado que hace la solicitud debe iniciar con
una linea como esta, indicando el recurso y la versión del protocolo
que se intenta usar:

```{lang: "text/plain"}
GET /index.html HTTP/1.1
```

Existen otras reglas acerca de cómo el solicitante puede incluir más
información en la ((solicitud)) y la forma en qué el otro lado, que
devuelve el recurso, empaca el contenido. Examinaremos HTTP en más detalle
en el capítulo [Chapter ?](http).

{{index layering, stream, ordering}}

La mayoría de los protocolos están construidos sobre otros protocolos. HTTP
utiliza la red como un dispositivo en el que pones bits y llegan a su destino
correcto en el orden correcto. Como vimos en el capítulo [Chapter ?](async),
asegurar eso ya es un problema muy complicado.

{{index TCP}}

{{indexsee "Transmission Control Protocol", TCP}}

El _Protocolo de Control de Transmisión_ (TCP del inglés Transmission
Control Protocol) es un ((protocolo)) que se encarga de este problema.
Todos los dispositivos conectados a internet lo "hablan", y la mayor
parte de la comunicación en ((internet)) está construida encima de él.

{{index "listening (TCP)"}}

Una ((conexión)) TCP funciona así: un computadora debe estar esperando,
o _escuchando_, a que otras computadoras inicien a hablarle. Para poder
escuchar diferentes tipos comunicaciones a la vez en una sola computadora,
cada oyente tiene un número (llamado ((_puerto_))) asociado a él. La
mayoria de los ((protocolo))s especifican qué puerto deben usar por
defecto. Por ejemplo, cuando queremos enviar un correo electrónico
utilizando el protocolo ((SMTP)), la máquina a través de la cual la
enviamos esté escuchando en el puerto 25.

Otra computadora pue establecer una ((conexion)) conectandose a la
ocmputadora destino usando el puerto correcto. Si la computadora destino
puede ser alcanzada y está escuchando en ee puerto, la conexión se
crea de forma exitosa. La computadora oyente es llamada el _((servidor))_,
y la computadora que se conecta es llamado el _((cliente))_.

{{index [abtraction, "of the network"]}}

Esa conexión actua como un una tubería de dos direcciones por la cual
los bits pueden fluir, las computadoras en ambos extremos pueden poner
información en esa tubería. Una vez que los bits son transmitidos de forma
exitosa, pueden ser leidos nuevamente por la computadora en el otro lado.
Este es un modelo conveniente. Podrías decir que ((TCP)) provee una
abstracción de la red.

{{id web}}

## La Web

La ((Red Mundial)) (en inglés World Wide Web, no confundir con ((Internet)))
es un conjunto de ((protocolo))s y formatos que nos permiten visitar
páginas web en un navegador. La parte "Web" del nombre se refiere al
hecho de que esas páginas pueden enlazarse fácilmente unas con otras,
conectandose a una gran ((red)) a entre la cual los usuarios pueden
moverse.

Para llegar a ser parte de la Web, todo lo que tienes que hacer es
conectar una computadora a ((Internet)) y ponerla a escuchar en el
puerto 80 con el protocolo ((HTTP)) para que otras computadoras puedan
pedirle documentos.

{{index URL}}

{{indexsee "Uniform Resource Locator", URL}}

Cada ((documento)) en la Web es nombrado por un _Localizador Uniforme
de Recursos_ (URL del inglés Uniform Resource Locator), y se ve así:

```{lang: null}
  http://eloquentjavascript.net/13_browser.html
 |      |                      |               |
 protocolo      servidor             ruta
```

{{index HTTPS}}

La primera sección nos dice que ésta URL utiliza el ((protocolo)) HTTP
(La versión ecriptada sería _https://_). Posteriormente sigue la sección
que identifica a qué ((servidor)) estamos solicitado el documento. La
última sección es una cadena de ruta que identifica el documento especifico
(o _((recurso))_) que nos interesa.

Las computadoras conectadas a internet obtienen una _((dirección IP))_, que
es un número que puede utilizado para enviar mensajes a esa computadora, ese
número se ve así `149.210.142.219` o así `2001:4860:4860::8888`. Pero
números casi aleatorios son dificiles de recordar y es raro para escribirlos,
así que en lugar de eso puedes registrar un _nombre de ((dominio))_ para
un una dirección específica o un conjunto de direcciones. Yo registré
_eloquentjavascript.net_ para apuntar a una dirección IP de una computadora
que controlo y así puedo usar ese nombre de dominio para entregar páginas
web.

{{index browser}}

Si escribes esa URL en la ((barra de dirección)) del navegador, el navegador
intentará obtener y mostrar el document en esa URL. Primero, el navegador
tiene que encontrar la dirección a la que _eloquentjavascript.net_ se
refiere. Entonces, utilizando el protocolo ((HTTP)), hará una conexión
al servidor en esa dirección y solicitará el recurso _/13_browser.html_.
Si todo sale bien, el servidor envía de vuelta un documento, que el
navegador muestra en la pantalla.

## HTML

{{index HTML}}

{{indexsee "Hypertext Markup Language", HTML}}

HTML, que significa _Lenguaje de Marcado de Hipertexto_ (en inglés
Hypertext Markup Language), es el formato de documento utilizado
para páginas web. Un documento HTML contiene ((texto)), así como
((etiqueta))s que dan estructura al texto, describiendo elementos
como enlaces, párrafos y encabezados.

Un pequeño documento HTML puede verse así:

```{lang: "text/html"}
<!doctype html>
<html>
  <head>
    <title>Mi página de inicio</title>
  </head>
  <body>
    <h1>Mi página de inicio</h1>
    <p>Hola, mi nombre es Marijn y esta es mi página de inicio.</p>
    <p>También escribí un libro! Léelo
      <a href="http://eloquentjavascript.net">aquí</a>.</p>
  </body>
</html>
```

{{if book

Así es como el documento se vería en el navegador:

{{figure {url: "img/home-page.png", alt: "My home page",width: "6.3cm"}}}

if}}

{{index [HTML, notation]}}

Las etiquetas, encerradas entre ((paréntesis angulares)) (`<` y `>`,
los símbolos para _Menor qué_ y _Mayor qué_), proveen información acerca
de la ((estructura)) del documento. El otro ((texto)) es sólo texto
plano.

{{index doctype, version}}

El documento inicia con `<!doctype html>`, eso le indica al navegador
que interprete la página como HTML _moderno_, no como distintos lenguajes
que fueron utilizados en el pasado.

{{index "head (HTML tag)", "body (HTML tag)", "title (HTML tag)", "h1 (HTML tag)", "p (HTML tag)"}}

Los documentos HTML tienen un encabezado y un cuerpo. El encabezado
contiene información _acerca del_ documento, y el cuerpo contiene
el documento en sí. En este caso, el encabezado declara que el título
del documento es "Mi página de inicio" y que está utilizando la
codificación UTF-8, que es una forma de codificar texto Unicode como
información binaria. El cuerpo del documento contiene un encabezado
(`<h1>`, que significa "encabezado 1", `<h2>` a `<h6>` produce
sub-encabezados) y dos ((párrafo))s (`<p>`).

{{index "href attribute", "a (HTML tag)"}}

Las etiquetas vienen en distintas formas. Algunos ((elemento))s, como el cuerpo,
un párrafo, o un enlace, se inicia por una _((etiqueta de apertura))_ como
`<p>` y finaliza con una _((etiqueta de cierre))_ como `</p>`. Algunas
etiquetas de apertura, como la utilizada para los enlaces, (`<a>`),
contienen información extra en forma de pares `name="value"`. Estos
se llaman _((atributos))_. En este caso, el destino del enlace
está indicado con `href="http://eloquentjavascript.net"`, donde
`href` significa "referencia de hipertexto".

{{index "src attribute", "self-closing tag", "img (HTML tag)"}}

Algunos tipos de ((etiqueta))s no encierran nada, y por lo tanto no
necesitan ser cerradas. La etiqueta meta `<meta charset="utf-8">`
es un ejemplo de esto.

{{index [escaping, "in HTML"]}}

Para poder incluir paréntesis angulares en el texto de un documento,
aunque tengan un significado especial en HTML, otra forma de notación
especial tiene que ser introducida. Un paréntesis angular de apertura
es escrito como `&lt;` (menor qué del inglés less than), y un
paréntesis angular de cierre es escrito como `&gt;` (mayor qué del
inglés greater than). En HTML, un ampersand (`&`) seguido por un nombre
o código de caracter, y un punto y coma (`;`) es llamado una _((entidad))_
y será reemplazada por el caracter que codifica.

{{index ["backslash character", "in strings"], "ampersand character", "double-quote character"}}

Esto es análogo a la forma en que las diagonales invertidas son
utilizadas en las cadenas en JavaScript. Ya que este mecanismo le
da a los carácteres ampersand un significado especial, también,
tienen que ser escapadas como `&amp;`. Dentro de los valores de los
atributos, que son encerrados en comillas dobles, `&quot;` pueden ser
utilizados para insertar un carácter de comillas.

{{index "error tolerance", parsing}}

HTML es analizado en una forma tolerante a errores. Cuando las etiquetas
que deberían estar ahí no están, el navegador las reconstruye. La forma
en que esto se realiza está estandarizado, y puedes confiar en que
todos los navegadores modernos lo realizarán de la misma manera.

El siguiente documento será tratado identicamente al mostrado anteriormente:

```{lang: "text/html"}
<!doctype html>

<meta charset=utf-8>
<title>Mi página de inicios</title>

<h1>Mi página de inicio</h1>
<p>Hola, mi nombre es Marijn y esta es mi página de inicio.
<p>También escribí un libro! Léelo
  <a href=http://eloquentjavascript.net>aquí</a>.
```

{{index "title (HTML tag)", "head (HTML tag)", "body (HTML tag)", "html (HTML tag)"}}

Las etiquetas `<html>`, `<head>` y `<body>` fueron removidas completamente.
El navegador sabe que `<meta>` y `<title>` pertenecen al encabezado y
`<h1>` significa que el cuerpo del documento ha iniciado. Además,
los párrafos no se están cerrando explícitamente ya que el abrir un
nuevo párrafo o finalizar el documento los cierra implicitamente.
Las comillas alrededor de los valores de los atributos tampoco están.

Este libro generalmente omitirá las etiquetas `<html>`, `<head>` y `<body>`
de los ejemplos para mantenerlos cortos. Pero cerraré etiquetas e incluiré
comillas alrededor de los atributos.

{{index browser}}

Usualmente omitiré las declaraciones ((doctype)) y `charset`. Eso
no significa que tu tienes que hacer lo mismo. Los navegadores
puede llegar a hacer cosas ridiculas cuando las omites. Siempre
considera que las declaraciones ((doctype)) y `charset` están ahí
implicitamente en los ejemplos, incluso cuando no se muestran en el
texto.

{{id script_tag}}

## HTML y JavaScript

{{index [JavaScript, "in HTML"], "script (HTML tag)"}}

En el contexto de este libro, la etiqueta HTML más importante es
`<script>`. Esta etiqueta nos permite incluir un fragmento de JavaScript
en un documento.

```{lang: "text/html"}
<h1>Testing alert</h1>
<script>alert("hello!");</script>
```

{{index "alert function", timeline}}

Ese comando será ejecutado tan pronto como su etiqueta `<script>` es
encontrada mientras el navegador lee el HTML. Esta página mostrará un
diálogo en cuanto sea abierta, la función `alert` se parece a `prompt`
en que muestra una pequeña ventana, pero sólo muestra un mensaje sin
esperar datos de entrada.

{{index "src attribute"}}

Incluir grandes programas directamente en un documento HTML
es muchas veces impráctico. A la etiqueta `<script>` se le puede proveer
un atributo `src` para obtener un archivo de script (un archivo de texto
que contiene un fragmento de códido JavaScript) de una URL.

```{lang: "text/html"}
<h1>Testing alert</h1>
<script src="code/hello.js"></script>
```

El archivo _code/hello.js_ incluido aquí contiene el mismo programa,
`alert("hello!")`. Cuando una página HTML hace referencia a otras URLs como
parte de si misma, por ejemplo, un archivo de imagen o un script, los navegadores
los recuperarán inmediatamente y los incluirán en la página.

{{index "script (HTML tag)", "closing tag"}}

Una etiqueta de script siempre debe cerrarse con `</script>`, incluso si se
refiere a un archivo de script y no contiene código en su interior. Si olvidas
cerrarla, el resto de la página será interpretada como parte del script.

{{index "relative path", dependency}}

Puedes cargar ((módulos ES)) (ver [Chapter ?](modules#es)) en el navegador
si le provees a tu etiqueta script un atributo `type="module"`. Esos módulos
pueden depender de otros módulos usando ((URL))s relativas a si mismos como
nombres de módulos en declaraciones `import`.

{{index "button (HTML tag)", "onclick attribute"}}

Algunos atributos también pueden contener un programa JavaScript. La etiqueta
`<button>` mostrada a continuación (que muestra un botón) tiene un atributo
`onclick`. El valor del atributo será ejecutado siempre que el botón sea presionado.

```{lang: "text/html"}
<button onclick="alert('Boom!');">NO PRESIONAR</button>
```

{{index "single-quote character", [escaping, "in HTML"]}}

Nota que tuve que usar comillas simples para la cadena en el atributo `onclick`
porque las comillas dobles ya están siendo utilizadas para encerrar el atributo
completo. También pude haber usado `&quot;`.

## En la caja de arena

{{index "malicious script", "World Wide Web", browser, website, security}}

Ejecutar programas descargados de ((Internet)) es pontencialmente peligroso.
No conoces mucho de las personas detrás de la mayoría de los sitios que visitas,
y no necesariamente quieren hacer el bien. Ejecutar programas de gente que no
quiere hacer el bien es como tu computadora se infecta de ((virus)), te roban
la información y compromenten tus cuentas.

Aunque la atracción de la Web es que puedes navegar sin necesariamente ((confiar))
en todas las páginas que visitas. Ésa es la razón por la cual los navegadores
limitan en gran medida las cosas que un programa JavaScript puede hacer: no puede
revisar los archivos en tu computadora o modificar nada relacionado a la página web
en la que se inscrustó.

{{index isolation}}

Aislar un entorno de programación en ésta forma es llamado _((sandbox))ing_,
la idea es que el programa pueda ejecutarse sin daños en una caja de arena.
Pero deberías imaginar que este tipo particular de caja de arena es como tener
los programas detrás de barras de acero y que no pueden salir.

la parte complicada del sandboxing es permitirle al programa suficientes permisos
para ser útil y a la vez restringirlo de hacer cualquier cosa peligrosa. Mucha
funcionalidad, como comunicarse con otros servidores o leer el contenido del
portapapeles puede ser problematico, son cosas que invaden la ((privacidad)).

{{index leak, exploit, security}}

De vez en cuando, alguien tiene una nueva forma de evitar las limitaciones de un
((navegador)) y hacer cosas dañinas, desde exponer información privada sin importancia
hasta tomar el control de toda la máquina sobre la que corre el navegador. Los
desarrolladores del navegador responden arreglando ese problema, y todo está bien
nuevamente, hasta que el próximo problema es descubierto, y tal vez se haga
público, en lugar de ser explotado de forma secreta por una agencia del gobierno
o ((mafia)).

## Compatibilidad y la guerra de los navegadores

{{index Microsoft, "World Wide Web"}}

En las etapas tempranas de la Web, un navegador llamado ((Mosaic)) dominó el mercado.
Después de unos cuantos años, la balanza se inclinó hacia ((Netscape)), el cual fue
suplantado después por el browser de Microsoft ((Internet Explorer)). En cualquier punto
en el que un único ((navegador)) fuera dominante, el fabricante de ese navegador se
sentía con derecho de inventar nuevas caracteristicas unilateralmente para la Web.
Ya que la mayoria de los usuarios usaban el navegador más popular, los ((sitios web))
simplemente empezarían a usar esas caracteristicas sin importarles los otros navegadores.

Ésta fue la edad oscura de la ((compatibilidad)), algunas veces llamada
_((guerra de navegadores))_. Los desarrolladores Web tenían que lidiar con
dos o tres plataformas incompatibles. Para hacer las cosas peor, los navegadores
usados alrededor del 2003 estaban llenos de ((defectos)), y  por supuesto,
los defectos eran distintos para cada ((navegador)). La vida era dura para los
que escribian páginas Web.

{{index Apple, "Internet Explorer", Mozilla}}

Mozilla ((Firefox)), una derivación sin fines de lucro de ((Netscape)), retó
la posición de Internet Explorer a finales de los 2000. Ya que ((Microsoft))
no estaba interesado en mantenerse competitivo en ese momento, Firefox tomó
una gran parte del mercado de Internet Explorer. Más o menos al mismo tiempo,
((Google)) introdujo su navegador ((Chrome)), el navegador ((Safari)) de Apple
también ganó popularidad, llevando a una situación en la que existieron cuatro
grandes competidores, en lugar de uno.

{{index compatibility}}

Los nuevos competidores tuvieron una actitud más seria hacia los ((estándar))es y
mejores prácticas de ((ingeniería)), dándonos menos incompatibilidad y menos
defectos. Microsoft, al vez que su parte del mercado caerse, le dio la vuelta y
adoptó esas actitudes en su navegador Edge, el cual reemplaza a Internet Explorer.
Si tú estás empezando a aprender desarrollo web, considérate afortunado. Las
últimas versiones de los navegadores se comportan uniformemente bien y tienen
relativamente menos defectos.