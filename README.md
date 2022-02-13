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
Estado	|	Texto	|	([A-Za-z])\w+	|	Estado (Si aplica)	|	State	|
Ciudad	|	Texto	|	([A-Za-z])\w+	|	Ciudad	|	City	|
Años de experiencia profesional	|	Texto	|	([A-Za-z])\w+	|	Años de experiencia profesional	|	Overall years of professional experience	|
Años de experiencia laboral o en campo	|	Texto	|	([A-Za-z])\w+	|	Años de experiencia en campo (Ejecucion)	|	Years of experience in field	|
Maximo nivel de estudios	|	Texto	|	([A-Za-z])\w+	|	Maximo nivel de estudios completados	|	Highest level of education completed	|
Genero	|	Texto	|	([A-Za-z])\w+	|	Genero	|	Gender	|
Raza	|	Texto	|	([A-Za-z])\w+	|	Raza	|	Race	|
Salario Anual en COP	|	Numerico	|	(\d{1,})	|	Columna generada multiplicando la TRM del dia del procesamiento de los datos de acuerdo a la moneda que se encuentra en la columna Moneda con la columna Salario (Moneda Original)	|	Annual salary * TRM	|
Salario Anual + Otros pagos en COP	|	Numerico	|	(\d{1,})	|	Columna generada multiplicando la TRM del dia del procesamiento de los datos de acuerdo a la moneda que se encuentra en la columna Moneda de la columna Salario (Moneda Original) + Otros pagos (Moneda Original)	|	(Annual salary + Other monetary comp) * TRM	|
