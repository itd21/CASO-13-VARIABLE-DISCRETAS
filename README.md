---
title: "CASO 13 VARIABLES DISCRETAS"
author: "ANGELICA ITZEL LOPEZ DELGADO"
date: '2022-03-27'
output: html_document
 code_folding: ocultar
    toc: verdadero
    toc_float: verdadero
    toc_depth: 6
editor_options: 
  markdown: 
    wrap: 72
---

# Objetivo

Resolver cuestiones de casos de probabilidad en casos mediante la identificación de variables aleatorias, funciones de probabilidad,funciones acumuladas, media, varianza y desviación estándar de distribuciones de variables discretas; visualización gráfica relacionada con variables discretas.

# Descripción

Desarrollar ejercicios relacionados con variables discretas para identificar variables discretas, las funciones de probabilidad de cada variable, la función acumulada, su visualización gráfica para su correcta implementación.

Se incluye en el caso, media, varianza y desviación estándar de distribuciones de variables discretas.

Los casos son identificados de la literatura relacionada con variables aleatorias discretas. Se deben elaborar tres ejercicios en este caso 13 encontrados en la literatura que se encuentran en el caso 14.

# Marco de referencia

Una variable aleatoria es una descripción numérica del resultado de un experimento [@anderson2008c].

Las variables aleatorias deben tomar valores numéricos. En efecto, una variable aleatoria asocia un valor numérico a cada uno de los resultados experimentales.

El valor numérico de la variable aleatoria depende del resultado del experimento. Una variable aleatoria puede ser discreta o continua, depende del tipo de valores numéricos que asuma. [@anderson_estadistica_2008].

Para este documento se tratan únicamente variables del tipo discreta.

En cualquier experimento aleatorio, los resultados se presentan al azar; así, a este se le denomina variable aleatoria. Por ejemplo, lanzar un dado constituye un experimento: puede ocurrir cualquiera de los seis resultados posibles. Cada valor de la variable aleatoria se relaciona con una probabilidad que indica la posibilidad de un resultado determinado [@lind_estadistica_2015].

En su libro [@walpole_probabilidad_2012] define que una variable aleatoria es una función que asocia un número real con cada elemento del espacio muestral.

Una función de probabilidad, una función de masa de probabilidad o una distribución de probabilidad de la variable aleatoria discreta X si, para cada resultado x posible.

-   Toda función de probabilidad debe ser mayor o igual que $0$. $$f(x) \geq 0$$

-   La suma de las probabilidad de todas las variables $x$ debe ser igual a $1$ o la suma de los valores de cada función de probabilidad con respecto a $x$ debe ser $1$ $$\sum _xf(x) = 1$$

-   La probabilidad de cada variable $x$ es igual a la función de probabilidad con respeto a $x$ $$P(X=x) = f(x)$$ [@walpole_probabilidad_2012].

Por otra parte, la función de la distribución acumulativa F(x) ó probabilidad acumulada de una variable aleatoria discreta $X$ con distribución de probabilidad $f(x)$ está dada por la suma de sus probabilidades de $t$ siendo $t$ menor o igual a $x$. Es decir, la probabilidad acumulada suma los valores de las funciones de probabilidad a partir del valor inicial de $x$. El valor final con respecto a valor final de $x$ debe ser igual a 1. $$F(x)=P(X \le x) = \sum_{t \le x}f(t)$$ [@walpole_probabilidad_2012].

La media de una distribución discreta es también recibe el nombre de valor esperado. Se trata de un promedio ponderado de los posibles valores de una variable aleatoria se ponderan con sus correspondientes probabilidades de ocurrencia [@lind_estadistica_2015]

La fórmula para el valor esperado es: $$\mu = \sum x \cdot P(x)$$

La varianza de una distribución discreta constituye un valor típico para resumir una distribución de probabilidad discreta, describe el grado de dispersión (variación) en una distribución [@lind_estadistica_2015].

Su fórmula es: $$\alpha^2 = \sum(x-\mu)^2\cdot P(x)$$

La fórmula anterior significa:

La media se resta de cada valor de la variable aleatoria y la diferencia se eleva al cuadrado.

Cada diferencia al cuadrado se multiplica por su probabilidad.

Se suman los productos resultantes para obtener la varianza.

La desviación estándar, $\alpha$, se determina al extraer la raíz cuadrada positiva de $\alpha^2$; es decir, $\alpha = \sqrt{\alpha^2}$ [@lind_estadistica_2015].

