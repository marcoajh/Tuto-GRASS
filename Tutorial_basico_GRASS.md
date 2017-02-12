

# Manejo básico de GRASS  
## Entrada, salida y consulta de datos  

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


## Ejercicios:

### Iniciar GRASS
1. Abrir GRASS en windows  
Clic en el boton de windows (inicio) y buscar grass, abrir *GRASS GIS 7.2.0*  

2. Identifiar los componentes de la interfaz
 * Administrador de mapas
 * Visualizador de mapas
 * Consola
 * Menús
 * Botones
 * Pestañas


### Visualizar mapas ráster y vectorial
1. Cargar mapas  
 a. Cargar el mapa raster *Elevacion*, que se encuentra en PERMANENT  
    ```d.rast Elevacion@PERMANENT```   
 b. Cargar el mapa vectorial *Cuencas*, que se encuentra en PERMANENT  
     ```d.vect Cuencas@PERMANENT```   
2. Copiar mapas  
 a. Copiar el raster *AGEB* que se encuentra en PERMANENT  
    ```g.copy raster=AGEB@PERMANENT,AGEB```  
 b. Copiar el vectorial *Cuencas* que se encuentra en PERMANENT  
    ```g.copy vector=Cuencas@PERMANENT,Cuencas```

### Importar un mapa raster
Importar la capa raster *Percolacion*   
```r.import input=C:\GIS_DB\Percolacion.tif output=Percolacion```

### Importar un mapa vectorial
Importar la capa vectorial *Hundimiento*   
```v.import input=C:\GIS_DB\Hundimiento.shp layer=Hundimiento output=Hundimiento```     

### Transformar un mapa vectorial a raster
Revisar la tabal de datos de la capa, identificar los campos numéricos y los de texto.  
Convertir la capa de *Cuencas* a raster   
```v.to.rast input=Cuencas output=Cuencas use=attr attribute_column=cat label_column=LAYER```   
Convertir la capa *Hundimiento* a raster   
```v.to.rast input=Hundimiento@Marco output=Hundimiento use=attr attribute_column=HUNDIM_NUM label_column=HUNDIM_LEY```  

El tipo de datos de la capa *Hundimiento* debe ser "entero"   
```r.info Hundimiento```   
```r.mapcalc "Hundimiento_int = int( Hundimiento )"```   



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
```r.stats -lan Cuencas separator=, > areas_cuencas.csv```     
### Obtener estadísticas cruzadas con dos capas
Reportar las AGEB que presentan el fenónemo de hundimiento.  
Guardar la salida como archivo separado por comas.  
```r.stats -ln AGEB,Hundimiento_int separator=, > ageb_hund.csv```   

#### Reportar la percolacion promedio en cada cuenca  

Calcular el promedio usando *r.statistics*   
```r.statistics base=Cuencas@Marco cover=Percolacion@Marco method=average output=cuencas_perc```   
Obtener un archivo de valores separados por comas   
```r.stats -ln cuencas_perc separator=, > Percolacion_promedio.csv```  
