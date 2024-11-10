<div style="background-color: #333333; color: #D1D1D1; padding: 20px; font-family: Arial, sans-serif;">

# Universidad de San Carlos de Guatemala  
**Facultad de Ingeniería**  
**Escuela de Estudios de Postgrado**  
**Maestría en Ingeniería para la Industria con Especialización en Ciencias de la Computación**  
**David Andrés Alcázar Escobar**  
</div>

## Bibliotecas Utilizadas

Para este proyecto, se emplearon las siguientes bibliotecas en R:

- `arules`: Para generación de reglas de asociación.
- `dplyr`: Para manipulación de datos.
- `ggplot2`: Para visualización de datos.
- `ggalt`: Para crear gráficos avanzados, como círculos de los clústeres.

## Instalación de Paquetes
Para instalar las bibliotecas necesarias en caso de no poseerlas instaladas, ejecute el siguiente código linea por linea en su entorno R. Esto incluye la instalación de `fim4r` desde una fuente local y la configuración de la ruta para `rtools40`, que es necesaria para compilar ciertos paquetes desde la fuente.

```r
# Configuración de PATH para rtools40
Sys.setenv(PATH = paste("C:/rtools40/usr/bin;C:/rtools40/ucrt64/bin", Sys.getenv("PATH"), sep = ";"))

# Verificación de la herramienta 'make' necesaria para compilar paquetes desde la fuente
Sys.which("make")

# Instalación del paquete fim4r desde una fuente local
install.packages("C:\\Users\\dalca\\Downloads\\db_csv_\\fim4r_1.8\\fim4r", 
                 repos = NULL, 
                 type = "source")

# Instalación de paquetes adicionales necesarios
install.packages('arules')
install.packages('dplyr')
install.packages("ggplot2")
install.packages("ggalt")
```

## Carga de Bibliotecas
Después de instalar los paquetes, cargue las bibliotecas en su entorno de trabajo con el siguiente código:

```r
library(ggalt)
library(ggplot2)
library(arules)
library(dplyr)
```
## Limpieza de Datos
La limpieza de datos implica la importación de archivos CSV y la manipulación de columnas para adecuarlas al análisis. 
Actualizar la ubicación de ambos archivos CSV (`Importaciones.csv` y `Catalogo Pais-Continente.csv`) según la ubicación donde se haya descargado el repositorio en su equipo.

```r
data_importacion = read.csv('C:\\Users\\dalca\\Documents\\Usac - Maestria\\2024\\Cursos\\Cuarto Trimeste\\Mineria de datos\\Proyecto 1\\95_2024_4_04_MIICC408_A_Proyecto1\\Datos\\Importaciones.csv', sep=',', fileEncoding = "latin1")
catalogo_continente = read.csv('C:\\Users\\dalca\\Documents\\Usac - Maestria\\2024\\Cursos\\Cuarto Trimeste\\Mineria de datos\\Proyecto 1\\95_2024_4_04_MIICC408_A_Proyecto1\\Datos\\Catalogo Pais-Continente.csv', sep=',', fileEncoding = "UTF-8")

# Renombrar columnas y unificar fechas
names(catalogo_continente) <- c("Codigo", "COD_CONTINENTE","CONTINENTE", "NOMBRE_PAIS")
data_importacion <- data_importacion %>%
  mutate(ANYO_MES = paste(ANYO, MES, sep = "-")) %>%
  select(-ANYO, -MES)
data_importacion <- merge(data_importacion, catalogo_continente, by.x = "PAIS", by.y = "Codigo", all.x = TRUE)

# Corrección de valores nulos
data_importacion <- data_importacion %>%
  mutate(
    NOMBRE_PAIS = case_when(
      PAIS == 2603 ~ "2603",
      PAIS == 4229 ~ "4229",
      TRUE ~ NOMBRE_PAIS
    ),
    COD_CONTINENTE = case_when(
      PAIS == 2603 ~ 1,
      PAIS == 4229 ~ 2,
      TRUE ~ COD_CONTINENTE
    ),
    CONTINENTE = case_when(
      PAIS == 2603 ~ "América",
      PAIS == 4229 ~ "Europa",
      TRUE ~ CONTINENTE
    )
  )
data_importacion$VALOR <- as.numeric(gsub(",", "", data_importacion$VALOR))
```
### Operador `%>%` (Pipe)

El operador `%>%`, conocido como "pipe" o canalización, es una funcionalidad proporcionada por el paquete `dplyr` en R, que facilita la escritura y lectura de código al permitir encadenar funciones. 

El operador `%>%` toma el resultado de la expresión a la izquierda y lo pasa como el primer argumento a la función a la derecha, permitiendo un flujo de trabajo más limpio y legible. 

###  Función `merge`

En este proyecto, se utiliza la función `merge` para combinar los datos de importaciones (`data_importacion`) con los datos de catálogo de continentes (`catalogo_continente`). Este paso es importante para enriquecer el conjunto de datos de importaciones con información adicional sobre el continente de cada país.

```r
data_importacion = merge(data_importacion, catalogo_continente, by.x = "PAIS", by.y = "Codigo", all.x = TRUE)
```

## Reglas de Asociación
Se utiliza el algoritmo Apriori y FP-Growth para identificar patrones en las importaciones.
### Algoritmo Apriori
```r
reglas_importacion_apriori = apriori(data_importacion, parameter = list(support = 0.2, confidence = 0.5))
inspect(reglas_importacion_apriori)
```
### Algoritmo FP-Growth
```r
reglas = fim4r(data_importacion, method = "fpgrowth", target="rules", supp = .2, conf = .5)
reglasframe = as(reglas, "data.frame")
```
## Análisis de Clúster
El análisis de clúster se realiza utilizando el algoritmo K-means, visualizando los clústeres con ggplot2 y ggalt para identificar grupos en los datos.
```r
cluster <- kmeans(data_importacion, centers=3)
ggplot(data_importacion, aes(x = ADUANA, y = VALOR, color = as.factor(cluster$cluster))) +
  geom_point() +
  geom_point(data = as.data.frame(cluster$centers), aes(x = ADUANA, y = VALOR), color = "black", size=4, shape=17) +
  labs(title = "ADUANA vs VALOR") +
  theme_minimal()
```
## Ejecución del Código

1. Instalar las bibliotecas necesarias.
2. Ejecutar cada sección del código en un entorno de R con acceso a los archivos de datos mencionados.
3. Asegurarse de que los archivos Importaciones.csv y Catalogo Pais-Continente.csv estén en la ubicación especificada.
4. Replicar los análisis y visualizaciones ejecutando cada bloque de código en el orden presentado.