# Desarrollo

## Cargar librerías

-   Posiblemente se utilicen algunas de ellas

```{r warning=FALSE, message=FALSE}
library(ggplot2)
library(stringr)  # String
library(stringi)  # String
library(gtools)
library(dplyr)
library(knitr)
options(scipen = 999) # Notación normal
```

## Ejercicios

-   Para cada ejercicio algunos vistos en el caso anterior y otros nuevos para este caso, se describe y define su contexto.
-   Se construye su tabla de probabilidad que contenga los valores de la variable aleatoria, la función de probabilidad y su función acumulada, la gráfica de barra de los valores de las variables aleatoria y la gráfica lineal de la función acumulada.
-   Con la tabla de probabilidades en algunos ejercicios se determinan y calculan probabilidades.
-   Se determina el **valor esperado** de cada ejercicio
-   Se determina la **varianza** y la **desviación estándar** de la distribución de las **variables discretas**.

### Billetes para rifa

Se venden 5000 billetes para una rifa a 1 euro cada uno. Existe un único premio de cierta cantidad, calcular los valores de las variables aleatorias y sus probabilidades para 0 para no gana y 1 para si gana cuando un comprador adquiere cincuenta billetes. [@course_hero_variables_nodate].

#### Tabla de probabilidad

```{r}
discretas <- c(0,1)   # 0 Que no gane, 1 que gane
n <- 5000 # sum(casos)
casos <- c(4950,50)
probabilidades <- casos / n
acumulada <- cumsum(probabilidades)   # Acumulada
tabla <- data.frame(x=discretas, 
              casos = casos,
              f.prob.x = probabilidades,
              F.acum.x = acumulada,
              x.f.prob.x = (discretas * probabilidades))
kable(tabla, caption = "Tabla de probabilidad con la columna para valor esperado")
```

#### Valor esperado

Se determina el valor esperado de acuerdo a la fórmula: $$\mu = \sum xP(x)$$

-   VE es el **valor esperado**

```{r}
# VE <- sum(tabla$x * tabla$f.prob.x)
VE <- sum(tabla$x.f.prob.x)
VE
```

El valor esperado significa la media ponderada de las probabilidades o lo que es lo mismo es lo que se puede esperar.

Significa muy muy muy .... remoto la probabilidad de ganar en el sorteo de 5000 boletos `r VE`

#### Varianza

-   Agregando columna para obtención de la varianza a partir de los datos de la tabla previamente generada.

```{r}
tabla <- cbind(tabla, 'VE' = VE, 'x-VE.cuad.f.prob.x' = (tabla$x - VE)^2 * tabla$f.prob.x)
#tabla 
kable(tabla, caption = "Tabla de probabilidad con valor esperado y columnas para varianza")
```

$$\alpha^2 = \sum(x-\mu)^2P(x)$$

-   varianza = varianza de la distribución

```{r}
varianza <- sum((tabla$x - VE)^2 * tabla$f.prob.x)
varianza
```

#### Desviación estándard de una distribución discreta

-   La raiz cuadrada de la varianza $$\alpha = \sqrt{ \alpha^2 }$$

-   desv.std = desviación estándard

```{r}
desv.std <- sqrt(varianza)
desv.std
```

#### La tabla con las sumatorias

```{r}
tabla.sumatorias <- rbind(tabla, apply(tabla, 2, sum))
tabla.sumatorias[nrow(tabla.sumatorias), c(1,4,6)] <- '****'
kable(tabla.sumatorias, caption = "Tabla de probabilidad con sumatorias")
```

#### Gráfica de barra

```{r}
ggplot(data = tabla, aes(x = x, y=f.prob.x, fill=x)) +
  geom_bar(stat="identity") 
```

#### Gráfica lineal acumulada

```{r}
ggplot(data = tabla, aes(x = x, y=F.acum.x)) +
  geom_point(colour="blue") + 
  geom_line(colour="red")
```

### Automóviles de Pelican Ford

Un vendedor llamado John Rasgdale vende la mayor cantidad de automóviles el sábado, así que desarrolló la siguiente distribución de probabilidades, en la cual se muestra la cantidad de automóviles que espera vender un sábado determinado.

-   La variable discreta venta de automóviles: $0,1,2,3,4$ el sábado. Los valores de la probabilidad son : $0.1, 0.2, 0.3, 0.3, 0.1$, previamente definidos.

