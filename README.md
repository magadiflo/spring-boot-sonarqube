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

## Configura propiedades de SonarQube en el proyecto

Vamos a agregar las siguientes propiedades de `SonarQube` al archivo `pom.xml`.

````xml

<properties>
    <java.version>21</java.version>

    <!--Sonar Properties-->
    <sonar.projectKey>spring-boot-sonarqube</sonar.projectKey>
    <sonar.projectName>spring-boot-sonarqube</sonar.projectName>
    <sonar.host.url>http://localhost:9000</sonar.host.url>
    <sonar.coverage.jacoco.xmlReportPaths>target/site/jacoco/jacoco.xml</sonar.coverage.jacoco.xmlReportPaths>
    <sonar.coverage.exclusions>src/**/entity/*, src/**/SpringBootTestApplication.java</sonar.coverage.exclusions>
    <!--/Sonar Properties-->
</properties>
````

**DONDE**

- `<sonar.projectKey>` y `<sonar.projectName>`, son usados por el plugin de `SonarQube` (lo agregaremos en el siguiente
  apartado), para identificar el proyecto dentro de `SonarQube`. `projectKey` es el identificador único del proyecto y
  `projectName` es el nombre del proyecto que se mostrará en la interfaz de usuario de `SonarQube`.


- `<sonar.host.url>`, es la URL del servidor de `SonarQube` al que el plugin enviará el análisis. En nuestro caso,
  apunta a un servidor `SonarQube` local en `http://localhost:9000`.


- `<sonar.coverage.jacoco.xmlReportPaths>`, define la ruta del reporte de cobertura de `JaCoCo` que será consumido por
  `SonarQube`. El plugin espera este reporte en la ruta especificada después de ejecutar los tests.


- `<sonar.coverage.exclusions>`, define los archivos o paquetes que deseas excluir del análisis de cobertura de código.
  En nuestro caso, estamos excluyendo las clases bajo `src/**/entity/*` y el archivo `SpringAppApplication.java`. Estas
  serán las mismas exclusiones que colocaremos en la etiqueta `<exclude>` del plugin de `JaCoCo`.

## Agrega plugins de SonarQube y JaCoCo

En nuestro `pom.xml` agregamos los siguientes plugins correspondientes a `SonarQube`, para la revisión de la calidad de
código y a `JaCoCo`, para la cobertura de código. Para mayor información sobre el uso de `JaCoCo` visitar mi
repositorio de [spring-boot-test-jacoco](https://github.com/magadiflo/spring-boot-test-jacoco).

````xml

<plugins>
    <!--SonarQube-->
    <plugin>
        <groupId>org.sonarsource.scanner.maven</groupId>
        <artifactId>sonar-maven-plugin</artifactId>
        <version>4.0.0.4121</version>
    </plugin>
    <!--/SonarQube-->
    <!--JaCoCo-->
    <plugin>
        <groupId>org.jacoco</groupId>
        <artifactId>jacoco-maven-plugin</artifactId>
        <version>0.8.12</version>
        <executions>
            <execution>
                <goals>
                    <goal>prepare-agent</goal>
                </goals>
            </execution>
            <execution>
                <id>report</id>
                <phase>test</phase>
                <goals>
                    <goal>report</goal>
                </goals>
                <configuration>
                    <excludes>
                        <exclude>**/entity/Account.class</exclude>
                        <exclude>**/entity/Bank.class</exclude>
                        <exclude>**/SpringBootTestApplication.class</exclude>
                    </excludes>
                </configuration>
            </execution>
            <execution>
                <id>check</id>
                <goals>
                    <goal>check</goal>
                </goals>
                <configuration>
                    <rules>
                        <rule>
                            <element>PACKAGE</element>
                            <limits>
                                <limit>
                                    <counter>LINE</counter>
                                    <value>COVEREDRATIO</value>
                                    <minimum>0.85</minimum>
                                </limit>
                            </limits>
                        </rule>
                    </rules>
                </configuration>
            </execution>
        </executions>
    </plugin>
    <!--/JaCoCo-->
</plugins>
````

Notar que en el plugin de `JaCoCo` estoy excluyendo tres clases: dos entidades y la clase principal de la aplicación.
Las entidades por sí mismas no contienen lógica, solo contiene atributos con los que se mapean a las columnas de
las tablas en la base de datos.

Con respecto a la clase principal `SpringBootTestApplication`, también lo excluímos, dado que no tenemos lógica dentro,
pero si la tuviéramos, sí sería necesario crearle sus test unitario y no excluirlo de la cobertura.

**NOTA**

> En nuestro caso, la entidad `Account` sí contiene cierta lógica en el método `debit()`, incluso está lanzando
> una excepción. Definir este método con esa lógica dentro de la entidad, estaría mal desde mi punto de vista.
> La lógica debería haberlo colocado en otra clase, pero bueno, así desde un inicio lo diseño el tutor `Andrés Guzmán`
> que fue de donde se tomó el curso original de este proyecto base (`spring-boot-test`). Entonces, en mi caso, también
> lo voy a excluir para tratarlo como una entidad que no debe añadirse para la evaluación de la cobertura de código.