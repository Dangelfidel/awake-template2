---
title: Revalorización del WIP Real S/4HANA
subtitle: Controlling
category:
  - CO-PC
author: Pablo
date: 2021-03-10T20:34:06.978Z
featureImage: /uploads/gaelle-marcel-9dzy0mo98xu-unsplash.jpg
---
# ¿Qué es el trabajo en curso (WIP)?

Si no estas familiarizado con el concepto de trabajo en curso, el WIP son los costes de materiales y actividades consumidos durante el proceso de fabricación, además de aquellos costes imputados en una orden de fabricación, pero sin haber una entrega o alta en stock del producto terminado al final del periodo.

*Representan aquellos costes de producción de las ordenes incompletas al final del periodo.*

## Revalorización del trabajo en curso

Mediante la función de revalorización del WIP podrás distribuir las diferencias calculadas en la ejecución del coste real del material ledger a los productos entregados en el almacén y al trabajo en curso (WIP) en proporción a las cantidades. Al activar la revalorización del trabajo en curso las diferencias pertenecientes al WIP serán valoradas a precios reales.

Se puede determinar el WIP de dos maneras:

1. Wip a coste real.

La diferencia entre el coste real cargado en una orden (Consumo) y el coste real abonado en la orden (Alta o Entrada).

2. Wip a coste teórico.

La valoración de la cantidad real confirmada hasta la fecha para cada notificación, menos el rechazo relevante.

Si NO hay una revaluación WIP pueden producirse los siguientes problemas reales de absorción de costes:

* Para la entrega parcial de la orden de producción, todas las desviaciones de materiales y clases de actividad se transfieren al nivel superior sólo a la cantidad entregada parcialmente. Esto puede provocar una distorsión del coste real del producto.
* Si no se entrega ningún producto en el período, las desviaciones no se pueden implosionar y no se asignan en el material ledger. En este escenario, las diferencias permanecen en las cuentas de diferencias de precio de material y en los centros de coste de las clases de actividad. Por lo tanto, no existe una absorción de diferencias completa.
* Si no se entrega ningún producto en el período, todas las desviaciones de materiales y clase de actividad pasan a WIP

Mediante la revaloración WIP ocurrirán las siguientes soluciones:

* Si hay una entrega parcial de la orden de producción, todas las desviaciones de materiales y clases de actividad se dividen entre el producto terminado y el WIP.
* Si no se entrega ningún producto en el período, todas las desviaciones de materiales y clase de actividad pasan a WIP

## Requisitos previos

Antes de seguir hablando sobre el trabajo en curso, deberás tener en cuenta algunos requisitos previos.

### Determinación de cuentas

Sera necesaria la configuración de la determinación de cuentas de la Gestión de materiales (MM).

Asignaras cuentas de mayor a operaciones comerciales de gestión de materiales (MM) utilizando la tx OBYC

OBYC: En ella están las operaciones y sus descripciones. Cada línea está relacionada con una clave de operación. Una clave de operación puede definirse como una clase diferente de proceso en MM en el que se asignan cuentas.  

<center> 

![](/uploads/imagen1.png)

 </center>  

Las siguientes claves de operación son necesarias para la revaluación del WIP a costes reales.
Las operaciones relacionadas con las contabilizaciones del cierre del material ledger son WPM y WPA. Las operaciones PRM y PRA están relacionadas con la contabilización de anulación cuando se concluyen las órdenes y el sistema anula las contabilizaciones WIP anteriores.

#### WIP por diferencias de precio: Material (WPM)

WPM es la clave de operación utilizada para contabilizar el material que se consume para un producto no terminado en la cuenta de balance WIP.

![](/uploads/imagen2.png)

En este caso se utiliza la cuenta de balance (Productos en curso ML)

#### WIP por diferencias de precio: Clase de actividad (WPA)

Esta clave de operación debe asignarse a una cuenta de revalorización WIP para contabilizar el consumo de clases de actividad para la fabricación no terminada. Se utilizará cuando hay una revaloración de las tarifas.

![](/uploads/wpa.png)

En este caso se utiliza la misma cuenta (52541100) para contabilizar los materiales y los componentes de clase de actividad en la revaluación del WIP. Sin embargo, se puede utilizar cuentas de balance diferentes para contabilizar los valores de la revaloración WIP de componentes de material y clase de actividad.

#### Diferencias de precio del WIP anulación de los materiales (PRM)

