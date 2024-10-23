# <h1 align=center> **DATAFT25 - PROYECTO INDIVIDUAL Nº2** </h1>

<p align="center">
<img src="https://www.soyhenry.com/_next/static/media/HenryLogo.bb57fd6f.svg"  height=300>
</p>

# <h1 align=center>**Análisis de datos de Siniestros Viales en la Ciudad de Buenos Aires**</h1>

# **INTRODUCCIÓN y CONTEXTO:**

Buenos Aires, como una de las ciudades más grandes y transitadas de Argentina, enfrenta un gran desafío en cuanto a la seguridad vial. El Observatorio de Movilidad y Seguridad Vial (OMSV), un centro de estudios bajo la órbita de la Secretaría de Transporte del Gobierno de la Ciudad Autónoma de Buenos Aires, ha solicitado la elaboración de un proyecto de análisis de datos con el fin de generar información que permita a las autoridades locales tomar medidas para disminuir la cantidad de víctimas fatales en siniestros viales.

Este análisis busca dimensionar la problemática en la Ciudad de Buenos Aires, caracterizar los siniestros y perfilar a las víctimas. El objetivo es proporcionar datos vinculados a la siniestralidad vial fatal que sirvan como información oportuna y relevante para la toma de decisiones basadas en evidencia.

<p align="center">
<img src="./_src/65a9608980bddc671e0e35c6c6305607.png"  style="max-width: 100%; height: auto;">
</p>



