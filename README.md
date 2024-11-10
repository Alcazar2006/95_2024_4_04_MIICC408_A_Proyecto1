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