-   Ya se dan las probabilidades de tal forma que la cantidad de casos no se dispone en este ejercicio.

-   ¿De qué tipo de distribución se trata?, variables discretas

-   ¿Cuántos automóviles espera vender John un sábado normal?

-   ¿Cuál es la varianza de la distribución? [@lind_estadistica_2015].

#### Tabla de probabilidad

```{r}
discretas <- 0:4   
casos <- rep(0, 5)
probabilidades <- c(0.1, 0.2, 0.3, 0.3, 0.1)
acumulada <- cumsum(probabilidades)   # Acumulada
tabla <- data.frame(x=discretas, 
              casos = casos,
              f.prob.x = probabilidades,
              F.acum.x = acumulada,
              x.f.prob.x = (discretas * probabilidades))
kable(tabla, caption = "Tabla de probabilidad con la columna para valor esperado (sin número de casos)")
```

#### Cálculo de probabilidades

¿Cuál es la probabilidad de que se vendan DOS automóviles, es decir $f(x=2)$ ó $P(x=2)$?, **30%**

```{r}
filter(tabla, x == 2 ) %>%
  select(x, f.prob.x)
```

¿Cuál es la probabilidad de que se vendan MENOS DE DOS automóviles, es decir $f(x< 2)$ ó $P(x<2)$ ? **30%**

$$
\sum P(x=0) + P(x=1)
$$

```{r}
filter(tabla, x < 2 ) %>%
  select(x, f.prob.x, F.acum.x)
```

¿Cuál es la probabilidad de que se vendan MAS DE DOS automóviles, es decir $f(x> 2)$ ó $P(x>2)$ ? **40%**

$$
\sum P(x=3) + P(x=4) \text{ ó }
$$

$$
1 - \sum P(x=0) + P(x=1) + P(x=2)
$$

```{r}
filter(tabla, x > 2 ) %>%
  select(x, f.prob.x, F.acum.x)
```

#### Valor esperado

Se determina el valor esperado de acuerdo a la fórmula: $$\mu = \sum x \cdot P(x)$$

-   VE es el **valor esperado**

```{r}
VE <- sum(tabla$x * tabla$f.prob.x)
VE
```

El valor esperado significa la media ponderada de las probabilidades o lo que es lo mismo es lo que se puede esperar.

#### Varianza

-   Agregando columna para obtención de la varianza a partir de los datos de la tabla previamente generada.

```{r}
tabla <- cbind(tabla, 'VE' = VE, 'x-VE.cuad.f.prob.x' = (tabla$x - VE)^2 * tabla$f.prob.x)
kable(tabla, caption = "Tabla de probabilidad con valor esperado y columnas para varianza  (sin número de casos)")
```

$$\alpha^2 = \sum(x-\mu)^2\cdot P(x)$$

-   varianza = varianza de la distribución

```{r}
varianza <- sum((tabla$x - VE)^2 * tabla$f.prob.x)
varianza
```

#### Desviación estándar de una distribución discreta

-   La raiz cuadrada de la varianza $$\alpha = \sqrt{ \alpha^2 }$$

-   desv.std = desviación estándard

```{r}
desv.std <- sqrt(varianza)
desv.std
```

#### La tabla con las sumatorias

```{r}
tabla.sumatorias <- rbind(tabla, apply(tabla, 2, sum))
tabla.sumatorias[nrow(tabla.sumatorias), c(1,2,4,6)] <- '****'
kable(tabla.sumatorias, caption = "Tabla de probabilidad con sumatorias,(sin número de casos)")
```

#### Gráfica de barra

```{r}
ggplot(data = tabla, aes(x = x, y=f.prob.x, fill=x)) +
  geom_bar(stat="identity") 
```

#### Gráfica lineal acumulada

```{r}
ggplot(data = tabla, aes(x = x, y=F.acum.x)) +
  geom_point(colour="blue") + 
  geom_line(colour="red")
```

### Solicitudes de puestos de hombres y mujeres

Una compañía tiene cinco solicitantes para dos puestos de trabajo: dos mujeres y tres hombres. Suponga que los cinco solicitantes son igualmente calificados y que no hay preferencia para elegir su género al igual que no importa el orden de género de hombres y mujeres (combinaciones).

Sea $x$ la variable aleatoria discreta al número de mujeres elegidas para ocupar los dos puestos de trabajo. Encuentre las probabilidades para elegir 0 mujeres, 1 mujer o 2 mujeres. [@mendenhall_introduccion_2010].