Cuando se complete una orden que estaba previamente en WIP, los valores del WIP deben anularse. La clave de operación PRM debe asignarse a una cuenta para anular el trabajo en curso a coste real.

![](/uploads/prm.png)

Usará la misma cuenta que en WPM para revertir los valores del WIP. Se utiliza la misma cuenta para simplificar movimientos de balance de entradas y salidas del WIP a valores reales en la misma cuenta.

#### Diferencias de precio del WIP anulación de las clases de actividad (PRA)

La misma forma que en PRM, se debe configurar la clave de operación PRA para que asigne una cuenta cuando anule el WIP para clases de actividad.

![](/uploads/pra.png)

Usará la misma cuenta que en WPA para revertir los valores del WIP. Se utiliza la misma cuenta para simplificar movimientos de balance de consumos actividades del WIP a valores reales en la misma cuenta.

### Activación revaluación WIP

![](/uploads/act1.png)

La revaluación WIP se configura mediante la transacción OMXW
La activación de la funcionalidad WIP a costes reales es por centro. Si se deseas utilizar la revaluación WIP a partir de un periodo, tendrás que activar esta funcionalidad antes de que comience el periodo. 
Esta sería la única configuración necesaria en relación con la revaluación del WIP en términos del Material Ledger, también deberías verificar que la configuración de las cuentas sea correcta.

El WIP tiene que estar activado para que se revalorice a precio real.

![](/uploads/centr.png)

## Escenario del proceso de trabajo en curso (WIP)

### Pasos de creación y progreso de orden de fabricación

Lo más fácil para entiendas y comprendas como actúa el WIP y su impacto, es mediante un ejemplo de revaluación del WIP a través de una orden de fabricación.
El escenario estará comprendido en los periodos 12/2020 y 01/2021. Lo primero se partirá de una orden de fabricación, con un producto terminado planificado de 100 piezas y un componente en este caso semielaborado de 100 piezas planificadas.

![](/uploads/j1.png)

Durante el periodo 12/2020 habrá un consumo de la semielaborado y un alta o entrada de mercancía en stock del producto terminado.

![](/uploads/j2.png)

\*Se consumen el total planificado del semiterminado

En el análisis de costes de la orden se observa cómo hay un consumo del producto semiterminado y un alta o entrada en stock producto terminado, además habrá un consumo de actividades, se consumirá el total de lo planificado, el consumo de las actividades se abonará en el centro de coste y se cargará en la orden de producción.

![](/uploads/inf.png)

Al final del periodo se realizarán las tareas del proceso de cierre de controlling, los pasos durante el proceso de cierre, se verificará los documentos del material ledger para entender cómo se generan las diferencias de precio para la revaluación del WIP.

### Cálculo del WIP

Un inciso importante a tener en cuenta, la definición de WIP real en las ordenes de fabricación a costes reales es simplemente la determinación de la diferencia entre los cargos por el consumo de materiales, imputaciones de actividad internas y externas, recargos de gastos generales y los abonos reales de las entradas de mercancías en stock, aunque este método teniendo activo la determinación de WIP real, no representa los costes reales porque el material y las clases de actividad todavía se valoran a precios estándar. Las diferencias a costes real ocurrirán en el momento que se ejecute el cockpit del material ledger.

Prosigamos con los pasos del proceso de cierre de controlling, para calcular el trabajo en curso WIP es mediante la transacción KKAX (Individual), calculará el WIP de la orden de fabricación para el periodo en que se realiza el proceso de cierre. 

![](/uploads/33.png)

Se muestra la orden de fabricación 1001640 que tiene producto no terminado, basándose en el cálculo estándar se determina que hay un WIP de 12,32$, coincidirá con el saldo de la orden.

En la parte de la derecha aparece un nuevo botón llamado Documento cantidades WIP cuando la función WIP real está activada.

![](/uploads/34.png)

Mediante la funcionalidad de revaloración WIP, la determinación del WIP crea el documento de cantidad para cada orden que contiene el producto no terminado. Nos muestra la cantidad de materiales y actividades que se consumieron en la orden pero que no generaron una salida de fabricación, simplemente muestra cantidades consumidas que se han identificado como WIP.

Formula del WIP: Consumo real – Consumo teórico.

