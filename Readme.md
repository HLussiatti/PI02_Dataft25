# <h1 align=center> **DATAFT25 - PROYECTO INDIVIDUAL Nº2** </h1>

<p align="center">
<img src="https://www.soyhenry.com/_next/static/media/HenryLogo.bb57fd6f.svg"  height=300>
</p>

# <h1 align=center>**Análisis de datos de Siniestros Viales en la Ciudad de Buenos Aires**</h1>

# **CONSIGNA:**

Elaboración de un proyecto de anális de datos, con el fin de generar información que le permita a las autoridades locales tomar medidas para disminuir la cantidad de víctimas fatales de los siniestros viales


# TABLA DE CONTENIDO
1. [ETL y EDA](#1-etl-y-eda*)
2. [Principales Conclusiones de EDA](#2-principales-conclusiones-de-eda*)
3. [Dashboard](#3-dashboard)
4. [Conclusiones Finales](#4-conclusiones-finales)
5. [Requisitos](#requisitos)
6. [Estructura del Proyecto](#estructura-del-proyecto)


# **TAREAS REALIZADAS:**
# <h3>**1. ETL y EDA**</h3>

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

# <h3>**2. Principales Conclusiones de EDA**</h3>

#### **2.1. Cantidad de Víctimas por Año**
- El año con más cantidad de víctimas registrdas fue el 2018.
- Se nota un descenso de la cantidad de víctimas a partir del año 2019. Es probable que se hayan tomado medidas para disminuir la los Siniestros Viales. 
- Es probable que la disminución del año 2020 haya estado influenciada por la Pandemia.
<p align="center">
<img src="./_src/1. Cantidad de Víctimas por año.png"  height=400>
</p>


#### **2.2. Serie temporal de Cantidad de Víctimas por Año y Mes**
- Se verifica la reducción de víctimas durante el período marzo/junio 2020 durante la Pandemia.
- Se observa un gran incremento en diciembre 2020 lo que considerarse un outlier.
- Si se realiza un análisis social, el encierro podría haber producido que durante la liberación antes las fiestas de fin de año, las personas hayan tenido comportamientos más riesgosos lo que haya conducido a un incremento en las víctimas fatales por accidentes de tránsito.
<p align="center">
<img src="./_src/2. Serie temporal de Cantidad de Víctimas.png"  height=400>
</p>

#### **2.3. Cantidad de Víctimas por Hora del Día en días Laborales (sin incluir el año 2020)**
-	En días laborables, entre los rangos horarios de 07 a 11 horas y de 14 a 18 horas, se produce un aumento del promedio de víctimas.
-	Las medidas deberían estar orientadas a reducir los accidentes en estos rangos horarios.
<p align="center">
<img src="./_src/3. Cantidad de Víctimas por Hora del Día en días Laborales.png"  height=400>
</p>


#### **2.4. Cantidad de Víctimas por Hora del Día en Fines de Semana (sin incluir el año 2020)**
- El incremento del promedio de víctimas entre las 03 y 09 horas podría estar asociado a víctimas por excesos durante el retorno de fiestas. Verificar rango etario
- Las medidas deberían estar orientadas a reducir los accidentes en el rango horario de 05 a 07 horas.
- Se observa un rango de mayor cantidad de vícitimas fatales en días no laborales entre las 05 y 07 horas y para un rango etario entre 20 y 30 años.
<p align="center">
<img src="./_src/4. Cantidad de Víctimas por Hora del Día en días No Laborales.png"  height=400>
</p>


#### **2.5. Cantidad de Víctimas por Edad**
-	Se observa que la mayoría de víctimas tienen entre 20 y 40 años
-	Entre 70 y 80 años se vuelve a dar un máximo.
-	Se observan pocas víctimas infantiles.
<p align="center">
<img src="./_src/5. Cantidad de Víctimas por Edad.png"  height=400>
</p>


#### **2.6. Cantidad de Víctimas por Tipo de Cruce**
-	Se identifica que la mayoría de los siniestros ocurren en Intersecciones de Calles.
-	Se deberían tomar medidas para reducir los siniestros viales en Intersecciones, ejemplo, colocación de semáforos.
<p align="center">
<img src="./_src/6. Cantidad de Víctimas por Tipo de Cruce.png"  height=400>
</p>


#### **2.7. Mapa de calor de Víctimas por accidente de tránsito por Tipo de Cruce y por Comuna**
-	Se utiliza el Shapefile de: https://data.buenosaires.gob.ar/dataset/comunas/resource/Juqdkmgo-612222-resource
-	Como se puede observar en el mapa de calor, la mayoría de las víctimas ocurren en las Comuna 1 y 4, en relación a la zona céntrica de la Ciudad, probablemente la que mayor tráfico tenga. Las medidas podrían focalizarse en estas zonas geográficas.
-	En el mapa por puntos se corrobora lo mostrado en el gráfico de accidentes por Tipo de calle, que la mayor cantidad de accidentes suceden en Avenidas y Calles dentro de la ciudad.
<p align="center">
<img src="./_src/7. Mapa de Calor.png"  height=600>
</p>


#### **2.8. Cantidad de Víctimas por Tipo de Víctima**
- Como se puede observar, la mayor cantidad de víctimas son motociclistas o peatones.
<p align="center">
<img src="./_src/8. Cantidad de Víctimas por Tipo de Víctima.png"  height=400>
</p>


#### **2.9. Gráfico 17: Cantidad de Víctimas por Tipo de Acusado**
- Como se puede observar, la mayor cantidad de contrapartes son los Autos, el Transporte Público y los transportes de Carga.
<p align="center">
<img src="./_src/9. Cantidad de Víctimas por Tipo de Acusado.png"  height=400>
</p>


#### **2.9. Gráfico 18: : Heatmap de Víctima vs Acusado**
- Se detecta que la mayor cantidad de víctimas son Petones causados por accidentes de tránsito en el medio de Transporte Público.
- En segundo lugar se destaca la cantidad de muertes de Peatones y Motos causadas por accidentes con Autos y por Transportes de Carga.
<p align="center">
<img src="./_src/10. Heatmap de Víctima vs Acusado.png"  height=600>
</p>

# <h3>**3. Dashboard**</h3>
 Una vez realizado el ETL y el EDA se elaboró un esquema de objetivos a alcanzar para la conformación un Storytelling que permitirera focalizar el análisis realizado en función de los resultados que se desean obtener.

**OJETIVOS**

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
- Paleta de colores según el Manual de Marca del GCBA.
- Tasa Víctimas cada 100 mil Habitantes de Argentina del "Anuario Estadístico de Siniestralidad Fatal Año 2021" de la Dirección Nacnioal de OBservatorio Vial.


Como resultado se conformaron 3 Dashboards:

- Un Overview que permita tener una primera aproximación general.
<p align="center">
<img src="./_src/11. Dashboard 1.png"  height=400>
</p>

- Un dashboard de Georreferencicación que permitiera ver la ubicación geográfica de los siniestros viales y realizar un análisis pormenorizado de los focos de accidentes.
<p align="center">
<img src="./_src/12. Dashboard 2.png"  height=400>
</p>

- Un dashboards de KPIs que nos permita evaluar cómo fueron evolucionan las métricas requeridas, considerando:
    - KPI 1: Reducir en un 10% la tasa semestral de siniestros viales.
    - KPI 2: Reducir en un 7% la tasa anual de siniestros viales de motociclistas.
    - KPI 3: Reducir en un 5% la tasa anuak de víctimas por siniestros viales en días No Laborales entre las 05 y 07 horas.

<p align="center">
<img src="./_src/13. Dashboard 3.PNG"  height=400>
</p>

# <h3>**4. Conclusiones Finales**</h3>
- Desde el año 2019 se ha logrado reducir la cantidad de víctimas fatales por accidentes de tránsito en la Ciudad de Buenos Aires.
- El año 2020 resultó un año atípico en el que las restricciones de la circulación por la Pandemia produjeron una reducción en la cantidad de siniestros viales.
- La Tasa de Siniestralidad por cada 100 mil Habitantes del año 2021 de la Ciudad de Buenos Aires se encuentra por debajo de la media nacional.
- La variación de la Tasa de Siniestralidad Semestral (KPI1) y de la Tasa de accidentes de Motociclistas como Víctimas (KPI2) está fuertemente influenciada por tomar como referencia el año 2020 el cual fue de Pandemia. 
- Las principales víctimas son los conductores de Moto y los Peatones y los principales causantes de los siniestros son los Autos y Transporte de Carga. Se ha logrado una reducción significativa de accidentes de tránsito causados por el Transporte Público desde el año 2019.
- La principal zona de accidentes fatales es el microcentro (Comuna 1) en los días y horarios Laborales.
- En días No Laborales las principales zonas de conflicto son la Comuna 8 que incluye los barrios de Villa Soldati, Villa Riachuelo y Villa Lugano y la Comuna 11 que incluyen los barrios de Villa General Mitre, Villa Devoto, Villa del Parque y Villa Santa Rita.


# <h3>**Requisitos**</h3>
- Python 3.7 o superior
- pandas
- ydata_profiling
- matplotlib
- seaborn
- gopandas
- contextliy
- Power BI.


# <h3>**Estructura del Proyecto**</h3>
- `_src/`: Imágenes utilizadas y archivo Shape Ciudad de Buenos Aires.
- `datasts/`: Contiene los archivos de datos utilizados
- `notebooks/`: Jupyter notebooks con el análisis.
- `ProfileReports/`: Con los reportes realizados con la librería `ydata_profiling`
- `Reportes Siniestros Viales Argentina/`: Con bibliografía utilizada de la estadística de siniestros viales de Argentina
- `Manual de marca GCBA/`: Con el manual de marcas y el tema de colores utilizado según el manual de marca de la Ciudad de Buenos Aires.
- `Readme.md`: Documentación.