Haciendo las combinaciones en donde $M = Mujer \text{ y }H = Hombre$

```{r}
personas <- c("H1", "H2", "H3", "M1", "M2")
S.espacio.muestral <- combinations(n = 5, r = 2, v=personas)
S.espacio.muestral 
```

De acuerdo al espacio muestral $n$ con diez elementos, ¿en cúantas ocasiones hay cero mujeres?, ¿en cuántas ocasiones hay una mujer? y en cuántas ocasiones hay dos mujeres?

```{r}
discretas <- c(0, 1, 2)
casos <- c(3, 6, 1 )
n <- sum(casos)
probabilidades <- casos / n
```

#### Tabla de probabilidades

```{r}
acumulada <- cumsum(probabilidades)   # Acumulada
tabla <- data.frame(x=discretas, 
              casos = casos,
              f.prob.x = probabilidades,
              F.acum.x = acumulada,
              x.f.prob.x = (discretas * probabilidades))
kable(tabla, caption = "Tabla de probabilidad con la columna para valor esperado")
```

#### Cálculo de probabilidades

¿Cuál es la probabilidad de que haya **UNA MUJER?,** es decir $P(X=1)$ ó $f(x=1)$ ? **60%**

```{r}
filter(tabla, x == 1 ) %>%
  select(x, f.prob.x)
```

¿Cuál es la probabilidad de que haya **MENOS DE DOS MUJERES?,** es decir $P(x=0) + P(x=1)$ ó $f(x<2)$ ? **90%**

```{r}
filter(tabla, x < 2 ) %>%
  select(x, f.prob.x, F.acum.x)
```

¿Cuál es la probabilidad de que haya **MAS DE 1 MUJER O SEA DOS?,** es decir $P(x=2)$ ó $f(x>1)$ ? **10%**

```{r}
filter(tabla, x > 1 ) %>%
  select(x, f.prob.x, F.acum.x)
```

#### Valor esperado

Se determina el valor esperado de acuerdo a la fórmula: $$\mu = \sum x \cdot P(x)$$

-   VE es el **valor esperado**

```{r}
VE <- sum(tabla$x * tabla$f.prob.x)
VE
```

#### Varianza

$$\alpha^2 = \sum(x-\mu)^2 \cdot P(x)$$

```{r}
tabla <- cbind(tabla, 'VE' = VE, 'x-VE.cuad.f.prob.x' = (tabla$x - VE)^2 * tabla$f.prob.x)
kable(tabla, caption = "Tabla de probabilidad con valor esperado y columnas para varianza")
```

Calculando la varianza

```{r}
varianza <- sum((tabla$x - VE)^2 * tabla$f.prob.x)
varianza
```

#### Desviación estándar

$$\alpha = \sqrt{ \alpha^2 }$$

Con la raiz cuadrada de la varianza se determina la desviación estándard de la distribución de variables aleatorias.

```{r}
desv.std <- sqrt(varianza)
desv.std
```

#### Tabla con sumatorias

```{r}
tabla.sumatorias <- rbind(tabla, apply(tabla, 2, sum))
tabla.sumatorias[nrow(tabla.sumatorias), c(1,4,6)] <- '****'
kable(tabla.sumatorias, caption = "Tabla de probabilidad con sumatorias")
```

#### Gráfica de barra

```{r}
ggplot(data = tabla, aes(x = x, y=f.prob.x, fill=x)) +
  geom_bar(stat="identity") 
```

#### Gráfica lineal acumulada

```{r}
ggplot(data = tabla, aes(x = x, y=F.acum.x)) +
  geom_point(colour="blue") + 
  geom_line(colour="red")
```

### Número de hijos de parejas

En la siguiente tabla se presenta la distribución del número de hijos de un grupo de 100 parejas (humanos): Ejercicio extraído de: [@web_descartes_estadistica_2018].

+---------------------------+---------------------+
| variable aleatoria x      | cantidad de parejas |
|                           |                     |
| No hijos                  |                     |
+:=========================:+:===================:+
| 0                         | 15                  |
+---------------------------+---------------------+
| 1                         | 40                  |
+---------------------------+---------------------+
| 2                         | 23                  |
+---------------------------+---------------------+
| 3                         | 10                  |
+---------------------------+---------------------+
| 4                         | 7                   |
+---------------------------+---------------------+
| 5                         | 4                   |
+---------------------------+---------------------+
| 6                         | 1                   |
+---------------------------+---------------------+
| Total parejas encuestadas | 100                 |
+---------------------------+---------------------+

