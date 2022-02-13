# Documentacion del modelo
## Modelo original (https://docs.google.com/spreadsheets/d/1IPS5dBSGtwYVbjsfbaMCYIWnOuRmJcbequohNxCyGVw)
Columna	|	Tipo de dato	|	Formato	|	Descripcion	|
---|---|---|---|
Timestamp	|	Fecha y hora	|	(\d{1,2})/(\d{1,2})/(\d{4})\s(\d{1,2}):(\d{1,2}):(\d{1,2})\s(AM|PM)	|	Fecha y hora de la captura del dato	|
How old are you?	|	Texto	|	(\d{1,})-(\d{1,})	|	Grupo etareo de la persona	|
Industry	|	Texto	|	([A-Za-z])\w+	|	Industria a la que pertenece la persona	|
Job title	|	Texto	|	([A-Za-z])\w+	|	Cargo actual	|
Additional context on job title	|	Texto	|	([A-Za-z])\w+	|	Contexto adicional sobre el cargo	|
Annual salary	|	Numerico	|	(\d{1,})	|	Salario anual	|
Other monetary comp	|	Numerico	|	(\d{1,})	|	Compensaciones adicionales (Bonos, comisiones) anuales	|
Currency	|	Texto	|	[A-Z]{3}	|	Moneda en la que recibe el pago	|
Currency - other	|	Texto	|	([A-Za-z])\w+	|	Otras monedas	|
Additional context on income	|	Texto	|	([A-Za-z])\w+	|	Contexto adicional sobre el ingreso	|
Country	|	Texto	|	([A-Za-z])\w+	|	Pais	|
State	|	Texto	|	([A-Za-z])\w+	|	Estado (Si aplica)	|
City	|	Texto	|	([A-Za-z])\w+	|	Ciudad	|
Overall years of professional experience	|	Texto	|	([A-Za-z])\w+	|	Años de experiencia profesional	|
Years of experience in field	|	Texto	|	([A-Za-z])\w+	|	Años de experiencia en campo (Ejecucion)	|
Highest level of education completed	|	Texto	|	([A-Za-z])\w+	|	Maximo nivel de estudios completados	|
Gender	|	Texto	|	([A-Za-z])\w+	|	Genero	|
Race	|	Texto	|	([A-Za-z])\w+	|	Raza	|

------

## Modelo implementado
Columna	|	Tipo de dato	|	Formato	|	Descripcion	|	Columna Original	|
---|---|---|---|---|
Grupo Etareo	|	Texto	|	(\d{1,})-(\d{1,})	|	Grupo etareo de la persona	|	How old are you?	|
Industria	|	Texto	|	([A-Za-z])\w+	|	Industria a la que pertenece la persona	|	Industry	|
Cargo	|	Texto	|	([A-Za-z])\w+	|	Cargo actual	|	Job title	|
Salario (Moneda Original)	|	Numerico	|	(\d{1,})	|	Salario anual	|	Annual salary	|
Otros pagos (Moneda Original)	|	Numerico	|	(\d{1,})	|	Compensaciones adicionales (Bonos, comisiones) anuales	|	Other monetary comp	|
Moneda	|	Texto	|	[A-Z]{3}	|	Moneda en la que recibe el pago	|	Currency	|
Pais	|	Texto	|	([A-Za-z])\w+	|	Pais	|	Country	|
Localización	|	Texto	|	([A-Za-z])\w+	|	Localizacion de la persona, de acuerdo a la estructura (Ciudad, Estado, Pais)	|	City	|
Años de experiencia profesional	|	Texto	|	([A-Za-z])\w+	|	Años de experiencia profesional	|	Overall years of professional experience	|
Años de experiencia laboral o en campo	|	Texto	|	([A-Za-z])\w+	|	Años de experiencia en campo (Ejecucion)	|	Years of experience in field	|
Maximo nivel de estudios	|	Texto	|	([A-Za-z])\w+	|	Maximo nivel de estudios completados	|	Highest level of education completed	|
Genero	|	Texto	|	([A-Za-z])\w+	|	Genero	|	Gender	|
Raza	|	Texto	|	([A-Za-z])\w+	|	Raza	|	Race	|
Salario Anual en COP	|	Numerico	|	(\d{1,})	|	Columna generada multiplicando la TRM del dia del procesamiento de los datos de acuerdo a la moneda que se encuentra en la columna Moneda con la columna Salario (Moneda Original)	|	Annual salary * TRM	|
Salario Anual + Otros pagos en COP	|	Numerico	|	(\d{1,})	|	Columna generada multiplicando la TRM del dia del procesamiento de los datos de acuerdo a la moneda que se encuentra en la columna Moneda de la columna Salario (Moneda Original) + Otros pagos (Moneda Original)	|	(Annual salary + Other monetary comp) * TRM	|
TRM	|	Numerico	|	(\d{1,})	|	TRM del dia del cargue de los datos (Informacion que se debe extraer a mano de Google, o BANREP)	|		|

![image](https://user-images.githubusercontent.com/92442412/153766581-bfd19dbe-16e7-4fa9-87eb-b8eb0d830d4d.png)

Adicionalmente, se crearon las medidas:
- [x] Cantidad de respuestas: Corresponde al calculo del conteo de cualquiera de las columnas
- [x] Salario promedio: Corresponde al Salario Anual promedio en Pesos Colombianos, este campo se deja preparado para cruzar con cualquiera de los campos descriptivos disponibles.

---

## Proceso para el cargue y limpieza de los datos
Todo el proceso fue realizado en M (Power Query) con el objetivo de simplificar los procesos.

### Limpieza de ciudades y paises
- [x] Con el objetivo de simplificar el proceso de limpieza de este campo, se recurrio al uso del API de Google Maps https://developers.google.com/maps/documentation/places/web-service
- [x] Se generó una llave de API (No se deja en este texto por motivos de seguridad)
- [x] Se creó una funcion en PowerQuery que realice el llamado al endpoint `https://maps.googleapis.com/maps/api/place/findplacefromtext/json?input=<TEXTO QUE QUIEREN BUSCAR>&inputtype=textquery&fields=formatted_address,name&key=<LLAVE>` y que lleva como parametro la concatenacion de las columnas ==City, State, Country== separadas por coma
- [x] Se ejecutó esta función para 3000 registros, esto debido a que la organizacion no ha aprobado presupuesto para la compra del API y con este número no incurrimos en gastos.
- [x] Utilizando DAX Studio se exportó toda la información a archivos de texto plano para su posterior procesamiento
- [x] Se dejó una sola tabla que entrega todo de forma condensada.
- [x] Se eliminaron las columnas que debido al idioma consideramos que no aportan aún.

En caso de necesitar generar nuevas llaves, pueden realizarlo directamente en https://console.cloud.google.com/google/maps-apis/overview

# Reporte
Vea el reporte en este link
https://app.powerbi.com/view?r=eyJrIjoiYjczNTM4ZmYtNTdlYS00Mjk2LWE0YWYtY2M3MGJmYzZjMGZlIiwidCI6IjVmZmIyOTg2LTQ2MDctNDQwZS1iYjBmLWQyYjlmZmU2NjZlOSIsImMiOjR9&pageName=ReportSection
