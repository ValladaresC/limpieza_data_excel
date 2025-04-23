# limpieza_data_excel

![image](https://github.com/user-attachments/assets/be58b0c3-a7d3-46c2-8fdb-785d31712d97)

## Archivo 1.

- Convertir en Tabla.
- Eliminar espacios vacíos en celdas en toda la base de datos:
	Fue realizado con el uso de la función ESPACIOS (o TRIM)
- Separar EmpID en dos columnas: la primera debe tener el centro de costo (SQ, TN, etc) y la segunda el numero de empleado (0####):
	Fue realizado con la herramientas de datos 'Texto en columnas'-'De ancho fijo'
- Separar Name en First Name y Last Name:
	Fue realizado con la herramienta de datos 'Texto en columnas'-'Delimitados'-'Por espacio'
- Rellenar los datos faltantes en Gender:
	En este caso existen dos opciones:
	1. Si la base de datos tiene pocos datos faltantes en esta columna, se puede observar la columna first_name y deducir el Gender.
	2. Si hay muchos datos faltantes en esta columna, puede usarse la opción de eliminar celdas vacías.
- Reemplazar los NULL de Department por 'Others':
	Fue realizado con la herramienta 'Buscar y Reemplazar'
- Formatear Salary a formato moneda
	Fue realizado con la herramienta 'Numero', primero estableciendo para toda la columna el formato 'Numero' y luego llevándolo a formato 'Moneda' con símbolo de $.
- Formatear Start Date con un único formato de fecha:
	Si las fechas tienen muchos formatos diferentes, lo más efectivo puede ser usar Power Query, ya que te permite crear reglas específicas para cada formato y convertirlos todos en fechas uniformes.
- Agregar una columna a partir de FTE con el nombre FTE Detail: para FTE < 1 reportar 'Part Time', sino 'Full Time':
	Se agregó una nueva columna y se condiciono bajo una formula con la función SI.
- Separa Work location en Work_location_city y Work_location_country, aquellos celdas con valor 'Remote' colocarlo en ambas columnas. Eliminar la columna original Work Location.
	Para separar la ciudad del país fue usada la herramienta de datos 'Texto en columnas'-'Delimitados'-'Por coma'. Posterior a la separación la columna Work_location_country fue filtrada por celdas vacías para completarlas de acuerdo al requerimiento y se utilizó la herramienta 'Buscar y Reemplazar'.	
- Eliminar filas con celdas vacías en toda la base de datos:
	Se seleccionó toda la base de datos y se usó la herramienta 'Ir a Especial' y se selecciona la opción de 'Celdas en Blanco', luego Eliminar Celdas y se eliminan las filas con aquellas celdas donde no había datos. En este caso la única columna con 11 celdas vacías fue Salary, quedando 266 filas.
- Eliminar duplicados tomando en consideración Emp_ID
	Fue usada la herramienta de datos 'Eliminar duplicados' seleccionando solo la columna Emp_ID, eliminándose 31 datos duplicados, quedando 234 filas.

![limpieza_archivo1](https://github.com/user-attachments/assets/c3b7443c-5563-47c3-89a5-dad90c1aaf4e)

> [!WARNING]
> La desventaja de hacer este tipo de proceso de limpieza de datos es que todo se realiza de manera Manual, sin embargo usando Power Query quedaría automatizado en caso de insertar más datos en el futuro.

> [!IMPORTANT]
> Este tipo de limpieza de datos Manual se recomienda en aquellas base de datos que no cambiarán en el tiempo o que la ingesta de nueva data es insignificante o nula, también si tienes conjuntos de datos muy pequeños o simples, donde los cambios son mínimos y fáciles de manejar. También puede ser útil cuando necesitas realizar ajustes muy específicos o personalizados que no se adaptan bien a las funciones automáticas. Sin embargo, para bases de datos más grandes o complejas, generalmente es más eficiente y preciso usar herramientas automatizadas, ya que reducen errores y ahorran tiempo.


## Archivo 2.

- Convertir en Tabla.
- Revisar Ortografía (países)
	Se usa el filtro para observar de manera resumida los países que están mal escritos. Luego se usa la herramienta 'Buscar y Reemplazar'. También puede usar la herramienta 'Ortografía' para ir reemplazando las palabras con errores.
- Limpiar, espacios (nombres y apellidos y departamento)
	Se usaron las funciones de LIMPIAR la cual quita todos los caracteres no imprimibles del texto y ESPACIOS la cual quita todos los espacios del texto excepto los espacios individuales entre palabras.
- Ajustar código (número+inicial de los apellidos)
	Fue realizada la siguiente formula: =[@COD]&IZQUIERDA([@[Primer apellido]];1)&IZQUIERDA([@[Segundo apellido]];1)
- Valores Duplicados
	Se uso la herramienta de formato condicional-resaltar celdas-valores duplicados y luego con la herramienta de datos-eliminar duplicados fueron eliminados 17 valores duplicados, quedando 233 filas.
- Mayúsculas (departamento)
	Se utilizó la función MAYUSC para hacer la conversión a letras mayúsculas.
- Nombres propios - Nombre completo
	Se utilizó la función NOMPROPIO para hacer la conversión.
- Separa Nombres en Nombre 1 y Nombre 2
	Se utilizó la herramienta de datos 'Texto en columnas'-'Delimitados'-'Por espacio'
- Concatenar (email) el cual debe estar compuesto nombre1.cod@cleandata.com
	Se formuló usando la función CONCAT de la siguiente manera: CONCAT([@[Nombre 1]];".";[@COD];"@cleandata.com")
- Formatos cifra ($ = sustituir y el punto decimal por la coma decimal).
	Se usó la herramienta 'Buscar y reemplazar'
- Formatos fechas
	Se usó la herramienta de datos 'Texto en columnas'-'Delimitados'-'QUITAR SELECCCIONES'-'Formato de Columna'-'Fecha MDA'.
- Renombrar boléanos. Para la columna Tiempo (True = Completo; False = Parcial) y para la columna Tipo de contrato  (True=Permanente ; false=Temporal)
	Se utilizaron las siguientes formula condicionales: SI([@Tiempo]="false";"Completo";"Parcial") y SI([@[Tipo de contrato]]="false";"Temporal";"Permanente"). También existe la opción de utilizar la herramienta 'Buscar y reemplazar'.
- Celdas vacías con null (filtros)
	Se puede identificar las celdas vacías haciendo un formato condicional y rellenando la celdas vacías con un color específico que las resalte. Luego puede filtrar por color del relleno y eliminarlas. 
	También puede utilizar el procedimiento explicado en el Archivo1.

![limpieza_archivo2](https://github.com/user-attachments/assets/58bd3739-de88-450e-81fe-984e750bfb31)

> [!NOTE]
> Recuerde que antes de eliminar filas con datos nulos o vacíos se debe tener en cuenta preguntar al cliente si es posible obtener esos datos faltantes o estar seguro de que se pueden eliminar

## Archivo 3.

- Unir birthYear, birthMonth y birthDay en una sola fecha y darle formato
	Puede usarse la función CONCAT pero en este caso fue usada la función UNIRCADENAS la cual concatena una lista o celdas mediante un carácter delimitador, con la siguiente formula: =UNIRCADENAS("/";VERDADERO;B2;C2;D2), sin embargo arroja una columna con datos de tipo texto o general, por lo tanto fue convertida con la función FECHANUMERO la cual convierte una fecha en forma de texto en un numero que representa la fecha en código de fecha y posteriormente fue formateada con tipo Fecha Corta.
- Obtener la edad
	Se usó la función SIFECHA la cual permite restar fechas y mostrar el valor en años, meses o días, en combinación con la función HOY() de la siguiente manera SIFECHA([@birthdate];HOY();"Y") 
- Formatear el "." decimal de Salary para ser reconocido como numero
	Fue utilizado la función SUSTITUIR la cual reemplaza el texto existente con el texto nuevo en una cadena de la siguiente manera: USTITUIR([@salary];".";",") y por ultimo se aplicó la función VALOR para formatear el texto a valor.
- Extraer de file_name solo el nombre del archivo, sin su extensión
	Fue usada la herramienta 'Buscar y reemplazar' de la siguiente manera: se colocó la siguiente nomenclatura *\ la cual busca todos los caracteres que estén antes del "\" y los reemplaza por nada. Por último para eliminar la extensión del archivo es usada la nomenclatura .* la cual reemplazará por nada todo lo que este después del punto incluyéndolo.

![limpieza_archivo3](https://github.com/user-attachments/assets/48400661-3e56-4827-a35c-875e4fc9d6c4)