```{r}
discretas <- c(0, 1, 2, 3, 4, 5, 6)
casos <- c(15, 40, 23, 10, 7, 4, 1 )
n <- sum(casos)
probabilidades <- casos / n
```

#### Tabla de probabilidades

```{r}
acumulada <- cumsum(probabilidades)   # Acumulada
tabla <- data.frame(x=discretas, 
              casos = casos,
              f.prob.x = probabilidades,
              F.acum.x = acumulada,
              x.f.prob.x = (discretas * probabilidades))
kable(tabla, caption = "Tabla de probabilidad con la columna para valor esperado")
```

#### Cálculo de probabilidades

¿Cuál es la probabilidad de encontrar aletoriamente parejas con TRES HIJOS, es decir, $f(x=3)$ ó $P(x=3)$ **10%**

```{r}
filter(tabla, x == 3 ) %>%
  select(x, f.prob.x, F.acum.x)
```

¿Cuál es la probabilidad de encontrar aleatoriamente parejas con MENOS DE TRES HIJOS, es decir, $f(x<3)$ ó $\sum f(x={0,1,2})$ ó $\sum P(x=0) + P(x=1) + P(x=2)$ ó $F \text{ acumulada }(x)$

**78%**

```{r}
filter(tabla, x < 3 ) %>%
  select(x, f.prob.x, F.acum.x)
```

¿Cuál es la probabilidad de encontrar aleatoriamente parejas con MAS DE TRES HIJOS, es decir, $f(x>3)$ ó $\sum f(x={4,5,6})$ ó $\sum P(x=4) + P(x=5) + P(x=6)$ ó $1 - F(x = 3)$; **12%**

```{r}
filter(tabla, x > 3 ) %>%
  select(x, f.prob.x, F.acum.x)
```

#### Valor esperado

Se determina el valor esperado de acuerdo a la fórmula: $$\mu = \sum x \cdot P(x)$$

-   VE es el **valor esperado**

```{r}
VE <- sum(tabla$x * tabla$f.prob.x)
VE
```

#### Varianza

$$\alpha^2 = \sum(x-\mu)^2 \cdot P(x)$$

```{r}
tabla <- cbind(tabla, 'VE' = VE, 'x-VE.cuad.f.prob.x' = (tabla$x - VE)^2 * tabla$f.prob.x)
kable(tabla, caption = "Tabla de probabilidad con valor esperado y columnas para varianza")
```

Calculando la varianza

```{r}
varianza <- sum((tabla$x - VE)^2 * tabla$f.prob.x)
varianza
```

#### Desviación estándar

$$\alpha = \sqrt{ \alpha^2 }$$

Con la raiz cuadrada de la varianza se determina la desviación estándard de la distribución de variables aleatorias.

```{r}
desv.std <- sqrt(varianza)
desv.std
```

#### Tabla con sumatorias

```{r}
tabla.sumatorias <- rbind(tabla, apply(tabla, 2, sum))
tabla.sumatorias[nrow(tabla.sumatorias), c(1,4,6)] <- '****'
kable(tabla.sumatorias, caption = "Tabla de probabilidad con sumatorias")
```

#### Gráfica de barra

```{r}
ggplot(data = tabla, aes(x = x, y=f.prob.x, fill=x)) +
  geom_bar(stat="identity") 
```

#### Gráfica lineal acumulada

```{r}
ggplot(data = tabla, aes(x = x, y=F.acum.x)) +
  geom_point(colour="blue") + 
  geom_line(colour="red")
```

### Lanzamiento de un dado

Se lanza un dado perfecto 240 veces, se anota el resultado obtenido en la cara superior obteniendo los siguientes resultados:

+-----------------+---------+---------+---------+---------+---------+---------+
| Cara superior   | 1       | 2       | 3       | 4       | 5       | 6       |
+:===============:+:=======:+:=======:+:=======:+:=======:+:=======:+:=======:+
| Número de veces | 40      | 39      | 42      | 38      | 42      | 39      |
+-----------------+---------+---------+---------+---------+---------+---------+

#### Inicializando variables

Pendiente

#### Tabla de probabilidad

Pendiente

#### Cálculo de probabilidades