El Layer WIP es un nuevo concepto con la revaloración WIP, cada columna de período WIP se denomina Layer WIP. La columna WIP 012 representa los valores en WIP para el periodo 12 
La columna Delta 012 muestra las modificaciones del layer WIP en comparación con el periodo anterior. En este caso la orden de fabricación no tiene WIP en el periodo anterior WIP 011 por lo que la columna delta es igual a la columna WIP

### Análisis de precios de material (ckm3n) y actividades (ckm3a)

Con la funcionalidad de revaluación WIP activa, después de la determinación del WIP, puedes observar en la CKM3N/CKM3A un nuevo tipo de proceso llamado Fabricación WIP

Se puede observar que para la semielaborado tiene una parte de 80 PI para la orden 1001640, la cual se abona a nivel de fabricación y se genera una carpeta de fabricación WIP cargándose en ella 80 PI, ya que esa parte proporcional pertenece al WIP. 

![](/uploads/36.png)

Se puede observar que para la actividad 1 tiene una parte de 0.134 H para la orden 1001640, la cual se abona a nivel de fabricación y se genera una carpeta de fabricación WIP cargándose en ella 0.134 H, ya que esa parte proporcional pertenece al WIP. 

![](/uploads/35.png)

Lo mismo ocurrirá para el resto de las actividades.

![](/uploads/37.png)



### Liquidación ordenes de producción

Uno de los siguientes pasos del proceso de cierre de controlling será la liquidación de las ordenes de producción, el sistema contabiliza en finanzas el trabajo en curso a precio estándar calculado en el paso anterior, generando dos documentos FI y CO.



![](/uploads/38.png)



Se muestra el documento contable que transfiere el valor WIP de PYG (54200000) a una cuenta de balance de productos en curso (13200000). El importe se basa en precios estándar

### Ejecución Cockpit del Material Ledger (Cálculo del coste real).

Tras realizar la liquidación de las ordenes el siguiente paso sería la ejecución del Cockpit del Material Ledger



![](/uploads/39.png)



Para el producto semielaborado de la orden 1001640 el sistema realiza la revaluación WIP generando una diferencia de precio de 32$ para las 80 PI de WIP

Una vez lanzada la ejecución de la tarifa real se calculará el precio real y sus diferencias revalorizando las actividades en las órdenes y saldando los centros de coste productivos al lanzar el cockpit del material ledger.





Para la actividad 1 y centro de coste 17101301 de la orden 1001640 el sistema realiza la revaluación WIP generando una diferencia de precio de 4,5$ para las 0,134 H de WIP

Una vez ejecutado el paso de la contabilización del cierre del material ledger se accederá a los documentos de cierre de material y actividad.

Se muestra las diferencias de precio generadas del trabajo en curso para cada uno de los componentes y actividades para la orden de producción.
En documento finanzas puede acceder a los diferentes documentos generados por el cierre contable de ML.

Cierre contable de materiales

* WPM coste real del material
  La posición 29 muestra la diferencia de precios cargada en la cuenta del WIP
  (52541200) a valor real con un valor de 32$.
* PRY diferencias de precio
  Las posiciones 26, 27 y 30 del documento contable son las diferencias de precio
  recibidas por la semielaborado, que se trasladarán al inventario (40
  $) y se transferirán a otros niveles (8$ a producto terminado y 32$ a la
  cuenta WIP).
* BSX Inventario
  La posición 9 muestra la diferencia de precio cargada en la cuenta de inventario
  (13100000) con un valor de 40$

Cierre contable de Actividades

2021-liq

## Escenario de reversión del WIP

Ya se ha visto como actúa el sistema cuando tiene activo el WIP real, por lo que, el siguiente paso es ver como se comportara el sistema ante una reversión del WIP real.
Por lo tanto, cuando una orden de producción tiene un valor en WIP y las altas de producto se realizan desde una orden, el sistema invierte automáticamente el valor proporcional de la cuenta WIP a la cuenta de PyG. Si se utiliza la revaluación WIP, el sistema necesitara anular el coste real asignado al WIP.
Los siguientes pasos se mostrará el mismo flujo que se ha visto anteriormente.

### Confirmación parcial de la orden de fabricación

Se realizará una entrada de mercancías de 30 PI de las 80 PI restantes del producto terminado de la orden de fabricación anterior.
Con lo cual la determinación del WIP tiene que anular las 30 Ud. de la cuenta del WIP. Manteniendo pendiente 50 PI en proceso en curso del semielaborado (S-202) y 50 PI del producto terminado por dar de alta en stock.