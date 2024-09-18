# [Spring Boot SonarQube](https://www.youtube.com/watch?v=H7XvwUMaCno)

- Este tutorial se tomó del canal de youtube de **Un Programador Nace**.
- Como base para el desarrollo de este tutorial utilicé el siguiente proyecto
  [spring-boot-test](https://github.com/magadiflo/spring-boot-test).

---

## Instala Plugin de SonarLint en IntelliJ IDEA

`SonarLint` de `Sonar` es una extensión IDE gratuita para encontrar y solucionar problemas de codificación
`en tiempo real`, marcando los problemas mientras escribes código, como un corrector ortográfico. Más que un `linter`,
también ofrece una guía contextual completa para ayudar a los desarrolladores a comprender por qué hay un problema,
evaluar el riesgo y enseñarles cómo solucionarlo. Esto ayuda a mejorar sus habilidades, aumentar su productividad y
tomar posesión de su código, llevando el `linting` a un nivel diferente.

Cuando se combina con `SonarQube` o `SonarCloud` en modo conectado, `SonarLint` forma una poderosa plataforma de calidad
de código de extremo a extremo para enriquecer el flujo de trabajo de `CI/CD`, asegurando que cualquier edición o
adición de código sea limpia. En el modo conectado, su equipo puede compartir conjuntos de reglas de lenguaje comunes,
configuraciones de análisis de proyectos y más.

`SonarLint` es una poderosa herramienta de código abierto para desarrolladores de todos los niveles de experiencia y
habilidad, que les permite entregar `código limpio`: código apto para el desarrollo y la producción. Una herramienta de
linting esencial para todos los desarrolladores.

Lo primero que haremos será instalar el plugin de `SonarLint` en nuestro `IntelliJ IDEA` para que en tiempo real vaya
escaneando nuestro código y nos diga qué tan buenas prácticas de codificación estamos siguiendo, qué es lo que debemos
corregir, etc.

![img.png](assets/01.png)

## Funcionamiento de SonarLint

Luego de haber instalado el `Plugin` de `SonarLint`, veamos rápidamente su funcionamiento dentro de nuestro proyecto.

Si abrimos alguna clase de nuestro proyecto, `SonarLint` entrará en acción, por ejemplo, si abrimos el controlador
`AccountController`, inmediatamente `SonarLint` realizará la verificación de nuestro código mostrándonos en la parte
superior derecha el símbolo de un `círculo con raya en medio de color rojo` y un número. Mientras el número sea el
menor posible, es mejor, dado que nos indica qué tantas malas prácticas han detectado.

![02.png](assets/02.png)

Si damos click en la imagen del círculo rojo, nos aparecerá la consola inferior donde nos indicará dónde se han cometido
las malas prácticas o algún problema de codificación. En nuestro caso, ha detectado dos problemas de intencionalidad,
en nuestro caso la línea `44` y `56` cuya regla que se está violando es
`Los tipos comodín genéricos no deben usarse en tipos de retorno`.

Por el momento lo dejaremos tal cual, ya sabemos para qué sirve el `SonarLint` y ahora continuaremos con el tutorial
donde veremos el uso de `SonarQube` que es el objetivo de este tutorial.