¿Cuál es la probabilidad de que el dado caiga un DOS, es decir $(x=2)$?

¿Cuál es la probabilidad de que el dado caiga en CUATRO, es decir $(x=4)$?

¿Cuál es la probabilidad de que el dado caiga MENOR QUE CUATRO, es decir $(x < 4)$?

¿Cuál es la probabilidad de que el dado caiga MAYOR QUE CUATRO, es decir $(x > 4)$?

#### Valor esperado

Pendiente

#### Varianza

Pendiente

#### Desviación estándar

Pendiente

#### Tabla de sumatorias

Pendiente

#### Gráfica de barra

Pendiente

#### Gráfica acumulada

Pendiente

### Tomar vasos de agua ITD

Se tiene un estudio de que en época de calor los estudiantes del Tecnológico consumen cierta cantidad de vasos de agua durante el dia.

Se estima que se toman al alrededor de 1 a 8 vasos diarios durante el día para aliviar la sed y hidratar el cuerpo.

La siguiente tabla establece la cantidad de vasos que toman los alumnos durante el día siendo x la variable aleatoria discreta los vasos que se toman.

De un estudio de 150 alumnos esas fueron las respuestas.

| x = vasos de agua | casos |
|-------------------|-------|
| 0                 | 8     |
| 1                 | 12    |
| 2                 | 16    |
| 3                 | 19    |
| 4                 | 24    |
| 5                 | 28    |
| 6                 | 25    |
| 7                 | 14    |
| 8                 | 4     |

#### Inicializando variables

```{r}
X <- c(0, 1, 2, 3, 4, 5, 6, 7, 8)
casos <- c(8, 12,  16, 19, 24, 28, 25, 14, 4 )
n <- sum(casos)
probabilidades <- casos / n
```

#### Tabla de probabilidad

Pendiente

#### Cálculo de probabilidades

¿Cuál es la probabilidad de que se elija aleatoriamente alumnos y mencionen que se toman CUATRO VASOS DE AGUA?

¿Cuál es la probabilidad de que se elija aleatoriamente alumnos y mencionen que se toman MENOS DE CUATRO VASOS DE AGUA?

¿Cuál es la probabilidad de que se elija aleatoriamente alumnos y mencionen que se toman MAS DE CUATRO VASOS DE AGUA?

#### Valor esperado

Pendiente

#### Varianza

Pendiente

#### Desviación estándar

Pendiente

#### Tabla de sumatorias

Pendiente

#### Gráfica de barra

Pendiente

#### Gráfica acumulada

Pendiente

# Interpretación de los ejercicios del caso

Se presentaron varios ejercicios de variables aleatorias discretas en donde se determiniaron las funciones de probabilidad y la función acumulada, la media o valor esperado, la varianza y su desviación estándard.

Se generaron gráficas de barras de los valores de las variables y la gráfica lineal de las tendencias.

El valor esperado en el ejercicio 1 del sorteo con valor de 1%, significa que es es muy muy muy .... remoto la probabilidad de ganar en el sorteo de 5000 boletos.

En el ejercicio de vena de automóviles de John, se trata de una distribución de probabilidad discreta de la variable aleatoria "número de automóviles vendidos".

El valor esperado es del 2.1 que significa que puede vender 2 autos como esperanza.

El valor esperado se utiliza para predecir la media aritmética de la cantidad de automóviles vendidos a largo plazo. Por ejemplo, si John trabaja $50$ sábados en un año, puede esperar vender $(50)(2.1)$ o $105$ automóviles solo durante los sábados. Por consiguiente, a veces la media recibe el nombre de valor esperado[@lind2015].

El valor de la varianza es de 1.29 que significa lo que puede variar con respecto al valor esperado. La desviación estándard es de $1.135782$.

¿Cómo se interpreta la variación?

Por ejemplo, Si la vendedora Rita Kirsch también vendió un promedio de 2.1 automóviles los sábados pero tien tal vez una desviacón de 1.9 en comparación del 1.135782 de John, entonces de puede decir que hay mayor variabilidad en la vendedora Rita dado que $(1.91 \geq 1.35)$ [@lind2015].

En el caso de las vacantes de puestos para hombres y mujeres el resultado del valor esperado es de $0.8$ que significa la probabilidad de contratar mujeres en promedio, su desviación estándar es de $0.6$ que significa nivel de dispersión (alejamiento) de la probabilidad de cada variable aleatoria con respecto al valor esperado.