# TABLA DE CONTENIDO
1. [Fuente de Datos](#1)
2. [Análisis de datos - ETL y EDA](#2)
3. [Principales Conclusiones de EDA](#3)
4. [Dashboard](#4) 
5. [Conclusiones Finales](#5)
6. [Posibles medidas para la reducción de Siniestros Viales](#6)
7. [Requisitos](#7)
8. [Estructura del Proyecto](#8)
9. [Contacto](#9)


# <h2 id="1">**1. Fuente de datos**</h2>
La información se obtuvo de la página oficial del [Gobierno de Buenos Aires](https://data.buenosaires.gob.ar/dataset/victimas-siniestros-viales):


- **`Tabla de Hechos:`** contiene la información temporal, espacial y los participantes de cada siniestro vial, indexada con un **`Id`**.
- **`Tabla de Víctimas:`** contiene la información de las víctimas involucradas en cada hecho, indexada por el  **`Id_hecho`**, permitiendo su relación con la tabla de Hechos.


# <h2 id="2">**2. Análisis de datos - ETL y EDA**</h2>

Se realizaron los siguientes pasos para el tratamiento y análisis de los datos:

1. **ETL:** Extracción, transformación y carga de datos.
2. **EDA:** Análisis exploratorio de los datos para obtener insights sobre la siniestralidad vial en la Ciudad de Buenos Aires.

Se presenta un [Notebook](./notebooks/ETL%20y%20EDA.ipynb) con el el proceso completo de ETL y EDA.

- En primer lugar se relizó la ingesta de los datos proporcionados y se elaboró un primer informe utilizando la librería **`ydata_profiling`**. Los datos fueron proporcionados por el Observatorio de Movilidad y Seguridad Vial de la Ciudad de Buenos Aire dependiente de la Secretaría de Transporte en un archivo llamado **`homicidios.xlsx`** que contiene la información de:
    - **`Hechos`**: Contiene información sobre los Siniestros Viales en el período 2016 - 2021.
    - **`Victimas`**: Contiene la información de las víctimas involucradas en los Siniestros Viales.
    - **`Bases de Siniestros Viales. Notas para su uso`**: contiene la información necesaria para interpretar las bases de datos.
    .
- Resumen de análisis de datos, **`df_hechos`**:
    - No se observan duplicados.
    - Los **`ID`** se encuentran correctamente normalizados, sin faltantes ni duplicados.
    - Se observa que los datos faltantes se completaron con el texto **`"SD"`**. Se verifica en **`Bases de Siniestros Viales. Notas para su uso`**. Se reemplazan estos valores por Nulos.
    - Campos de Fechas y Hora:
        - Se verificó el cálculo del Año, Mes, Día y Hora.
        - Se verificaron los datos de Fecha con el **`df_victimas`**, se encontró una diferencia y se la corrigó.
        - Se clasificaron los días tipos de día **`Laboral`** y **`No Laboral`**.
    - Campos de direcciones y coordenadas:
        - Se verificaron los nulos y faltantes.
        - Los campos **`Altura`** y **`Cruce`** tienen muchos faltantes pero resultan son complementarios.
        - Se creó una clasificación con el **`Tipo de Cruce`** para identificar si el accidente ocurrió en una Intersección o no.
        - Se creó la clasificació **`Calle_Cruce`** para identificar intersecciones con mayor cantidad de víctimas.
    - Campos de clasificación de **`Participantes`**, **`Víctimas`** y **`Acusados`**:
        - Se verificó la consistencia de las clasificaciones con los valores permitidos en **`Bases de Siniestros Viales. Notas para su uso`**.
        - Se reemplazó la clasificacón **`PASAJEROS`** en el campo **`Acusados`** según "Notas para el uso de la base". Por su definición, se entiende que como Víctima hace referencia a personas lesionadas que se encuentran dentro, descendiendo o ascendiendo de las unidades de autotrasporte público de pasajeros/as; como acusado contraparte, son vehículos de autotransporte público de pasajeros/as. Por lo tanto se dejará la clasificación de **`PASAJEROS`** en víctimas pero se reemplazará en el caso de Acusado por **`TRANSPORTE PÚBLICO`**. 
        - Se corrigen algunas clasficaciones para **`Id`** específicos.
    - Se verificó que el **`Número de víctimas`** de la Tabla **`Hechos`** coincide con el la sumatoria de cada **`Id`** de la tabla **`Víctimas`**.

- Resumen de análisis de datos, **`df_victimas`**:
    - Hay valores duplicados de **`Id`**, estos se deben corresponder con más de una **`Vícitima`** para el **`Id`** de **`Hechos`**..
    - Se verifica que todos los **`Id`** y **`Fehcas`** existan en la tabla de **`Hechos`**.
    - Se normalizan los campos de **`Edad`** y **`Fecha de fallecimiento`**
    - Los ID se encuentran correctamente normalizados, sin faltantes ni duplicados.
    - Se normalizaron los tipos de 
    - Se analizaron los valorse de Roles, Víctima y Sexo según "Notas para el uso de la base".

- Se crearon dos nuevos archivos con la información en **`datasets/2. Depurado`**.

- Se realizó el análisis Univariado, Bivariado y Multivariado con la siguientes conclusiones.

# <h2 id="3">**3. Principales Conclusiones de EDA**</h2>

Algunas de las conclusiones más relevantes obtenidas a partir del análisis de datos son:

- **Perfil temporal**: Se observa una reducción en la cantidad de siniestros desde el año 2019. No se detecta estacionalidad mensual significativa al excluir el año 2020 (pandemia) del análisis.
- **Perfil de las víctimas**: Las principales víctimas son motociclistas jóvenes, hombres entre 20 y 40 años, y peatones hombres entre 50 y 60 años.
- **Zonas de mayor incidencia**: El microcentro, especialmente en las comunas 1 y 4, concentra la mayor cantidad de accidentes durante días y horarios laborales.
- **Tipo de vías**: Los accidentes ocurren principalmente en intersecciones de calles y en avenidas.
- **Horarios de mayor incidencia**: Las horas pico de tránsito laboral, especialmente en avenidas principales, presentan el mayor número de siniestros.
- **Variaciones según el tipo de día**: En días no laborales, el horario más conflictivo es entre las 5:00 y las 7:00 horas, y las principales víctimas son jóvenes de entre 20 y 30 años.


#### **2.1. Cantidad de Víctimas por Año**
- El año con más cantidad de víctimas registrdas fue el 2018.
- Se nota un descenso de la cantidad de víctimas a partir del año 2019. Es probable que se hayan tomado medidas para disminuir la los Siniestros Viales. 
- Es probable que la disminución del año 2020 haya estado influenciada por la Pandemia.
<p align="center">
<img src="./_src/1. Cantidad de Víctimas por año.png"  style="max-width: 100%; height: auto;">
</p>


#### **2.2. Serie temporal de Cantidad de Víctimas por Año y Mes**
- Se verifica la reducción de víctimas durante el período marzo/junio 2020 durante la Pandemia.
- Se observa un gran incremento en diciembre 2020 lo que considerarse un outlier.
- Si se realiza un análisis social, el encierro podría haber producido que durante la liberación antes las fiestas de fin de año, las personas hayan tenido comportamientos más riesgosos lo que haya conducido a un incremento en las víctimas fatales por accidentes de tránsito.
<p align="center">
<img src="./_src/2. Serie temporal de Cantidad de Víctimas.png"  style="max-width: 100%; height: auto;">
</p>


#### **2.3. Cantidad de Víctimas por Hora del Día en días Laborales (sin incluir el año 2020)**
-	En días laborables, entre los rangos horarios de 07 a 11 horas y de 14 a 18 horas, se produce un aumento del promedio de víctimas.
-	Las medidas deberían estar orientadas a reducir los accidentes en estos rangos horarios.
<p align="center">
<img src="./_src/3. Cantidad de Víctimas por Hora del Día en días Laborales.png"  style="max-width: 100%; height: auto;">
</p>


#### **2.4. Cantidad de Víctimas por Hora del Día en días No Laborales (sin incluir el año 2020)**
- Se observa un rango de mayor cantidad de vícitimas fatales en días no laborales entre las 05 y 07 horas y para un rango etario entre 20 y 30 años, posiblemente asociado a víctimas por excesos durante el retorno de salidas nocturnas.
- Las medidas deberían estar orientadas a reducir los accidentes en este tipo de días y rango horario.

<p align="center">
<img src="./_src/4. Cantidad de Víctimas por Hora del Día en días No Laborales.png"   style="max-width: 100%; height: auto;">
</p>


#### **2.5. Cantidad de Víctimas por Edad**
-	Se observa que la mayoría de víctimas tienen entre 20 y 40 años
-	Entre 70 y 80 años se vuelve a dar un máximo.
-	Se observan pocas víctimas infantiles.
<p align="center">
<img src="./_src/5. Cantidad de Víctimas por Edad.png"   style="max-width: 100%; height: auto;">
</p>


#### **2.6. Cantidad de Víctimas por Tipo de Cruce**
-	Se identifica que la mayoría de los siniestros ocurren en Intersecciones de Calles.
-	Se deberían tomar medidas para reducir los siniestros viales en Intersecciones, ejemplo, colocación de semáforos.
<p align="center">
<img src="./_src/6. Cantidad de Víctimas por Tipo de Cruce.png"   style="max-width: 100%; height: auto;">
</p>


#### **2.7. Mapa de calor de Víctimas por accidente de tránsito por Tipo de Cruce y por Comuna**
-	Se utiliza el Shapefile de: https://data.buenosaires.gob.ar/dataset/comunas/resource/Juqdkmgo-612222-resource
-	Como se puede observar en el mapa de calor, la mayoría de las víctimas ocurren en las Comuna 1 y 4, en relación a la zona céntrica de la Ciudad, probablemente la que mayor tráfico tenga. Las medidas podrían focalizarse en estas zonas geográficas.
-	En el mapa por puntos se corrobora lo mostrado en el gráfico de accidentes por Tipo de calle, que la mayor cantidad de accidentes suceden en Avenidas y Calles dentro de la ciudad.
<p align="center">
<img src="./_src/7. Mapa de Calor.png"   style="max-width: 100%; height: auto;">
</p>


#### **2.8. Cantidad de Víctimas por Tipo de Víctima**
- Como se puede observar, la mayor cantidad de víctimas son motociclistas o peatones.
<p align="center">
<img src="./_src/8. Cantidad de Víctimas por Tipo de Víctima.png"   style="max-width: 100%; height: auto;">
</p>


#### **2.9. Gráfico 17: Cantidad de Víctimas por Tipo de Acusado**
- Como se puede observar, la mayor cantidad de contrapartes son los Autos, el Transporte Público y los transportes de Carga.
<p align="center">
<img src="./_src/9. Cantidad de Víctimas por Tipo de Acusado.png"   style="max-width: 100%; height: auto;">
</p>


#### **2.9. Gráfico 18: : Heatmap de Víctima vs Acusado**
- Se detecta que la mayor cantidad de víctimas son Petones causados por accidentes de tránsito en el medio de Transporte Público.
- En segundo lugar se destaca la cantidad de muertes de Peatones y Motos causadas por accidentes con Autos y por Transportes de Carga.
<p align="center">
<img src="./_src/10. Heatmap de Víctima vs Acusado.png"   style="max-width: 100%; height: auto;">
</p>

# <h2 id="4">**4. Dashboard**</h2>
El Dashboard interactivo desarrollado en Power BI presenta visualizaciones y estadísticas claves para entender la siniestralidad vial en la Ciudad de Buenos Aires, segmentadas por fecha, comuna, tipo de vehículo y rol de las víctimas.

**OBJETIVOS**

- ¿Cuál es el estado de situación actual en cuanto a la cantidad de accidentes viales en la ciudad de Buenos Aires respecto de la media nacional?
- ¿Cómo ha sido la evolución temporal de los siniestros viales en el período?
- ¿Cuál es la caracterización de los siniestros viales en cuanto a sus participantes?
- ¿Cuáles son las zonas geográficas donde se produce la mayor cantidad de siniestros viales? 
- ¿Cuáles son los días de la semana, tipos de día y rangos horarios donde se producen la mayor cantidad de siniestros viales?
- ¿Cuáles son las relaciones entre víctima y acusado?
- ¿Cuáles son las edades de las víctimas?

**DATOS ADICIONALES**

Para poder calcular métricas y realizar análisis adicionales fue necesario importar algunos datos adicionales:
- Cantidad de Población por Año y por Comuna: https://www.estadisticaciudad.gob.ar/eyc/?p=28146
- Mapa de comuna SHP: https://data.buenosaires.gob.ar/dataset/comunas/resource/Juqdkmgo-612222-resource
- Datos de días No Laborables de Argentina a través de la API de https://nolaborables.com.ar/
- Paleta de colores según el [Manual de Marca del GCBA](./Manual%20de%20marca%20GCBA/Manual%20de%20marca%20GCBA.pdf)
- Tasa Víctimas cada 100 mil Habitantes de Argentina del [Anuario Estadístico de Siniestralidad Fatal Año 2021](./Reportes%20Siniestros%20Viales%20Argentina/anuario_estadistico_2021.pdf) de la Dirección Nacnioal de OBservatorio Vial.


Como resultado se conformaron 3 Dashboards:

**OVERVIEW**

- Un Overview que permita tener una primera aproximación general.
<p align="center">
<img src="./_src/11. Dashboard 1.PNG"   style="max-width: 100%; height: auto;">
</p>

**GEORREFERENCIACIÓN**

- Un dashboard de Georreferencicación que permitiera ver la ubicación geográfica de los siniestros viales y realizar un análisis pormenorizado de los focos de accidentes.
<p align="center">
<img src="./_src/12. Dashboard 2.PNG"  style="max-width: 100%; height: auto;">
</p>

**KPIs**

- Un dashboards de KPIs que nos permita evaluar cómo fueron evolucionan las métricas requeridas, considerando:
    - KPI 1: Reducir en un 10% la tasa semestral de siniestros viales.
    - KPI 2: Reducir en un 7% la tasa anual de siniestros viales de motociclistas.
    - KPI 3: Reducir en un 5% la tasa anual de víctimas por siniestros viales en días No Laborales entre las 05 y 07 horas.

<p align="center">
<img src="./_src/13. Dashboard 3.PNG"   style="max-width: 100%; height: auto;">
</p>

# <h2 id="5">**5. Conclusiones Finales**</h2>

El análisis de siniestralidad vial en la Ciudad de Buenos Aires ha permitido identificar patrones claros que pueden ayudar en la implementación de políticas públicas orientadas a la reducción de siniestros. 

- Desde el año 2019 se ha logrado reducir la cantidad de víctimas fatales por accidentes de tránsito en la Ciudad de Buenos Aires.
- El año 2020 resultó un año atípico en el que las restricciones de la circulación por la Pandemia produjeron una reducción en la cantidad de Siniestros Viales.
- La Tasa de Siniestralidad por cada 100 mil Habitantes del año 2021 de la Ciudad de Buenos Aires se encuentra por debajo de la media nacional.
- La variación de la Tasa de Siniestralidad Semestral (KPI1) y de la Tasa de accidentes de Motociclistas como Víctimas (KPI2) está fuertemente influenciada por tomar como referencia el año 2020 el cual fue de Pandemia. 
- Las principales víctimas son los conductores de Moto y los Peatones (representando más del 80%) y los principales causantes de los siniestros son los Autos y Transporte de Carga (representando el 70%). Se ha logrado una reducción significativa de accidentes de tránsito causados por el Transporte Público desde el año 2019.
- La mayoría de los Siniestros Viales se producen en intersecciones de calles y principalmente en Avenidas.
- La principal zona de accidentes fatales es el microcentro (Comuna 1) en los días y horarios Laborales.
- En días No Laborales las principales zonas de conflicto son la Comuna 8 que incluye los barrios de Villa Soldati, Villa Riachuelo y Villa Lugano y la Comuna 11 que incluyen los barrios de Villa General Mitre, Villa Devoto, Villa del Parque y Villa Santa Rita.

# <h2 id="6">**6. Posibles medidas para la reducción de Siniestros Viales**</h2>

Algunas de las recomendaciones propuestas incluyen:

- **Restricciones al transporte**: Limitar el transporte vehicular particular y de carga durante los días laborales en la zona céntrica.
- **Mejora de infraestructura**: Mejorar la señalización y la semaforización en intersecciones y avenidas principales.
- **Controles de tránsito**: Reforzar controles de alcoholemia y velocidad, especialmente en días no laborales y durante la madrugada en zonas conflictivas como Villa Soldati, Villa Riachuelo, Villa Lugano, Villa General Mitre, Villa Devoto, Villa del Parque y Villa Santa Rita.
- **Educación vial**: Implementar campañas de concientización enfocadas en motociclistas y conductores jóvenes.


# <h2 id="7">**Requisitos**</h2>
- Python 3.7 o superior
- pandas.
- ydata_profiling.
- matplotlib.
- seaborn.
- gopandas.
- contextliy.
- Power BI.


# <h2 id="8">**Estructura del Proyecto**</h2>
- `_src/`: Imágenes y archivo Shape Ciudad de Buenos Aires.
- `datasts/`: Archivos de datos utilizados.
- `notebooks/`: Jupyter Notebooks con el análisis de los datos.
- `ProfileReports/`: Reportes generados con la librería `ydata_profiling`
- `Reportes Siniestros Viales Argentina/`: Bibliografía utilizada sobre la estadística de siniestros viales en Argentina.
- `Manual de marca GCBA/`:  Manual de marca y colores utilizado según las directrices de la Ciudad de Buenos Aires.
- `Readme.md`: Documentación del proyecto.
- `PI02_Dashboard Siniestros Viales CBA.pbix`: Dashboard desarrollado en Power BI.

# <h2 id="9">**Contacto**</h2>
- Nombre compelto: Hernán Lussiatti
- Mail: hernanlussiatti@gmail.com