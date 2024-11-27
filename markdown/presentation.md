name: inverse
layout: true
class: center, middle, inverse
---
# Albor Micro-Frontends 

---
name: content
layout: false
## Contenido

- ¿Por qué nos interesa?

--

- ¿Cómo funciona?

--

- ¿Cómo se implementa?

--

- ¿Cómo no volver a la misma situación?

---
name: why-1
class: center, middle
# ¿Qué dolores sufrimos actualmente?

???

A veces inconvenientes grandes, a veces piedritas en el zapato, pero siempre hay cosas para mejorar que nos permitan concentrarnos en entregar valor trabajando más cómoda y eficientemente.

**Consular con los presentes.**

---
name: why-2
layout: false
## Algunos dolores actuales

- Tecnologías obsoletas 

???

Las tecnologías obsoletas no solo afectan el ánimo de los programadores sino que generan dificultades para incorporar integrantes a los equipos y otras limitaciones por el ecosistema (por ejemplo, no es buena idea ejecutar una app de .NET clásico en docker).

--

- Problemas de escalabilidad

--

- Preparar el ambiente y hacer pruebas locales es engorroso

--

- Trabajar sobre _"código feo"_

???

El código actual no es realmente "feo", se llegó ahí por muchas razones luego de muchas personas y muchos de años de trabajo. Pero aún así, el código no encaja con el estilo actual y nos impide utilizar nuevos estilos más limpios. Además, al intentar respetar una coherencia (o minimizar los cambios) sin entender completamente los fundamentos (muchos años, muchas personas), vamos erosionándolo más, por ejemplo utilizando incorrectamente abstracciones existentes y no crear nuevas.

--

- Dificultades para hacer unit tests

???

Los programadores anteriores no tenían en cuenta los unit tests, y la erosión nos lleva a un código cada vez más acoplado y poco cohesivo.

--

- Fragilidad

???

Tanto la falta de tests, el acoplamiento, la poca cohesión, falta de abstracciones correctas y demasiadas abstracciones _leaky_ hacen que el impacto de los cambios sea un poco impredecible.

--

- Dificultades para hacer experimentos, refactorizaciones y actualizaciones

???

Hay un circulo vicioso entre la fragilidad y los problemas que la provocan.

--

- Dificultades para automatizar validaciones

???

Formateo automático de código, validación de convenciones de código, análisis estático, test unitarios en CI/CD, etc.

--

- Releases fuera de horario de trabajo

???

También riesgos y nervios a la hora del deploy. Si hay que hacer un rollback tal vez no pueda hacerse rápido.

---
name: por-que-big-smash
layout: false
class: center, middle, inverse

.image-scaled[![Big Smash](img/big-smash.png)]

???