Del ejercicio de parejas contestar las preguntas:

1.  ¿Cuál es la probabilidad de una pareja elegida al azar tenga menos de dos hijos? $P(x<2)$

2.  ¿Cuál es la probabilidad de que tenga más de tres hijos? $P(x>3)$

3.  Si se elige un hijo al azar, ¿cuál es la probabilidad de que no tenga hermanos? $P(x=0)$

4.  Determina el número de hijos esperado al seleccionar una familia al azar. ¿Cuál es el valor esperado y qué significa?

5.  Calcula la varianza y la desviación de la distribución e interpretar su significado.

**Interpretar el ejercicio del dado**

discretas \<- c(1, 2, 3,4,5,6) casos \<- c(40, 39, 42,38,42,39 ) n \<- sum(casos) probabilidades \<- casos / n

acumulada \<- cumsum(probabilidades) \# Acumulada frecacum \<- cumsum(casos) tabla \<- data.frame(x=discretas, casos = casos, f.prob.x = probabilidades, F.acum = frecacum, F.acum.x = acumulada,

              x.f.prob.x = (discretas * probabilidades))

kable(tabla, caption = "Tabla de probabilidad con la columna para valor esperado")

r=filter(tabla, x == 2 ) %\>% select(x, f.prob.x) r=filter(tabla, x == 4 ) %\>% select(x, f.prob.x) r=filter(tabla, x == 3 ) %\>% select(x, F.acum.x) r= filtro \<- filter(tabla, x \> 4 ) %\>% select(x, f.prob.x) filtro

suma \<- sum(filtro$f.prob.x[1]+filtro$f.prob.x[2]) paste(suma)

VE \<- sum(tabla\$x.f.prob.x) VE

v \<- sum(x.VE.cuad.f.prob.x = (tabla$x - VE)^2 * tabla$f.prob.x) v

sqrt(v)

tabla.sumatorias \<- rbind(tabla, apply(tabla, 2, sum)) tabla.sumatorias[nrow(tabla.sumatorias), c(1,4,6)] \<- '\*\*\*\*' kable(tabla.sumatorias, caption = "Tabla de probabilidad con sumatorias")

ggplot(data = tabla, aes(x = x, y=f.prob.x, fill=x)) + geom_bar(stat="identity")

ggplot(data = tabla, aes(x = x, y=F.acum.x)) + geom_point(colour="violet") + geom_line(colour="darkred")

**Interpretar el ejercicio de los vasos de agua** X \<- c(0, 1, 2, 3, 4, 5, 6, 7, 8) casos \<- c(8, 12, 16, 19, 24, 28, 25, 14, 4 ) n \<- sum(casos) probabilidades \<- casos / n

acumulada \<- cumsum(probabilidades) \# Acumulada frecacum \<- cumsum(casos) tabla \<- data.frame(vasos=X, frec = casos, f.prob.x = probabilidades, F.acum = frecacum, F.acum.x = acumulada,

              x.f.prob.x = (X * probabilidades))

kable(tabla, caption = "Tabla de probabilidad con la columna para valor esperado")

R= filter(tabla, vasos == 4) %\>% select(frec,f.prob.x) R=filter(tabla, vasos == 3) %\>% select(F.acum.x) R=filtro\<- filter(tabla, vasos \> 4) %\>% select(vasos,f.prob.x) sum(filtro\$f.prob.x)

valor esperado

VE \<- sum(tabla\$x.f.prob.x) VE

Varianza varianza \<- sum((tabla$vasos-VE)^2*tabla$f.prob.x) varianza tabla \<- cbind(tabla, 'VE' = VE, 'x-VE.cuad.f.prob.x' = (tabla$vasos - VE)^2 * tabla$f.prob.x) DS \<- sqrt(varianza) DS

tabla.sumatorias \<- rbind(tabla, apply(tabla, 2, sum)) tabla.sumatorias[nrow(tabla.sumatorias), c(1,4,6,8)] \<- '\*\*\*\*' kable(tabla.sumatorias, caption = "Tabla de probabilidad con sumatorias")

ggplot(data = tabla, aes(x = X, y=f.prob.x, fill=X)) + geom_bar(stat="identity")

ggplot(data = tabla, aes(x = vasos, y=F.acum.x)) + geom_point(colour="cyan") + geom_line(colour="magenta")

# Referencias bibliográficas
