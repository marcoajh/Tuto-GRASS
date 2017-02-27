



# Manejo básico de GRASS  
## Importacion y exportacion de archivos y consulta de datos  

### Contenido de PERMAMENT:
#### Raster
Elevacion  
Cuencas  
AGEB  
Municipios  

#### Vectorial
Cuencas  
AGEB  
Municipios  


### ¿Qué es GRASS?  
**GRASS GIS** (Geographic Resources Analysis Support System) es un Sistema de Información Geográfica (GIS) de código abierto (software libre).  Puede soportar información tanto raster como vectorial y posee herramientas de procesamiento digital de imágenes.


### ¿Qué son LOCATION y MAPSET?  
Una LOCATION es una extensión geográfica de interés que contiene conjuntos de datos que deben estar en el mismo sistema de coordenadas. Cada LOCATION tiene un directorio PERMANENT donde se almacena información básica sobre toda la LOCATION y es un buen lugar para colocar archivos base. Podemos pensar en una LOCATION como una biblioteca de datos para una región de interés.

En un MAPSET, se pueden organizar mapas temáticamente, geográficamente, por proyecto, por usuario o como sea. Cada sesión de GRASS se ejecuta bajo el nombre de un MAPSET. Un MAPSET puede ser un tan grande como el LOCATION o abarcar una region menor. Técnicamente son subdirectorios dentro de una LOCATION. En un entorno de red con varios usuarios que trabajan en la misma LOCATION, los MAPSET desempeñan un papel especial. Los usuarios sólo pueden seleccionar (y por lo tanto modificar) un conjunto de mapas que poseen (es decir, que han creado). Sin embargo, los datos de todos los conjuntos de mapas para una LOCATION determinada pueden ser leídos por cualquier persona (a menos que se impida mediante permisos de archivo UNIX).   
El MAPSET "PERMANENT" generalmente contiene los mapas de base de sólo lectura como el modelo de elevación, mientras que los otros lugares son legibles y escribibles por sus propietarios. También contiene alguna información sobre la ubicación misma que no se encuentra en otros mapas (información de proyección, etc.), por lo que debe existir en cada LOCATION.



 #### Estructura de archivos de GRASS   

   ![Estructura LOCATION GRAS](https://github.com/marcoajh/Tuto-GRASS/blob/master/estructura_loc.png "Estructura de archivos de una LOCATION en GRASS")

Un mapa ráster de GRASS consiste de varios archivos en varios subdirectorios dentro de un MAPSET, con la siguiente organización
*	**cellhd/**: encabezado del mapa, incluye el código de la proyección, las coordenadas extremas del mapa ráster, número de filas, número de columnas, resolución e información acerca de la compresión.  
*	**cell/**, **fcell/** o **g3dcell/**: Matriz genérica de valores en un formato portable y comprimido que depende del tipo de datos del ráster (entero, punto flotante o 3D grid).  
*	**hist/**: Archivo de historial que contiene metadatos tales como la fuente de los datos, el comando que se usó para generar el mapa ráster o información determinada por el usuario.    
*	**cats/**: Archivo opcional de categorías el cual contiene etiquetas de texto o numéricas asignadas a las categorías del mapa ráster.    
*	**colr:**: Archivo opcional con una tabla de colores   
*	**cell_misc:**: Fecha y hora, rango de valores   

Un mapa vectorial de GRASS está almacenado en varios archivos separados en un solo directorio. Mientras que los atributos están almacenados en un archivo SQLite los datos geométricos son almacenados con la siguiente organización:
*	**head:** Encabezado ascii del mapa vectorial con información sobre el origen del mapa (fecha y nombre), escala y umbral.
*	**coor:** Archivo binario de la geometría, que incluye las coordenadas de los elementos gráficos (primitivos) que definen las características del mapa vectorial.
*	**topo:** Archivo binario de la topología que describe las relaciones espaciales entre los elementos gráficos del mapa vectorial.  
*	**hist:** Archivo historial ASCII con los comandos que fueron usados para crear el mapa vectorial así como el nombre y hora y fecha de creación.  
*	**cidx:** Archivo binario, índice de las categorías que se usa para vincular ID’s de objetos a la tabla de atributos.
*	**dbln:** Archivo ASCII que contiene la definición del enlace a atributos almacenados en la base de datos (DBMS).
* **sidx** Archivo binario, índice espacial  
* **frmt** Archivo de texto, descripción del formato  
* **fidx** Archivo binario, índice de atrinbutos (sólo en el formato OGR)



## Ejercicios:

### Iniciar GRASS
1. Abrir GRASS en windows  
Clic en el botón de windows (inicio) y buscar grass, hacer click en *GRASS GIS 7.2.0* para abrir.  

   ![Abrir GRASS](https://github.com/marcoajh/Tuto-GRASS/blob/master/abrir_grass.PNG "Abrir GRASS desde el menú inicio")

2. Identifiar los componentes de la interfaz  
 * Administrador de mapas  
 * Visualizador de mapas  
 * Consola  
 * Menús  
 * Botones  
 * Pestañas   


   ![Pantalla con los componentes de la interfaz de GRASS](https://github.com/marcoajh/Tuto-GRASS/blob/master/interfaz.PNG "Pantalla con los componentes de la interfaz de GRASS, Administrador de capas, Visualizador de mapas y Consola de comandos")

### Visualizar mapas ráster y vectorial

1. Visualizar las lista de mapas disponibles  
  a. Desplegar la lista de capas raster disponibles  
  ```g.list rast```   
  b. Desplegar la lista de capas vectoriales disponibles  
  ```g.list vect```

  ![g.list rast y g.list vect](https://github.com/marcoajh/Tuto-GRASS/blob/master/g.list.PNG "Visualización de los mapas raster y vectorial disponibles usando g.list")


2. Cargar mapas  
 a. Cargar el mapa raster *Elevacion*, que se encuentra en PERMANENT  
    ```d.rast Elevacion@PERMANENT```   

      ![d.rast](C:\GIS_DB\imagenes\d.rast.png "Uso del comando d.rast en la consola del administrador de capas")

 b. Cargar el mapa vectorial *Cuencas*, que se encuentra en PERMANENT  
     ```d.vect Cuencas@PERMANENT```   

  ![d.rast](https://github.com/marcoajh/Tuto-GRASS/blob/master/d.vect.PNG "Uso del comando d.vect en la consola del administrador de capas")

3. Copiar mapas  
 a. Copiar el raster *AGEB* que se encuentra en PERMANENT  
    ```g.copy raster=AGEB@PERMANENT,AGEB```  
 b. Copiar el vectorial *Cuencas* que se encuentra en PERMANENT  
    ```g.copy vector=Cuencas@PERMANENT,Cuencas```

  ![g.copy](https://github.com/marcoajh/Tuto-GRASS/blob/master/g.copy.PNG "Uso del comando g.copy en la consola del administrador de capas")


### Importar un mapa raster
Importar la capa raster *Percolacion*   
```r.import input=C:\GIS_DB\Percolacion.tif output=Percolacion```

### Importar un mapa vectorial
Importar la capa vectorial *Hundimiento*   
```v.import input=C:\GIS_DB\Hundimiento.shp layer=Hundimiento output=Hundimiento```     

### Transformar un mapa vectorial a raster
Revisar la tabal de datos de la capa, identificar los campos numéricos y los de texto.  

  ![Datos del atributo 1](https://github.com/marcoajh/Tuto-GRASS/blob/master/rev_tabla_1.PNG "Visualización de la tabla de atributos de un archivo vectorial")

  ![Datos del atributo 2](https://github.com/marcoajh/Tuto-GRASS/blob/master/rev_tabla_2.PNG "Visualización del tipo de datos en la tabla de un archivo vectorial")

Convertir la capa de *Cuencas* a raster   
```v.to.rast input=Cuencas output=Cuencas use=attr attribute_column=cat label_column=LAYER```   

Convertir la capa *Hundimiento* a raster   
```v.to.rast input=Hundimiento@Marco output=Hundimiento use=attr attribute_column=HUNDIM_NUM label_column=HUNDIM_LEY```  


### Transformar un mapa raster a vectorial
Convertir la capa raster *Percolacion* a vectorial  
```r.to.vect input=Percolacion output=Percolacion type=area column=Porcentaje```   

### Exportar un mapa ráster
Exportar el raster *hundimiento*  
```r.out.gdal input=Hundimiento output=C:\GIS_DB\Hundimiento.tif format=GTiff```

### Exportar un mapa vectorial  
Exportar el vectorial de *Cuencas*  
```v.out.ogr input=Cuencas@Marco output=C:\GIS_DB\Cuencas format=ESRI_Shapefile```  

### Obtener estadísticas de una capa
Reportar el área de cada cuenca  
```r.stats -lan Cuencas```   
Guardar la salida como archivo de texto   
```r.stats -lan Cuencas separator=, > D:/areas_cuencas.csv```     
### Obtener estadísticas cruzadas con dos capas
Reportar las AGEB que presentan el fenónemo de hundimiento y guardar la salida como archivo separado por comas.  
```r.stats -ln AGEB,Hundimiento separator=,```   

  ![salida de r.stats 1](https://github.com/marcoajh/Tuto-GRASS/blob/master/stats_hund.PNG "Salida de r.stats con una capa CELL y otra DCELL")

El tipo de datos de la capa *Hundimiento* debe ser "entero"   
```r.info Hundimiento```   
```r.mapcalc "Hundimiento_int = int( Hundimiento )"```

```r.stats -ln AGEB,Hundimiento_int separator=,```   

  ![salida de r.stats 2](https://github.com/marcoajh/Tuto-GRASS/blob/master/stats_hund2.PNG "Salida de r.stats con dos capas CELL (enteros)")

```r.stats -ln AGEB,Hundimiento_int separator=, > D:/ageb_hund.csv```


#### Reportar la percolacion promedio en cada cuenca  

Calcular el promedio usando *r.statistics*   
```r.statistics base=Cuencas@Marco cover=Percolacion@Marco method=average output=cuencas_perc```   
Obtener un archivo de valores separados por comas   
```r.stats -ln cuencas_perc separator=, > D:/Percolacion_promedio.csv```  