[Mary Poppendieck nos dice...](https://www.youtube.com/watch?v=J1OrTht8LeA)

---
name: por-que-small-pokes
layout: false
class: center, middle, inverse

.image-scaled[![Small Pokes](img/small-pokes.png)]

???

Siguiendo esta premisa, los micro-frontends son una de las herramientas para resolver los problemas que mencionamos anteriormente.

También tenemos micro-servicios, mejoras iterativas sobre Albor2, tercerizar partes del sistema, etc.

---
name: small-app-1
layout: false
## Aplicación pequeña

- **Aplicación:** Una aplicación tiene límites claros, interfaces bien definidas con otros sistemas.
- **Pequeña:** No tiene demasiado código, tiene un scope reducido.

???

Mencionar ejemplos de posibles aplicaciones. Por ahora: Albor Home y Albor Auth Service MFE, luego podrían haber menues, librería de estilos, webcomponentes, etc.

Para ejemplificar el tema de los límites: ahora mismo el micro-frontend de Albor Home tiene 4 sistemas _limítrofes_:

- Albor 2
  - Configuración a través de objecto global `window`.
  - HTML de los menues a través del DOM
  - CSS de Albor 2
- El usuario
  - a través del browser
- API de Albor 3 (aka API Remitos)
  - a través de requests HTTP
- Albor Auth Service Micro-frontend
  - Obtención del token de la API

--

### Ventajas

- Fácil de entender

- Fácil de probar

- Fácil de escalar

- Fácil para experimentar

- Fácil para evolucionar (refactorizar, actualizar dependencias, estilo, etc.)

--

- **Fácil de reemplazar**

--
 
### Desafíos

- Prestar atención a que los límites de la aplicación estén claros y no romperlos

---

name: small-app-2
layout: true
class: center, middle, inverse
---
# Demo

???

Mostrar cómo se puede probar Albor Home localmente con datos dummy.

Mostrar el Storybook de Albor Home y contar cómo se llegó ahí luego de reemplazar esbuild por vite y refactorizar los componentes para desacoplarlos del backend e incluir los estilos.

Mencionar refactorización de autenticación para ejemplo de compatibilidad hacia adelante y atrás:
- [Refactorización para aislar módulos de autenticación](https://gybconsulting.visualstudio.com/AgroNET/_git/albor-home/pullrequest/2357)
- [No inicializar autenticación si falta configuración](https://gybconsulting.visualstudio.com/AgroNET/_git/albor-home/pullrequest/2366)
- [Albor2 inicializar auth en nuevo módulo en lugar de home](https://gybconsulting.visualstudio.com/AgroNET/_git/AgroNET/pullrequest/2368)
- [Eliminar código redundante](https://gybconsulting.visualstudio.com/AgroNET/_git/albor-home/pullrequest/2370)

---
name: independent-lifetime
layout: false
## Ciclo de vida independiente

<br />

--

### Ventajas

- Nos da libertad para entregar valor antes 

- Permite adaptar el tipo de tests según los riesgos específicos de la aplicación

- Permite validar rápidamente la aplicación publicada

- Reduce riesgos y permite rollbacks menos traumáticos

- Ayuda a realizar experimentos

--
 
### Desafíos

- Mantener compatibilidad hacia adelante y hacia atrás

- Feature flags

---
name: pipelines
layout: false
## Validaciones y deploys automáticos

<br />

- Validación de convenciones de código

- Validación de ortografía (y typos)

- Validación convenciones de commits

- Linting

- Build

- Tests

- Publicar fácil y rápidamente a cualquier entorno

- Probar código no mergeado aún con datos reales en cualquier entorno

---

name: pipelines-demo
layout: true
class: center, middle, inverse
---
# Demo

???

Hacemos algún pequeño cambio, hacemos un PR y lo probamos en diferentes entornos.

<!-- TODO: Preparar el cambio de antemano, también el contenido del PR, alguna captura, etc -->

---
name: how
layout: false
class: center, middle
# ¿Cómo funciona?

---
name: hashes-cache
layout: false
## Hashes y Cache

- ¿Que son y para que sirven esos molestos sufijos que se agregan a los nombres de archivos en el builds?

  Por ejemplo `main.d6c28435.js`

- Problemas publicando los archivos con el mismo nombre

- Proxy <!-- TODO: consultar a Mati cómo se llama el proxy que usamos en AWS -->

---
name: manifest-loader
layout: false
## Manifests y Loader

1. Build genera nombres de archivos con hash

2. Build genera un `asset-manifest-{version}.json` con referencias a eso archivos

  Nosotros usamos `asset-manifest-v1.json` para producción y `asset-manifest-main.json` para PA-DEV y PA.

3. Se publican esos archivos a una ubicación conocida

  Por ejemplo `https://cdn.alboragro.com/albor-home/asset-manifest-v1.json`

4. Donde se usa el micro-frontend, hacemos referencia a la URL absoluta del manifest y los cargamos con un script (_loader_)

  Por ahora se cargan dentro de las páginas de Albor 2. Luego podríamos tener archivos estáticos para las "mini-aplicaciones"

---

name: small-app-2
layout: true
class: center, middle, inverse
---
# Demo

???

Mostrar como está configurado y cómo se carga la home desde el `.cshtml` de Albor 2.

Abrirlo en algún entorno y mostrar las llamadas HTTP.

---
name: how-to
layout: false
class: center, middle
# ¿Cómo se implementa?

---

name: how-to-demo
layout: true
class: center, middle, inverse
---
# Demo

???

Mostrar repo de Albor Auth Service MFE o Albor Home y los pipelines de build.

---
name: keep-it-clean
layout: false
class: center, middle
# ¿Cómo lo mantenemos?

???

¡Muy bien! ya tenemos un sistema moderno, con las últimas tecnologías y prácticas para un desarrollo eficiente. Pero, ¿qué impide que dentro de 2 años estemos en la misma situación?

---
name: como-antifragile
layout: false

# Antifragile

.image-scaled[![Fragile / Robust / Antifragile](img/fragile-robust-antifragile.png)]

???

Diseño incremental respondiendo al feedback, pasos pequeños y controlados. No es lo mismo hacer algo incompleto y perfectible que hacer algo con bugs incontrolados.

Contra-ejemplos:  Diseño completo predefinido, 10X developer, The Zone.

---
name: como-pasitos-0
layout: false

# Pequeños pasos incrementales (o no tanto)

Queremos construir esta feature:

.center[![La feature que queremos](img/pasitos-completo.jpg)]

Podemos tomarnos 4 sprints y hacerlo completo, incluso, todo en un mismo _Pull Request_.

--

Pero

* No entregamos valor hasta el final.

--

* Si algo pasa y tenemos que dar prioridad a otra cosa, es muy probable que se 
pierda el esfuerzo.

--

* Si la feature estaba mal definida y no resuelve el problema deseado, recién lo
descubrimos al final.

--

* ¿Quien va a poder encontrar un bug en ese PR?

--

* ¿Se va a poder probar toda la feature?

--

* ¿Como sabemos que no hay huecos? _

---
name: como-pasitos-1
layout: false

# Pequeños pasos incrementales (o no tanto)

.center[![Parte por parte](img/pasitos1.jpg)]

--

Pero

* No entregamos valor hasta el final.

--

* Si algo pasa y tenemos que dar prioridad a otra cosa, es muy probable que se
pierda el esfuerzo. ¿Que hacemos con esa rueda?

--

* Si la feature estaba mal definida y no resuelve el problema deseado, recién lo
descubrimos al final.

--

* Se puede probar cada paso, pero no en contexto, ¿que ocurre si en el análisis
o diseño olvidamos algo? _

---
name: como-pasitos-2
layout: false

# Pequeños pasos incrementales (o no tanto)

.center[![Mejorando el auto](img/pasitos2.jpg)]

--

Pero

* El primer paso es muy grande, hasta llegar a él, tenemos los problemas anteriores.

--

* Tenemos pasos funcionales pero poco elegantes, ya que todo está pensado para
que el paso final sea el ideal.

--

* Si algo pasa y tenemos que dar prioridad a otra cosa, es muy probable que
terminemos con esa camioneta fea. _

---
name: como-pasitos-3
layout: false

# Pequeños pasos incrementales (o no tanto)

.center[![Cada paso un vehículo](img/pasitos3.jpg)]

--

Pero

* No sabemos re-usar el monopatín para hacer la bicicleta.

???

Si hacemos el software como máquinas, hacer el monopatín no nos sirve para hacer
la bicicleta.

Pero si hacemos el software como **profesionales**, aprendemos, generamos estructuras
para hacer más fácil estructuras más complejas.

--

* Tardamos más.

--

* ¿Tardamos más? _

???

Tal vez descubrimos que lo que necesitábamos era la bicicleta, entonces tardamos
menos.

Las otras alternativas no garantizan que lleguemos a lo que necesitábamos, o que
no aparezcan escenarios inesperados.

---
name: como-más
layout: false

# Saquemos ventaja del compilador

.left-column[
```csharp
var value = 0;
if (condition)
{
    value = 100;
}
else
{
    value = 200;
}
operation(value);
```

]
.right-column[
```csharp
int value;
if (condition)
{
    value = 100;
}
else
{
    value = 200;
}
operation(value);
```
]

???

¿Cual código es mejor?

Supongamos que ya no tiene sentido el segundo caso.

--

.left-column[
```csharp
var value = 0;
if (condition)
{
    value = 100;
}


operation(value);
```

]
.right-column[
```csharp
int value;
if (condition)
{
    value = 100;
}

// Error: `value` could be not defined
operation(value);
```
]

???

Si queremos que el valor por defecto sea cero, debemos hacerlo explicitamente.

--

.left-column[
```csharp
var value = condition ? 200 : 0;
operation(value);
```
]

???

Mejor todavía, usemos el operador ternario y evitemos la _mutabilidad_.

Ese es un ejemplo cualquiera, hay muchas formas de aprovechar al compilador, _campos readonly_, no usar 
nullables cuando no corresponde, etc. Las restricciones del compilador están para ayudarnos!

En JavaScript podemos usar _const_ para evitar ciertos tipos de mutabilidad, en c# no tenemos eso, pero
también deberíamos evitarlo.

Lo mismo con Linq u otras formas de programación declarativa.

---
name: como-acronims
layout: false

## SOLID

???

Tenemos que estudiar estos pricipios y aplicarlos siempre que podamos, o al
menos tenerlos en cuenta cuando decidamos romperlos. 

Solo voy a enumerar los principios, no voy a dar un curso sobre ellos, tampoco
soy un experto.

--

* **`S`** - Single responsibility principle

???

> _Una clase debe tener solo una razón para cambiar._
> 
> — Uncle Bob

También deberíamos tener en cuenta la inversa. En lo posible, si la lógica cambia
deberíamos modificar solo una clase.

--

* **`O`** - Open/closed principle

???

Las entidades abiertas para extensión, pero cerradas para modificación.

Hay momentos en que vale la pena refactorizar e introducir breking changes, pero
por defecto, tenemos que mantener compatibilidad hacia atrás, tanto en API's como
en objetos.

--

* **`L`** - Liskov substitution principle

???

Tenemos que tenerlo en cuenta en diseños donde hay diferentes tipos que se comportan
como uno, evitar `if` basados en tipos, pero lo mismo clases que representan muchos
conceptos a la vez. En algún punto se relaciona con el SRP: si rompemos este principio
cuando algo cambie en la lógica del negocio tendremos que cambiar el código en
múltiples lugares.

--

* **`I`** - Interface segregation principle

???

Los clientes de un programa sólo deberían conocer de éste aquellos métodos que realmente usan, y no aquellos que no necesitan usar.

Cuanto más expongamos, más nos atamos.

--

* **`D`** - Dependency inversion principle

???

Los módulos de alto nivel no deben depender en módulos de bajo nivel. Ambos deben depender de abstracciones.

Eso no solo nos permite intercambiar las implementaciones y ayuda al implementar tests, sino que también
nos ayuda a definir más claramente los límites de cada clase y lograr un sistema menos acoplado.

--

<br />


## YAGNI

_You Aren't Gonna Need It._

???

Cada cosa que hacemos, nos ata. Hagamos solo lo necesario, pensemos en el futuro,
pero no dejemos cosas preparadas para después, más bien, tratamos de que lo que
hacemos tenga sentido en un modelo, si es así, podrá evolucionar bien.

---

# Mucho más

* Cada cambio incluido en un commit tiene que tener una razón, no tiene que haber ni una línea de más ni una de menos.
* Tests, tests y más tests y cuanto más livianos mejor.
* Creatividad para facilitar la visibilidad y la prueba temprana de los cambios.
* Automatizar ejecución de tests y otras restricciones, por ejemplo de estilo de código, en cada PR.
* Code Review, no solo para evitar errores, sino para transferencia de conocimiento.
* Dedicar tiempo al otro, hablemos, ayudémonos, pensemos, hagamos pair programming, etc.
* Cuidado con la falacia de la _Deuda Técnica_. 
* Somos profesionales, entonces: aprendamos, pensemos y decidamos.
* Otras?

???

La idea es poder asegurar la calidad y la coordinación desde el principio.

> * Creatividad para facilitar la visibilidad y la prueba temprana de los cambios.

Integrar a todo el equipo, facilitar la lectura de los PRs, ejemplos de storybook, publicación de los PRs, etc.

> * Cuidado con la falacia de la _Deuda Técnica_. 

La calidad no se puede deber, porque nunca se sabe que cuanto va a ser el _interés_. Si podemos decidir recortar detalles, pero nunca perder el control.

> * Somos profesionales, entonces: aprendamos, pensemos y decidamos.

No hay tareas _fáciles_, no hay _"piloto automático"_, no existe el arquitecto que nos diga cómo hacerlo y todo va a estar bien. Tenemos que tomar decisiones todos los días orientadas a buscar valor y asegurar la sustentabilidad.

---

name: gracias
layout: false
class: center, middle, inverse

¡Gracias! 😊
