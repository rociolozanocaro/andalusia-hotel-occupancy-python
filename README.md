# Del dato crudo a la decisión: ocupación hotelera en Málaga
Es un mini-proyecto para la prueba técnica de formadora Data Analytics. El objetivo es preparar una actividad que muestre el ciclo de vida del dato desde una perspectiva técnica, pedagógica y ética.

**Pregunta principal:** ¿está la capacidad hotelera de Málaga cerca de su límite en temporada alta?

---

## Contenido del repositorio

```
├── DelDatoCrudoALaDecisión-RocíoLozanoCaro.ipynb   #https://colab.research.google.com/drive/1Z5Kn-ls1jgI842740nIgeV5bOtR_f5pH#scrollTo=36gzMiAnAWc_
├── requirements.txt                #Librerías y sus versiones
├── README.md                       #Guía didáctica
├── data/
│   ├── 2066.csv                    #Copia del CSV original
│   └── 2066_limpio.csv             #Dataset tras la limpieza
└── img/                            #Visualizaciones
    ├── visualización1                  
    ├── visualización2
    └── visualización3
```

## Cómo ejecutarlo

1. Abre el notebook en Google Colab.
2. Ejecuta todas las celdas en orden: Ejecutar todo/Run all.
3. El notebook descarga los datos directamente desde la web del INE, así que no hay que subir ningún archivo. Si la web del INE fallara, sube `data/2066.csv` a Colab y cambia la URL por `'2066.csv'` en la celda de carga.
4. En local: `pip install -r requirements.txt`. La librería `missingno` se instala en la primera celda con `%pip install missingno`.

---

## El dataset

- **Fuente:** Encuesta de Ocupación Hotelera (EOH), INE. Tabla 2066: establecimientos, plazas, grado de ocupación y personal empleado por provincia.
- **Enlaces:** [CSV](https://www.ine.es/jaxiT3/files/t/csv_bdsc/2066.csv) · [Tabla en INEbase](https://www.ine.es/jaxiT3/Tabla.htm?t=2066) · [Metadatos](https://www.ine.es/dynt3/metadatos/es/RespuestaDatos.htm?oe=30235) · [Definición de variables (páginas 4 y 5)](https://www.ine.es/daco/daco42/ocuphotel/meto_eoh.pdf)
- **Licencia de los datos:** Creative Commons Reconocimiento 4.0 (CC BY 4.0). *Elaboración propia con datos extraídos del sitio web del INE: www.ine.es.*
- **Unidad de análisis:** provincia × mes (cada fila es una provincia en un mes concreto).
- **Unidades de medida:** varían según la variable: viajeros, pernoctaciones, días, personas, tanto por cien, establecimientos, plazas y habitaciones.
- **Periodo:** 1999-2026, mensual. Última actualización: agosto de 2026. Los datos de enero de 2026 en adelante son provisionales.
- **Tamaño:** Ver sección 3 del notebook.
- **Columnas principales:** `Provincias`, `Periodo`, `Establecimientos y personal empleado (plazas)` (indica la métrica de cada fila) y `Total` (su valor). Como cada fila es una métrica distinta, `Total` cambia de unidad según la métrica.

## Calidad del dato y limpieza

- Los valores `..` del INE significan "dato no disponible": se convierten en nulos y se eliminan.
- `Total` llega como texto en formato español (punto de miles, coma decimal) y se convierte a número.
- `Periodo` (`2010M01`) se separa en `Año` y `Mes`.
- Tras convertir `Total` aparecen 70 nulos nuevos que se eliminan.
- Outliers: los valores altos de ocupación se mantienen porque corresponden a picos reales (ninguno supera el 100 % ni baja del 0 %).
- Antes/después de dimensiones y % de nulos: ver el notebook, sección 3.

## SQL

El dataset limpio se carga en SQLite (`2066.db`, tabla `tabla_2066_limpia`) y se crea una tabla auxiliar `temporadas` donde los meses de verano son temporada alta y el resto baja. Las 4 consultas están en el notebook, sección 5.

1. Ranking de provincias por ocupación media.
2. Meses de mayor ocupación en Málaga.
3. Málaga frente a Cádiz en ocupación y personal empleado.
4. Temporada alta frente a baja en Málaga.

## Visualizaciones

1. Evolución de la ocupación 1999-2026, Málaga frente a Cádiz.
 ![Visualización1](img/Visualización1.png)
2. Estacionalidad en Málaga: ocupación y personal empleado por mes.
 ![Visualización2](img/Visualización2-1.png)
 ![Visualización2](img/Visualización2-2.png)
3. Distribución de la ocupación mensual, Málaga frente a Cádiz.
 ![Visualización3](img/Visualización3.png)

**Por qué estos visuales:** línea para la evolución temporal, dos paneles para comparar la estacionalidad de dos variables con escalas distintas, boxplot para comparar la dispersión entre provincias. Se descarta el gráfico circular porque no hay partes de un total que comparar.

## Conclusiones


- En Málaga, el mes de mayor ocupación media es agosto (79,5 %), seguido de julio (74,2 %), septiembre (70,3 %) y junio (69,2 %).
- Málaga tiene una ocupación media del 60,5 % frente al 50,4 % de Cádiz.
- En temporada alta (junio-septiembre) Málaga tiene un 73,3 % de ocupación media y 19.024 personas empleadas. En temporada baja, un 54,1 % y 13.328. El empleo del sector es un 43 % mayor en los meses de más ocupación, lo que indica un patrón estacional.

## Recomendación accionable

La recomendación estaría dirigida a la diputación de Málaga: sería recomendable estudiar si la ocupación hotelera en general tan alta a lo largo de los años puede tener consecuencias para la calidad de vida de los ciudadanos. También podría investigar si es algo que pase con otro tipo de trabajos no turísticos y decidir si es beneficioso fomentar otras formas de empleo que no se basen en algo temporal.

## Sesgos, limitaciones y ética

- No recoge apartamentos turísticos ni otras opciones. Solamente hoteles.
- Son datos agregados por provincia y mes: no permiten hablar de hoteles concretos ni de personas.
- No hay datos de tipo de contrato ni salario, así que no se puede afirmar si el empleo estacional es precario o beneficioso.
- Las medias esconden variación. Se complementan con mediana y desviación.
- Los datos desde enero de 2026 son provisionales.
- Sesgos de quien analiza: la forma de ver y entender el mundo tiene influencia en cómo se interpretan los datos pudiendo haber, aunque sea no intencional, una interpretación de los datos a favor de su punto de vista.

---

## Qué hace el código, paso a paso

1. Importar librerías y cargar el CSV directamente desde la URL del INE.
2. Explorar el dataset: dimensiones, tipos, estadística básica y duplicados.
3. Evaluar la calidad: localizar nulos  (incluidos los no tan evidentes como `..`), visualizarlos y decidir qué hacer con ellos.
4. Limpiar y tipar: convertir `Total` a número, separar `Periodo` en año y mes.
5. Exportar el dataset limpio.
6. Consultar con SQL en SQLite: agregaciones, filtros, ordenación y un JOIN con una tabla auxiliar.
7. Explorar y visualizar con Pandas, Matplotlib y Seaborn.

## Qué aprende una persona al ejecutarlo

- No se puede analizar y sacar conclusiones ni recomendaciones de una tabla sin limpiar. La limpieza es una de las partes más importantes para analizar datos. Sin una buena limpieza los datos no puedes tener conclusiones sólidas.
- Que un dato real puede traer trampas (códigos especiales como `..`, formatos de número, columnas que mezclan unidades) y que limpiar es decidir, no solo borrar.
- A documentar el antes y el después de cada cambio.
- Interpretar gráficas de distintos tipos.
- A consultar SQL y a leer el resultado.
- A distinguir lo que el dato muestra (estacionalidad) de lo que se interpreta (precariedad o estabilidad laboral).
- A no confundir correlación con causalidad (no tiene por qué ser: a mayor cantidad de personas empleadas y mayor ocupación hotelera mayor estabilidad).
- La interpretación y las conclusiones están sesgadas por la manera en que pensamos sobre algunos temas. 
- A no usar la estadística para forzar que apoye nuestra hipótesis y conclusiones.
- Hay que tener paciencia, los errores pequeños (por ejemplo, escribir mal una palabra) pueden causar errores grandes en el código.

## Cómo usarlo en clase

- Habría que tener en cuenta la experiencia del alumnado. Un grupo donde hayan personas que no tengan nada de experiencia cometerán más errores. Harán el ejercicio y lo entenderán de manera más lenta que un grupo donde sí tengan conocimientos sobre el tema. 
- También es importante tener en cuenta el conocimiento sobre el dataset. Por muy experto que se sea en análisis de datos si no se sabe apenas nada sobre el dataset y lo que significa cada columna el análisis será de peor calidad que en uno donde sí se sepa (puede ocurrir con temas muy específicos o altamente técnicos).

La idea con esta actividad sería primero enseñar qué hace cada parte del código del notebook. 
En el notebook vienen comentarios sobre cada paso del código porque así serviría para todos los niveles y podrían revisarlo si lo necesitan. También, si lo deciden así, puede ser la base sobre la que monten todo el conocimiento que vayan adquiriendo sobre limpieza y EDA.

A medida que se va mostrando código se irían explicando conceptos de estadística, ética con los datos y posibles errores (como no poner el separador como ; en este caso) si se cambian algo del código. Se pueden resolver dudas si surgen.
También se pueden hacer preguntas con algún otro ejemplo para ir comprobando que van entendiendo lo que se explica. Intentando que se extrapole lo que se hace en este dataset a otros datasets.

Al llegar a las consultas SQL podemos cambiar parámetros para ver qué se obtiene. Se puede pensar en otras preguntas que se puedan resolver, etc.

A la hora de interpretar las visualizaciones se pueden hacer preguntas de verdadero o falso o alguna donde tengan que dar un valor numérico o categórico para ver si están entendiendo lo que representa la gráfica. También se puede preguntar directamente qué se obtiene de la gráfica en lugar de dar pequeñas pistas como con las preguntas. Se podría ir incrementando el nivel de dificultad según se vaya viendo que van entendiendo los conceptos. Si necesitan un refuerzo extra se podría hacer otra gráfica con otra columna o cambiando lo que se les dificulte para que lo vean con otro ejemplo.

Una buena manera de saber si lo han entendido bien es preguntarles y que den toda la explicación sobre la interpretación o la solución del código. Así se puede evaluar dónde necesitan refuerzo. También es importante intentar que participen todos o la mayoría y no solamente las mismas personas monopolizando la conversación. También puede funcionar que se lo expliquen los propios compañeros entre sí si tienen esa iniciativa.

## Qué puede salir mal y cómo solucionarlo