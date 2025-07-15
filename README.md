# PineScript
Documentacion del manual de pine script v5 de TradingView simplificada con REGEX en formatos accesibles para una IA.

FUNCIONES:

alert()

Crea un evento de alerta cuando se llama durante la barra en tiempo real, que desencadenará una alerta de scripts en función de los "eventos de la función de alerta" cuando se haya creado una con anterioridad para el indicador o la estrategia a través del cuadro de diálogo "Crear alerta".
Sintaxis
alert(message, freq) → void
Argumentos
message (series string) Mensaje enviado cuando se activa la alerta. Argumento obligatorio.
freq (input string) Frecuencia de activación. Los valores posibles son: alert.freq_all (todas las llamadas a la función activan la alerta), alert.freq_once_per_bar (la primera llamada a la función durante la barra activa la alerta), alert.freq_once_per_bar_close (la llamada a la función activa la alerta solo cuando se produce durante la última iteración de script de la barra en tiempo real, cuando se cierra). El valor por defecto es alert.freq_once_per_bar.
Ejemplo

//@version=5
indicator("`alert()` example", "", true)
ma = ta.sma(close, 14)
xUp = ta.crossover(close, ma)
if xUp
    // Trigger the alert the first time a cross occurs during the real-time bar.
    alert("Price (" + str.tostring(close) + ") crossed over MA (" + str.tostring(ma) +  ").", alert.freq_once_per_bar)
plot(ma)
plotchar(xUp, "xUp", "▲", location.top, size = size.tiny)
Observaciones
El Centro de ayuda explica cómo crear dichas alertas.
A diferencia de alertcondition, las llamadas alert NO cuentan como trazado adicional.
Las llamadas a funciones se pueden establecer tanto en ámbitos globales como locales.
Las llamadas a funciones no muestran nada en el gráfico.
El argumento 'freq' solo afecta a la frecuencia de activación de la llamada de la función en la que se utiliza.
Ver también
alertcondition
alertcondition()

Crea una condición de alerta, disponible en el cuadro de diálogo Crear alerta. Tenga en cuenta que alertcondition NO crea una alerta, solo le ofrece más opciones en el cuadro de diálogo Crear alerta. Además, el efecto de alertcondition no se visualiza en el gráfico.
Sintaxis
alertcondition(condition, title, message) → void
Argumentos
condition (series bool) Serie de valores booleanos utilizada para las alertas. El valor true significa la activación de alertas, mientras que false indica desactivación de alertas. Argumento obligatorio.
title (const string) Título de la condición de alerta. Argumento opcional.
message (const string) Mensaje a mostrar cuando se activa una alerta. Argumerto opcional.
Ejemplo

//@version=5
indicator("alertcondition", overlay=true)
alertcondition(close >= open, title='Alert on Green Bar', message='Green Bar!')
Observaciones
Tenga en cuenta que la llamada alertcondition genera un gráfico adicional. Se tienen en cuenta todas estas llamadas para calcular el número de series de salida por script.
Ver también
alert
array.abs()2 sobrecargas

Devuelve un array que contiene el valor absoluto de cada elemento del array original.
Sintaxis y sobrecargas
array.abs(id) → array<float>
array.abs(id) → array<int>
Argumentos
id (array<int/float>) Objeto array.
Ver también
array.new_float
array.insert
array.slice
array.reverse
order.ascending
order.descending
array.avg()2 sobrecargas

Función que devuelve el promedio de los elementos del array.
Sintaxis y sobrecargas
array.avg(id) → series float
array.avg(id) → series int
Argumentos
id (array<int/float>) Objeto array.
Ejemplo

//@version=5
indicator("array.avg example")
a = array.new_float(0)
for i = 0 to 9
    array.push(a, close[i])
plot(array.avg(a))
Devoluciones
Promedio de los elementos del array.
Ver también
array.new_float
array.max
array.min
array.stdev
array.binary_search()

Función que devuelve el índice del valor, o -1 en caso de no encontrar el valor. El array a buscar debe estar clasificado en orden ascendente.
Sintaxis
array.binary_search(id, val) → series int
Argumentos
id (array<int/float>) Objeto array.
val (series int/float) Valor a buscar en el array.
Ejemplo

//@version=5
indicator("array.binary_search")
a = array.from(5, -2, 0, 9, 1)
array.sort(a) // [-2, 0, 1, 5, 9]
position = array.binary_search(a, 0) // 1
plot(position)
Observaciones
Una búsqueda binaria funciona en matrices previamente ordenadas de forma ascendente. Comienza comparando un elemento en el centro del array con el valor objetivo. Si el elemento coincide con este valor, se devuelve su posición en el array. Si el valor del elemento es mayor que el del objetivo, la búsqueda continúa en la mitad inferior del array. Si el valor del elemento es menor que el del objetivo, la búsqueda continúa en la mitad superior del array. Al hacer esto de forma recurrente, el algoritmo va eliminando progresivamente porciones cada vez más pequeñas del array en las que no puede estar el valor objetivo.
Ver también
array.new_float
array.insert
array.slice
array.reverse
order.ascending
order.descending
array.binary_search_leftmost()

Función que devuelve el índice del valor cuando lo encuentra. En caso contrario, la función devuelve el índice del siguiente elemento más pequeño situado al lado izquierdo de donde se encontraría el valor si estuviera en el array. El array a buscar debe estar ordenado de forma ascendente.
Sintaxis
array.binary_search_leftmost(id, val) → series int
Argumentos
id (array<int/float>) Objeto array.
val (series int/float) Valor a buscar en el array.
Ejemplo

//@version=5
indicator("array.binary_search_leftmost")
a = array.from(5, -2, 0, 9, 1)
array.sort(a) // [-2, 0, 1, 5, 9]
position = array.binary_search_leftmost(a, 3) // 2
plot(position)
Ejemplo

//@version=5
indicator("array.binary_search_leftmost, repetitive elements")
a = array.from(4, 5, 5, 5)
// Returns the index of the first instance.
position = array.binary_search_leftmost(a, 5) 
plot(position) // Plots 1
Observaciones
Una búsqueda binaria funciona en matrices previamente ordenadas de forma ascendente. Comienza comparando un elemento en el centro del array con el valor objetivo. Si el elemento coincide con este valor, se devuelve su posición en el array. Si el valor del elemento es mayor que el del objetivo, la búsqueda continúa en la mitad inferior del array. Si el valor del elemento es menor que el del objetivo, la búsqueda continúa en la mitad superior del array. Al hacer esto de forma recurrente, el algoritmo va eliminando progresivamente porciones cada vez más pequeñas del array en las que no puede estar el valor objetivo.
Ver también
array.new_float
array.insert
array.slice
array.reverse
order.ascending
order.descending
array.binary_search_rightmost()

Función que devuelve el índice del valor cuando lo encuentra. En caso contrario, la función devuelve el índice del siguiente elemento más pequeño situado al lado derecha de donde se encontraría el valor si estuviera en el array. El array debe estar ordenado de forma ascendente.
Sintaxis
array.binary_search_rightmost(id, val) → series int
Argumentos
id (array<int/float>) Objeto array.
val (series int/float) Valor a buscar en el array.
Ejemplo

//@version=5
indicator("array.binary_search_rightmost")
a = array.from(5, -2, 0, 9, 1)
array.sort(a) // [-2, 0, 1, 5, 9]
position = array.binary_search_rightmost(a, 3) // 3
plot(position)
Ejemplo

//@version=5
indicator("array.binary_search_rightmost, repetitive elements")
a = array.from(4, 5, 5, 5)
// Returns the index of the last instance.
position = array.binary_search_rightmost(a, 5) 
plot(position) // Plots 3
Observaciones
Una búsqueda binaria funciona en arrays ordenados de forma ascendente. Comienza comparando un elemento en el centro del array con el valor objetivo. Si el elemento coincide con este valor, se devuelve su posición en el array. Si el valor del elemento es mayor que el del objetivo, la búsqueda continúa en la mitad inferior del array. Si el valor del elemento es menor que el del objetivo, la búsqueda continúa en la mitad superior del array. Al hacer esto de forma recurrente, el algoritmo va eliminando progresivamente porciones cada vez más pequeñas del array en las que no puede estar el valor objetivo.
Ver también
array.new_float
array.insert
array.slice
array.reverse
order.ascending
order.descending
array.clear()

Función que elimina todos los elementos de un array.
Sintaxis
array.clear(id) → void
Argumentos
id (any array type) Objeto array.
Ejemplo

//@version=5
indicator("array.clear example")
a = array.new_float(5,high)
array.clear(a)
array.push(a, close)
plot(array.get(a,0))
plot(array.size(a))
Ver también
array.new_float
array.insert
array.push
array.remove
array.pop
array.concat()

Función que se utiliza para agrupar dos arrays. Empuja todos los elementos del segundo array al primero y devuelve este último.
Sintaxis
array.concat(id1, id2) → array<type>
Argumentos
id1 (any array type) Primer objeto array.
id2 (any array type) Segundo objeto array.
Ejemplo

//@version=5
indicator("array.concat example")
a = array.new_float(0,0)
b = array.new_float(0,0)
for i = 0 to 4
    array.push(a, high[i])
    array.push(b, low[i])
c = array.concat(a,b)
plot(array.size(a))
plot(array.size(b))
plot(array.size(c))
Devoluciones
Primer array con los elementos agrupados del segundo array.
Ver también
array.new_float
array.insert
array.slice
array.copy()

Función que crea una copia de un array existente.
Sintaxis
array.copy(id) → array<type>
Argumentos
id (any array type) Objeto array.
Ejemplo

//@version=5
indicator("array.copy example")
length = 5
a = array.new_float(length, close)
b = array.copy(a)
a := array.new_float(length, open)
plot(array.sum(a) / length)
plot(array.sum(b) / length)
Devoluciones
Copia de un array.
Ver también
array.new_float
array.get
array.slice
array.sort
array.covariance()

Función que devuelve la covarianza de dos arrays.
Sintaxis
array.covariance(id1, id2, biased) → series float
Argumentos
id1 (array<int/float>) Objeto array.
id2 (array<int/float>) Objeto array.
biased (series bool) Determina el tipo de estimación a usar. Opcional. El valor por defecto es true.
Ejemplo

//@version=5
indicator("array.covariance example")
a = array.new_float(0)
b = array.new_float(0)
for i = 0 to 9
    array.push(a, close[i])
    array.push(b, open[i])
plot(array.covariance(a, b))
Devoluciones
Covarianza de dos arrays.
Observaciones
Si biased es true, la función realizará el cálculo utilizando una estimación sesgada de toda la población y, si es false, la muestra se realizará sin sesgos.
Ver también
array.new_float
array.max
array.stdev
array.avg
array.variance
array.every()

Devuelve true si todos los elementos del array id son true y, de lo contrario, false.
Sintaxis
array.every(id) → series bool
Argumentos
id (array<bool>) Objeto array.
Observaciones
Esta función también sirve con arrays de tipo int y float, en cuyo caso los valores cero se consideran false, y el resto true.
Ver también
array.some
array.get
array.fill()

Función que establece los elementos de un array con un solo valor. Si no se especifica ningún índice, se configuran todos los elementos. Si solo se proporciona un índice inicial (0 por defecto), se establecen los elementos que se inician en ese índice. En el caso de utilizar ambos parámetros del índice, entonces se establecen los elementos del índice inicial hasta el final, pero sin incluir este último (na por defecto).
Sintaxis
array.fill(id, value, index_from, index_to) → void
Argumentos
id (any array type) Objeto array.
value (series <type of the array's elements>) Valor con el que se rellena el array.
index_from (series int) Índice de inicio, el valor por defecto es 0.
index_to (series int) El valor por defecto del índice final es na. Debe ser superior al índice del último elemento a establecer.
Ejemplo

//@version=5
indicator("array.fill example")
a = array.new_float(10)
array.fill(a, close)
plot(array.sum(a))
Ver también
array.new_float
array.set
array.slice
array.first()

Devuelve el primer elemento del array. Lanza un error de ejecución si está vacío.
Sintaxis
array.first(id) → series <type>
Argumentos
id (any array type) Objeto array.
Ejemplo

//@version=5
indicator("array.first example")
arr = array.new_int(3, 10)
plot(array.first(arr))
Ver también
array.last
array.get
array.from()12 sobrecargas

La función utiliza un número variable de argumentos con uno de los siguientes tipos: int, float, bool, string, label, line, color, box, table, linefill, y devuelve un array del tipo correspondiente.
Sintaxis y sobrecargas
array.from(arg0, arg1, ...) → array<type>
array.from(arg0, arg1, ...) → array<series enum>
array.from(arg0, arg1, ...) → array<label>
array.from(arg0, arg1, ...) → array<line>
array.from(arg0, arg1, ...) → array<box>
array.from(arg0, arg1, ...) → array<table>
array.from(arg0, arg1, ...) → array<linefill>
array.from(arg0, arg1, ...) → array<string>
array.from(arg0, arg1, ...) → array<color>
array.from(arg0, arg1, ...) → array<int>
array.from(arg0, arg1, ...) → array<float>
array.from(arg0, arg1, ...) → array<bool>
Argumentos
arg0, arg1, ... (<arg..._type>) Argumentos array.
Ejemplo

//@version=5
indicator("array.from_example", overlay = false)
arr = array.from("Hello", "World!") // arr (array<string>) will contain 2 elements: {Hello}, {World!}.
plot(close)
Devoluciones
Valor del elemento del array.
Observaciones
Esta función puede aceptar hasta 4.000 argumentos de tipo 'int', 'float', 'bool' o 'color'. Para todos los demás tipos, incluidos los definidos por el usuario, el límite es de 999.
array.get()

Función que devuelve el valor del elemento en el índice especificado.
Sintaxis
array.get(id, index) → series <type>
Argumentos
id (any array type) Objeto array.
index (series int) Índice del elemento cuyo valor se va a devolver.
Ejemplo

//@version=5
indicator("array.get example")
a = array.new_float(0)
for i = 0 to 9
    array.push(a, close[i] - open[i])
plot(array.get(a, 9))
Devoluciones
Valor del elemento del array.
Ver también
array.new_float
array.set
array.slice
array.sort
array.includes()

Función que devuelve true si encuentra el valor en un array; de lo contrario, false.
Sintaxis
array.includes(id, value) → series bool
Argumentos
id (any array type) Objeto array.
value (series <type of the array's elements>) Valor a buscar en el array.
Ejemplo

//@version=5
indicator("array.includes example")
a = array.new_float(5,high)
p = close
if array.includes(a, high)
    p := open
plot(p)
Devoluciones
True si encuentra el valor en el array; de lo contrario, false.
Ver también
array.new_float
array.indexof
array.shift
array.remove
array.insert
array.indexof()

Función que devuelve el índice de la primera aparición del valor o -1 si no se encuentra el valor.
Sintaxis
array.indexof(id, value) → series int
Argumentos
id (any array type) Objeto array.
value (series <type of the array's elements>) Valor a buscar en el array.
Ejemplo

//@version=5
indicator("array.indexof example")
a = array.new_float(5,high)
index = array.indexof(a, high)
plot(index)
Devoluciones
Índice de un elemento.
Ver también
array.lastindexof
array.get
array.lastindexof
array.remove
array.insert
array.insert()

La función modifica el contenido de un array añadiendo nuevos elementos en su lugar.
Sintaxis
array.insert(id, index, value) → void
Argumentos
id (any array type) Objeto array.
index (series int) Índice en el que se inserta el valor.
value (series <type of the array's elements>) Valor que se añade al array.
Ejemplo

//@version=5
indicator("array.insert example")
a = array.new_float(5, close)
array.insert(a, 0, open)
plot(array.get(a, 5))
Ver también
array.new_float
array.set
array.push
array.remove
array.pop
array.unshift
array.join()

Función que crea y devuelve una nueva cadena concatenando todos los elementos de un array, separados por la cadena de separación especificada.
Sintaxis
array.join(id, separator) → series string
Argumentos
id (array<int/float/string>) Objeto array.
separator (series string) Cadena que se utiliza para separar cada elemento del array.
Ejemplo

//@version=5
indicator("array.join example")
a = array.new_float(5, 5)
label.new(bar_index, close, array.join(a, ","))
Ver también
array.new_float
array.set
array.insert
array.remove
array.pop
array.unshift
array.last()

Devuelve el último elemento del array. Lanza un error de ejecución si está vacío.
Sintaxis
array.last(id) → series <type>
Argumentos
id (any array type) Objeto array.
Ejemplo

//@version=5
indicator("array.last example")
arr = array.new_int(3, 10)
plot(array.last(arr))
Ver también
array.first
array.get
array.lastindexof()

Función que devuelve el índice de la última aparición del valor o -1 si no se encuentra el valor.
Sintaxis
array.lastindexof(id, value) → series int
Argumentos
id (any array type) Objeto array.
value (series <type of the array's elements>) Valor a buscar en el array.
Ejemplo

//@version=5
indicator("array.lastindexof example")
a = array.new_float(5,high)
index = array.lastindexof(a, high)
plot(index)
Devoluciones
Índice de un elemento.
Ver también
array.new_float
array.set
array.push
array.remove
array.insert
array.max()4 sobrecargas

Función que devuelve el mayor valor o el enésimo mayor valor de un array dado.
Sintaxis y sobrecargas
array.max(id) → series float
array.max(id) → series int
array.max(id, nth) → series float
array.max(id, nth) → series int
Argumentos
id (array<int/float>) Objeto array.
Ejemplo

//@version=5
indicator("array.max")
a = array.from(5, -2, 0, 9, 1)
thirdHighest = array.max(a, 2) // 1
plot(thirdHighest)
Devoluciones
El mayor o el enésimo mayor valor del array.
Ver también
array.new_float
array.min
array.sum
array.median()2 sobrecargas

Función que devuelve la mediana de los elementos del array.
Sintaxis y sobrecargas
array.median(id) → series float
array.median(id) → series int
Argumentos
id (array<int/float>) Objeto array.
Ejemplo

//@version=5
indicator("array.median example")
a = array.new_float(0)
for i = 0 to 9
    array.push(a, close[i])
plot(array.median(a))
Devoluciones
Mediana de los elementos del array.
Ver también
array.median
array.avg
array.variance
array.min
array.min()4 sobrecargas

Función que devuelve el valor más pequeño o el enésimo valor más pequeño de un array dado.
Sintaxis y sobrecargas
array.min(id) → series float
array.min(id) → series int
array.min(id, nth) → series float
array.min(id, nth) → series int
Argumentos
id (array<int/float>) Objeto array.
Ejemplo

//@version=5
indicator("array.min")
a = array.from(5, -2, 0, 9, 1)
secondLowest = array.min(a, 1) // 0
plot(secondLowest)
Devoluciones
El menor o el enésimo valor más pequeño del array.
Ver también
array.new_float
array.max
array.sum
array.mode()2 sobrecargas

Función que devuelve el modo de los elementos de un array. Si hay varios valores con la misma frecuencia, devuelve el valor más pequeño.
Sintaxis y sobrecargas
array.mode(id) → series float
array.mode(id) → series int
Argumentos
id (array<int/float>) Objeto array.
Ejemplo

//@version=5
indicator("array.mode example")
a = array.new_float(0)
for i = 0 to 9
    array.push(a, close[i])
plot(array.mode(a))
Devoluciones
Valor más frecuente del array id. Si no existe ninguno, devuelve en su lugar el valor más pequeño.
Ver también
array.new_float
ta.mode
matrix.mode
array.avg
array.variance
array.min
array.new_bool()

Función que crea un nuevo objeto array con elementos de tipo bool.
Sintaxis
array.new_bool(size, initial_value) → array<bool>
Argumentos
size (series int) Tamaño inicial de un array. Opcional. El valor por defecto es 0.
initial_value (series bool) Valor inicial de todos los elementos del array. Opcional. El valor por defecto es 'na'.
Ejemplo

//@version=5
indicator("array.new_bool example")
length = 5
a = array.new_bool(length, close > open)
plot(array.get(a, 0) ? close : open)
Devoluciones
ID de un objeto array que se puede usar en otras funciones array.*().
Observaciones
Un índice de un array comienza a partir de 0.
Ver también
array.new_float
array.get
array.slice
array.sort
array.new_box()

Función que crea un nuevo objeto array con elementos de tipo box.
Sintaxis
array.new_box(size, initial_value) → array<box>
Argumentos
size (series int) Tamaño inicial de un array. Opcional. El valor por defecto es 0.
initial_value (series box) Valor inicial de todos los elementos del array. Opcional. El valor por defecto es 'na'.
Ejemplo

//@version=5
indicator("array.new_box example")
boxes = array.new_box()
array.push(boxes, box.new(time, close, time+2, low, xloc=xloc.bar_time))
plot(1)
Devoluciones
ID de un objeto array que se puede usar en otras funciones array.*().
Observaciones
Un índice de un array comienza a partir de 0.
Ver también
array.new_float
array.get
array.slice
array.new_color()

Función que crea un nuevo objeto array con elementos de tipo color.
Sintaxis
array.new_color(size, initial_value) → array<color>
Argumentos
size (series int) Tamaño inicial de un array. Opcional. El valor por defecto es 0.
initial_value (series color) Valor inicial de todos los elementos del array. Opcional. El valor por defecto es 'na'.
Ejemplo

//@version=5
indicator("array.new_color example")
length = 5
a = array.new_color(length, color.red)
plot(close, color = array.get(a, 0))
Devoluciones
ID de un objeto array que se puede usar en otras funciones array.*().
Observaciones
Un índice de un array comienza a partir de 0.
Ver también
array.new_float
array.get
array.slice
array.sort
array.new_float()

Función que crea un nuevo objeto array con elementos de tipo float.
Sintaxis
array.new_float(size, initial_value) → array<float>
Argumentos
size (series int) Tamaño inicial de un array. Opcional. El valor por defecto es 0.
initial_value (series int/float) Valor inicial de todos los elementos del array. Opcional. El valor por defecto es 'na'.
Ejemplo

//@version=5
indicator("array.new_float example")
length = 5
a = array.new_float(length, close)
plot(array.sum(a) / length)
Devoluciones
ID de un objeto array que se puede usar en otras funciones array.*().
Observaciones
Un índice de un array comienza a partir de 0.
Ver también
array.new_color
array.new_bool
array.get
array.slice
array.sort
array.new_int()

Función que crea un nuevo objeto array con elementos de tipo int.
Sintaxis
array.new_int(size, initial_value) → array<int>
Argumentos
size (series int) Tamaño inicial de un array. Opcional. El valor por defecto es 0.
initial_value (series int) Valor inicial de todos los elementos del array. Opcional. El valor por defecto es 'na'.
Ejemplo

//@version=5
indicator("array.new_int example")
length = 5
a = array.new_int(length, int(close))
plot(array.sum(a) / length)
Devoluciones
ID de un objeto array que se puede usar en otras funciones array.*().
Observaciones
Un índice de un array comienza a partir de 0.
Ver también
array.new_float
array.get
array.slice
array.sort
array.new_label()

Función que crea un nuevo objeto array con elementos de tipo label.
Sintaxis
array.new_label(size, initial_value) → array<label>
Argumentos
size (series int) Tamaño inicial de un array. Opcional. El valor por defecto es 0.
initial_value (series label) Valor inicial de todos los elementos del array. Opcional. El valor por defecto es 'na'.
Ejemplo

//@version=5
indicator("array.new_label example", overlay = true, max_labels_count = 500)

//@variable The number of labels to show on the chart.
int labelCount = input.int(50, "Labels to show", 1, 500)

//@variable An array of `label` objects.
var array<label> labelArray = array.new_label()

//@variable A `chart.point` for the new label.
labelPoint = chart.point.from_index(bar_index, close)
//@variable The text in the new label.
string labelText = na
//@variable The color of the new label.
color labelColor = na
//@variable The style of the new label.
string labelStyle = na

// Set the label attributes for rising bars.
if close > open
    labelText  := "Rising"
    labelColor := color.green
    labelStyle := label.style_label_down
// Set the label attributes for falling bars.
else if close < open
    labelText  := "Falling"
    labelColor := color.red
    labelStyle := label.style_label_up

// Add a new label to the `labelArray` when the chart bar closed at a new value.
if close != open
    labelArray.push(label.new(labelPoint, labelText, color = labelColor, style = labelStyle))
// Remove the first element and delete its label when the size of the `labelArray` exceeds the `labelCount`.
if labelArray.size() > labelCount
    label.delete(labelArray.shift())
Devoluciones
ID de un objeto array que se puede usar en otras funciones array.*().
Observaciones
Un índice de un array comienza a partir de 0.
Ver también
array.new_float
array.get
array.slice
array.new_line()

Función que crea un nuevo objeto array con elementos de tipo line.
Sintaxis
array.new_line(size, initial_value) → array<line>
Argumentos
size (series int) Tamaño inicial de un array. Opcional. El valor por defecto es 0.
initial_value (series line) Valor inicial de todos los elementos del array. Opcional. El valor por defecto es 'na'.
Ejemplo

//@version=5
indicator("array.new_line example")
// draw last 15 lines
var a = array.new_line()
array.push(a, line.new(bar_index - 1, close[1], bar_index, close))
if array.size(a) > 15
    ln = array.shift(a)
    line.delete(ln)
Devoluciones
ID de un objeto array que se puede usar en otras funciones array.*().
Observaciones
Un índice de un array comienza a partir de 0.
Ver también
array.new_float
array.get
array.slice
array.new_linefill()

Función que crea un nuevo objeto array con elementos de tipo linefill.
Sintaxis
array.new_linefill(size, initial_value) → array<linefill>
Argumentos
size (series int) Tamaño inicial de un array.
initial_value (series linefill) Valor inicial de todos los elementos de un array.
Devoluciones
ID de un objeto array que se puede usar en otras funciones array.*().
Observaciones
Un índice de un array comienza a partir de 0.
array.new_string()

Función que crea un nuevo objeto array con elementos de tipo string.
Sintaxis
array.new_string(size, initial_value) → array<string>
Argumentos
size (series int) Tamaño inicial de un array. Opcional. El valor por defecto es 0.
initial_value (series string) Valor inicial de todos los elementos del array. Opcional. El valor por defecto es 'na'.
Ejemplo

//@version=5
indicator("array.new_string example")
length = 5
a = array.new_string(length, "text")
label.new(bar_index, close, array.get(a, 0))
Devoluciones
ID de un objeto array que se puede usar en otras funciones array.*().
Observaciones
Un índice de un array comienza a partir de 0.
Ver también
array.new_float
array.get
array.slice
array.new_table()

Función que crea un nuevo objeto array con elementos de tipo table.
Sintaxis
array.new_table(size, initial_value) → array<table>
Argumentos
size (series int) Tamaño inicial de un array. Opcional. El valor por defecto es 0.
initial_value (series table) Valor inicial de todos los elementos del array. Opcional. El valor por defecto es 'na'.
Ejemplo

//@version=5
indicator("table array")
tables = array.new_table()
array.push(tables, table.new(position = position.top_left, rows = 1, columns = 2, bgcolor = color.yellow, border_width=1))
plot(1)
Devoluciones
ID de un objeto array que se puede usar en otras funciones array.*().
Observaciones
Un índice de un array comienza a partir de 0.
Ver también
array.new_float
array.get
array.slice
array.new<type>()

Función que crea un nuevo objeto array con elementos de <type>.
Sintaxis
array.new<type>(size, initial_value) → array<type>
Argumentos
size (series int) Tamaño inicial de un array. Opcional. El valor por defecto es 0.
initial_value (<array_type>) Valor inicial de todos los elementos del array. Opcional. El valor por defecto es 'na'.
Ejemplo

//@version=5
indicator("array.new<string> example")
a = array.new<string>(1, "Hello, World!")
label.new(bar_index, close, array.get(a, 0))
Ejemplo

//@version=5
indicator("array.new<color> example")
a = array.new<color>()
array.push(a, color.red)
array.push(a, color.green)
plot(close, color = array.get(a, close > open ? 1 : 0))
Ejemplo

//@version=5
indicator("array.new<float> example")
length = 5
var a = array.new<float>(length, close)
if array.size(a) == length
    array.remove(a, 0)
    array.push(a, close)
plot(array.sum(a) / length, "SMA")
Ejemplo

//@version=5
indicator("array.new<line> example")
// draw last 15 lines
var a = array.new<line>()
array.push(a, line.new(bar_index - 1, close[1], bar_index, close))
if array.size(a) > 15
    ln = array.shift(a)
    line.delete(ln)
Devoluciones
ID de un objeto array que se puede usar en otras funciones array.*().
Observaciones
Un índice de un array comienza a partir de 0.
Si desea inicializar un array y especificar todos sus elementos al mismo tiempo, utilice la función array.from.
Ver también
array.from
array.push
array.get
array.size
array.remove
array.shift
array.sum
array.percentile_linear_interpolation()2 sobrecargas

Devuelve el valor para el cual el porcentaje especificado de valores array (percentil) sea menor o igual al mismo, utilizando la interpolación lineal.
Sintaxis y sobrecargas
array.percentile_linear_interpolation(id, percentage) → series float
array.percentile_linear_interpolation(id, percentage) → series int
Argumentos
id (array<int/float>) Objeto array.
percentage (series int/float) Porcentaje de valores que debe ser igual o menor que el valor devuelto.
Observaciones
En estadística, el percentil es el porcentaje de elementos de ranking que aparecen en una puntuación determinada o por debajo de ella. Esta medida muestra el porcentaje de puntuaciones dentro de una distribución de frecuencia estándar que es inferior al rango del percentil que se está midiendo. La interpolación lineal hace una estimación del valor entre dos rangos.
Ver también
array.new_float
array.insert
array.slice
array.reverse
order.ascending
order.descending
array.percentile_nearest_rank()2 sobrecargas

Devuelve el valor para el cual el porcentaje especificado de valores del array (percentil) sea menor o igual al mismo, utilizando el método de clasificación más cercano.
Sintaxis y sobrecargas
array.percentile_nearest_rank(id, percentage) → series float
array.percentile_nearest_rank(id, percentage) → series int
Argumentos
id (array<int/float>) Objeto array.
percentage (series int/float) Porcentaje de valores que debe ser igual o menor que el valor devuelto.
Observaciones
En estadística, un percentil es el porcentaje de elementos de ranking que aparecen en una puntuación determinada o por debajo de ella. Esta medida muestra el porcentaje de puntuaciones dentro de una distribución de frecuencias estándar que es inferior al rango percentil que se está midiendo.
Ver también
array.new_float
array.insert
array.slice
array.reverse
order.ascending
order.descending
array.percentrank()2 sobrecargas

Devuelve el rango percentil del elemento en el index especificado.
Sintaxis y sobrecargas
array.percentrank(id, index) → series float
array.percentrank(id, index) → series int
Argumentos
id (array<int/float>) Objeto array.
index (series int) Índice del elemento para el que debe calcularse el rango percentil.
Observaciones
El rango percentil es el porcentaje de la cantidad de elementos del array que son menores o iguales al valor de referencia.
Ver también
array.new_float
array.insert
array.slice
array.reverse
order.ascending
order.descending
array.pop()

Función que elimina el último elemento del array y devuelve su valor.
Sintaxis
array.pop(id) → series <type>
Argumentos
id (any array type) Objeto array.
Ejemplo

//@version=5
indicator("array.pop example")
a = array.new_float(5,high)
removedEl = array.pop(a)
plot(array.size(a))
plot(removedEl)
Devoluciones
Valor del elemento eliminado.
Ver también
array.new_float
array.set
array.push
array.remove
array.insert
array.shift
array.push()

La función añade un valor a un array.
Sintaxis
array.push(id, value) → void
Argumentos
id (any array type) Objeto array.
value (series <type of the array's elements>) Valor del elemento añadido al final del array.
Ejemplo

//@version=5
indicator("array.push example")
a = array.new_float(5, 0)
array.push(a, open)
plot(array.get(a, 5))
Ver también
array.new_float
array.set
array.insert
array.remove
array.pop
array.unshift
array.range()2 sobrecargas

Función que devuelve la diferencia entre los valores mínimo y máximo de un array determinado.
Sintaxis y sobrecargas
array.range(id) → series float
array.range(id) → series int
Argumentos
id (array<int/float>) Objeto array.
Ejemplo

//@version=5
indicator("array.range example")
a = array.new_float(0)
for i = 0 to 9
    array.push(a, close[i])
plot(array.range(a))
Devoluciones
La diferencia entre los valores mínimo y máximo en el array.
Ver también
array.new_float
array.min
array.max
array.sum
array.remove()

La función modifica el contenido del array eliminando el elemento con el índice especificado.
Sintaxis
array.remove(id, index) → series <type>
Argumentos
id (any array type) Objeto array.
index (series int) Índice del elemento que se va a eliminar.
Ejemplo

//@version=5
indicator("array.remove example")
a = array.new_float(5,high)
removedEl = array.remove(a, 0)
plot(array.size(a))
plot(removedEl)
Devoluciones
Valor del elemento eliminado.
Ver también
array.new_float
array.set
array.push
array.insert
array.pop
array.shift
array.reverse()

Función que invierte un array. El primer elemento de la serie se convierte en el último y el último en el primero.
Sintaxis
array.reverse(id) → void
Argumentos
id (any array type) Objeto array.
Ejemplo

//@version=5
indicator("array.reverse example")
a = array.new_float(0)
for i = 0 to 9
    array.push(a, close[i])
plot(array.get(a, 0))
array.reverse(a)
plot(array.get(a, 0))
Ver también
array.new_float
array.sort
array.push
array.set
array.avg
array.set()

Función que establece el valor del elemento en el índice especificado.
Sintaxis
array.set(id, index, value) → void
Argumentos
id (any array type) Objeto array.
index (series int) Índice del elemento que se va a modificar.
value (series <type of the array's elements>) Nuevo valor a establecer.
Ejemplo

//@version=5
indicator("array.set example")
a = array.new_float(10)
for i = 0 to 9
    array.set(a, i, close[i])
plot(array.sum(a) / 10)
Ver también
array.new_float
array.get
array.slice
array.shift()

Función que elimina el primer elemento de un array y devuelve su valor.
Sintaxis
array.shift(id) → series <type>
Argumentos
id (any array type) Objeto array.
Ejemplo

//@version=5
indicator("array.shift example")
a = array.new_float(5,high)
removedEl = array.shift(a)
plot(array.size(a))
plot(removedEl)
Devoluciones
Valor del elemento eliminado.
Ver también
array.unshift
array.set
array.push
array.remove
array.includes
array.size()

Función que devuelve el número de elementos de un array.
Sintaxis
array.size(id) → series int
Argumentos
id (any array type) Objeto array.
Ejemplo

//@version=5
indicator("array.size example")
a = array.new_float(0)
for i = 0 to 9
    array.push(a, close[i])
// note that changes in slice also modify original array
slice = array.slice(a, 0, 5)
array.push(slice, open)
// size was changed in slice and in original array
plot(array.size(a))
plot(array.size(slice))
Devoluciones
Número de elementos del array.
Ver también
array.new_float
array.sum
array.slice
array.sort
array.slice()

Función que crea una sección a partir de una serie existente. Cualquier cambio de objeto que se realiza en la sección, se aplica tanto en el nuevo array como en el original.
Sintaxis
array.slice(id, index_from, index_to) → array<type>
Argumentos
id (any array type) Objeto array.
index_from (series int) Índice de base cero donde empezar la extracción.
index_to (series int) Índice base cero anterior a finalizar la extracción. La función extrae hasta el elemento con este índice, pero no lo incluye.
Ejemplo

//@version=5
indicator("array.slice example")
a = array.new_float(0)
for i = 0 to 9
    array.push(a, close[i])
// take elements from 0 to 4
// *note that changes in slice also modify original array 
slice = array.slice(a, 0, 5)
plot(array.sum(a) / 10)
plot(array.sum(slice) / 5)
Devoluciones
Copia superficial de la sección de un array.
Ver también
array.new_float
array.get
array.slice
array.sort
array.some()

Devuelve true si al menos un elemento del array id es true y, de lo contrario, false.
Sintaxis
array.some(id) → series bool
Argumentos
id (array<bool>) Objeto array.
Observaciones
Esta función también sirve con arrays de tipo int y float, en cuyo caso los valores cero se consideran false, y el resto true.
Ver también
array.every
array.get
array.sort()

Función que clasifica todos los elementos del array.
Sintaxis
array.sort(id, order) → void
Argumentos
id (array<int/float/string>) Objeto array.
order (series sort_order) Orden de clasificación: order.ascending (por defecto) u order.descending
Ejemplo

//@version=5
indicator("array.sort example")
a = array.new_float(0,0)
for i = 0 to 5
    array.push(a, high[i])
array.sort(a, order.descending)
if barstate.islast
    label.new(bar_index, close, str.tostring(a))
Ver también
array.new_float
array.insert
array.slice
array.reverse
order.ascending
order.descending
array.sort_indices()

Devuelve un array de índices que, cuando se utiliza para indexar el array original, accederá a sus elementos según el orden seleccionado. No modifica el array original.
Sintaxis
array.sort_indices(id, order) → array<int>
Argumentos
id (array<int/float/string>) Objeto array.
order (series sort_order) Orden de clasificación: order.ascending u order.descending. Es opcional. El valor por defecto es order.ascending.
Ejemplo

//@version=5
indicator("array.sort_indices")
a = array.from(5, -2, 0, 9, 1)
sortedIndices = array.sort_indices(a) // [1, 2, 4, 0, 3]
indexOfSmallestValue = array.get(sortedIndices, 0) // 1
smallestValue = array.get(a, indexOfSmallestValue) // -2
plot(smallestValue)
Ver también
array.new_float
array.insert
array.slice
array.reverse
order.ascending
order.descending
array.standardize()2 sobrecargas

Función que devuelve el array de elementos estandarizados.
Sintaxis y sobrecargas
array.standardize(id) → array<float>
array.standardize(id) → array<int>
Argumentos
id (array<int/float>) Objeto array.
Ejemplo

//@version=5
indicator("array.standardize example")
a = array.new_float(0)
for i = 0 to 9
    array.push(a, close[i])
b = array.standardize(a)
plot(array.min(b))
plot(array.max(b))
Devoluciones
Array de elementos estandarizados.
Ver también
array.max
array.min
array.mode
array.avg
array.variance
array.stdev
array.stdev()2 sobrecargas

Función que devuelve la desviación estándar de los elementos de un array.
Sintaxis y sobrecargas
array.stdev(id, biased) → series float
array.stdev(id, biased) → series int
Argumentos
id (array<int/float>) Objeto array.
biased (series bool) Determina el tipo de estimación a usar. Opcional. El valor por defecto es true.
Ejemplo

//@version=5
indicator("array.stdev example")
a = array.new_float(0)
for i = 0 to 9
    array.push(a, close[i])
plot(array.stdev(a))
Devoluciones
Desviación estándar de los elementos del array.
Observaciones
Si biased es true, la función realizará el cálculo utilizando una estimación sesgada de toda la población y, si es false, la muestra se realizará sin sesgos.
Ver también
array.new_float
array.max
array.min
array.avg
array.sum()2 sobrecargas

Función que devuelve la suma de los elementos del array.
Sintaxis y sobrecargas
array.sum(id) → series float
array.sum(id) → series int
Argumentos
id (array<int/float>) Objeto array.
Ejemplo

//@version=5
indicator("array.sum example")
a = array.new_float(0)
for i = 0 to 9
    array.push(a, close[i])
plot(array.sum(a))
Devoluciones
Suma de los elementos del array.
Ver también
array.new_float
array.max
array.min
array.unshift()

Función que inserta el valor al comienzo del array.
Sintaxis
array.unshift(id, value) → void
Argumentos
id (any array type) Objeto array.
value (series <type of the array's elements>) Valor que se añade al inicio del array.
Ejemplo

//@version=5
indicator("array.unshift example")
a = array.new_float(5, 0)
array.unshift(a, open)
plot(array.get(a, 0))
Ver también
array.shift
array.set
array.insert
array.remove
array.indexof
array.variance()2 sobrecargas

Función que devuelve la varianza de los elementos del array.
Sintaxis y sobrecargas
array.variance(id, biased) → series float
array.variance(id, biased) → series int
Argumentos
id (array<int/float>) Objeto array.
biased (series bool) Determina el tipo de estimación a usar. Opcional. El valor por defecto es true.
Ejemplo

//@version=5
indicator("array.variance example")
a = array.new_float(0)
for i = 0 to 9
    array.push(a, close[i])
plot(array.variance(a))
Devoluciones
Varianza de los elementos del array.
Observaciones
Si biased es true, la función realizará el cálculo utilizando una estimación sesgada de toda la población y, si es false, la muestra se realizará sin sesgos.
Ver también
array.new_float
array.stdev
array.min
array.avg
array.covariance
barcolor()

Color establecido de las barras.
Sintaxis
barcolor(color, offset, editable, show_last, title, display) → void
Argumentos
color (series color) Color de las barras. Puede utilizar constantes como 'red' o '# ff001a', así como expresiones complejas del tipo 'close >= open ? color.green : color.red'. Argumento obligatorio.
offset (series int) Cambia la serie de colores hacia la izquierda o hacia la derecha en el número determinado de barras. El valor por defecto es 0.
editable (const bool) Si es true , el estilo del color de las barras se podrá editar en el cuadro de diálogo Formato. El valor por defecto es true.
show_last (input int) Cuando está configurado, define el número de barras (contando a partir de la última barra y hacia atrás) que se rellenan en el gráfico.
title (const string) Titulo del barcolor. Argumento opcional.
display (input plot_simple_display) Controla dónde se muestra el barcolor. Los valores posibles son: display.none, display.all. Por defecto es display.all.
Ejemplo

//@version=5
indicator("barcolor example", overlay=true)
barcolor(close < open ? color.black : color.white)
Ver también
bgcolor
plot
fill
bgcolor()

Rellena el fondo de barras con el color indicado.
Sintaxis
bgcolor(color, offset, editable, show_last, title, display, force_overlay) → void
Argumentos
color (series color) Color del fondo rellenado. Puede utilizar constantes como 'red' o '# ff001a', así como expresiones complejas de tipo 'close >= open ? color.green : color.red'. Argumento obligatorio.
offset (series int) Cambia la serie de colores hacia la izquierda o hacia la derecha en el número determinado de barras. El valor por defecto es 0.
editable (const bool) Si es true, entonces el estilo bgcolor se podrá editar en el cuadro de diálogo Formato. El valor por defecto es true.
show_last (input int) Cuando está configurado, define el número de barras (contando a partir de la última barra y hacia atrás) que se rellenan en el gráfico.
title (const string) Título del bgcolor. Argumento opcional.
display (input plot_simple_display) Controla dónde se muestra el bgcolor. Los valores posibles son: display.none, display.all. Por defecto es display.all.
force_overlay (const bool) Si es true, los resultados trazados se mostrarán en el panel principal del gráfico, incluso cuando el script ocupe un panel separado. Es opcional. Por defecto es false.
Ejemplo

//@version=5
indicator("bgcolor example", overlay=true)
bgcolor(close < open ? color.new(color.red,70) : color.new(color.green, 70))
Ver también
barcolor
plot
fill
bool()4 sobrecargas

Convierte na en bool
Sintaxis y sobrecargas
bool(x) → const bool
bool(x) → input bool
bool(x) → simple bool
bool(x) → series bool
Argumentos
x (simple int/float/bool) Valor a convertir al tipo especificado, normalmente na.
Devoluciones
Valor del argumento después de convertirse en bool.
Ver también
float
int
color
string
line
label
box()

Convierte na en box.
Sintaxis
box(x) → series box
Argumentos
x (series box) Valor a convertir al tipo especificado, normalmente na.
Devoluciones
Valor del argumento después de convertirse en box.
Ver también
float
int
bool
color
string
line
label
box.copy()

Clona el objeto box.
Sintaxis
box.copy(id) → series box
Argumentos
id (series box) Objeto box.
Ejemplo

//@version=5
indicator('Last 50 bars price ranges', overlay = true)
LOOKBACK = 50
highest = ta.highest(LOOKBACK)
lowest = ta.lowest(LOOKBACK)
if barstate.islastconfirmedhistory
    var BoxLast = box.new(bar_index[LOOKBACK], highest, bar_index, lowest, bgcolor = color.new(color.green, 80))
    var BoxPrev = box.copy(BoxLast)
    box.set_lefttop(BoxPrev, bar_index[LOOKBACK * 2], highest[50])
    box.set_rightbottom(BoxPrev, bar_index[LOOKBACK], lowest[50])
    box.set_bgcolor(BoxPrev, color.new(color.red, 80))
Ver también
box.new
box.delete
box.delete()

Elimina el objeto box especificado. En el caso de que ya se haya eliminado, no hace nada.
Sintaxis
box.delete(id) → void
Argumentos
id (series box) El objeto box que se va a eliminar.
Ver también
box.new
box.get_bottom()

Devuelve el valor del precio del borde inferior del recuadro.
Sintaxis
box.get_bottom(id) → series float
Argumentos
id (series box) Un objeto box.
Devoluciones
Valor del precio.
Ver también
box.new
box.set_bottom
box.get_left()

Devuelve el índice de barras o el tiempo UNIX (en función del último valor de 'xloc') del borde izquierdo del recuadro.
Sintaxis
box.get_left(id) → series int
Argumentos
id (series box) Un objeto box.
Devoluciones
Índice de barras o UNIX timestamp (en milisegundos).
Ver también
box.new
box.set_left
box.get_right()

Devuelve el índice de barras o el tiempo UNIX (en función del último valor de 'xloc') del borde derecho del recuadro.
Sintaxis
box.get_right(id) → series int
Argumentos
id (series box) Un objeto box.
Devoluciones
Índice de barras o UNIX timestamp (en milisegundos).
Ver también
box.new
box.set_right
box.get_top()

Devuelve el valor del precio del borde superior del recuadro.
Sintaxis
box.get_top(id) → series float
Argumentos
id (series box) Un objeto box.
Devoluciones
Valor del precio.
Ver también
box.new
box.set_top
box.new()2 sobrecargas

Crea un nuevo objeto box.
Sintaxis y sobrecargas
box.new(top_left, bottom_right, border_color, border_width, border_style, extend, xloc, bgcolor, text, text_size, text_color, text_halign, text_valign, text_wrap, text_font_family, force_overlay) → series box
box.new(left, top, right, bottom, border_color, border_width, border_style, extend, xloc, bgcolor, text, text_size, text_color, text_halign, text_valign, text_wrap, text_font_family, force_overlay) → series box
Argumentos
top_left (chart.point) Objeto chart.point que especifica la posición de la esquina superior izquierda del recuadro.
bottom_right (chart.point) Objeto chart.point que especifica la ubicación de la esquina inferior derecha del recuadro.
border_color (series color) Ccolor de los cuatro bordes. Opcional. El valor por defecto es color.blue.
border_width (series int) Ancho de los cuatro bordes, en píxeles. Parámetro opcional. El valor por defecto es de 1 píxel.
border_style (series string) Estilo de los cuatro bordes. Valores posibles: line.style_solid, line.style_dotted, line.style_dashed. Opcional. El valor por defecto es line.style_solid.
extend (series string) Cuando se utiliza extend.none, los bordes horizontales empiezan en el borde izquierdo y terminan en el derecho. Con extend.left o extend.right, los bordes horizontales se extienden indefinidamente a la izquierda o derecha del recuadro, respectivamente. Con extend.both, los bordes horizontales se extienden a ambos lados. Opcional. El valor por defecto es extend.none.
xloc (series string) Determina si los argumentos 'left' y 'right' son un índice de barras o un valor de tiempo. Si xloc = xloc.bar_index, los argumentos deben ser un índice de barras. Si xloc = xloc.bar_time, los argumentos deben ser un tiempo UNIX. Valores posibles: xloc.bar_index y xloc.bar_time. Opcional. El valor por defecto es xloc.bar_index.
bgcolor (series color) Color de fondo del recuadro. Opcional. El valor por defecto es color.blue.
text (series string) Texto que se mostrará en el recuadro. Opcional. El valor por defecto es una cadena vacía.
text_size (series string) Tamaño del texto. Parámetro opcional cuyo valor por defecto es size.auto. Valores posibles: size.auto, size.tiny, size.small, size.normal, size.large, size.huge.
text_color (series color) Color del texto. Opcional. El valor por defecto es color.black.
text_halign (series string) Alineación horizontal del texto del recuadro. Opcional. El valor por defecto es text.align_center. Valores posibles: text.align_left, text.align_center, text.align_right.
text_valign (series string) Alineación vertical del texto del recuadro. Opcional. El valor por defecto es text.align_center. Valores posibles: text.align_top, text.align_center, text.align_bottom.
text_wrap (series string) Determina si el texto se presenta en una sola línea, extendiéndose más allá de la anchura del recuadro cuando sea necesario, o ajustado de modo que cada línea no sea más ancha que el propio recuadro (y recortado por el borde inferior si la altura del texto ajustado resultante es superior a la altura del recuadro). Opcional. El valor por defecto es text.wrap_none. Valores posibles: text.wrap_none, text.wrap_auto.
text_font_family (series string) Familia de fuentes del texto. Opcional. El valor por defecto es font.family_default. Valores posibles: font.family_default, font.family_monospace.
force_overlay (const bool) Si es true, el dibujo se mostrará en el panel principal del gráfico, incluso cuando el script ocupe un panel separado. Es opcional. Por defecto es false.
Ejemplo

//@version=5
indicator("box.new")
var b = box.new(time, open, time + 60 * 60 * 24, close, xloc=xloc.bar_time, border_style=line.style_dashed)
box.set_lefttop(b, time, 100)
box.set_rightbottom(b, time + 60 * 60 * 24, 500)
box.set_bgcolor(b, color.green)
Devoluciones
El ID del objeto box que se puede usar en las funciones box.set_*() y box.get_*().
Ver también
box.delete
box.get_left
box.get_top
box.get_right
box.get_bottom
box.set_top_left_point
box.set_left
box.set_top
box.set_bottom_right_point
box.set_right
box.set_bottom
box.set_border_color
box.set_bgcolor
box.set_border_width
box.set_border_style
box.set_extend
box.set_bgcolor()

Establece el color del fondo del recuadro.
Sintaxis
box.set_bgcolor(id, color) → void
Argumentos
id (series box) Un objeto box.
color (series color) Nuevo color del fondo.
Ver también
box.new
box.set_border_color()

Establece el color del borde del recuadro.
Sintaxis
box.set_border_color(id, color) → void
Argumentos
id (series box) Un objeto box.
color (series color) Nuevo color del borde.
Ver también
box.new
box.set_border_style()

Establece el estilo del borde del recuadro.
Sintaxis
box.set_border_style(id, style) → void
Argumentos
id (series box) Un objeto box.
style (series string) Nuevo estilo del borde.
Ver también
box.new
line.style_solid
box.set_border_width()

Establece el ancho del borde del recuadro.
Sintaxis
box.set_border_width(id, width) → void
Argumentos
id (series box) Un objeto box.
width (series int) Ancho de los cuatro bordes, en píxeles.
Ver también
box.new
box.set_bottom()

Establece la coordenada del borde inferior del recuadro.
Sintaxis
box.set_bottom(id, bottom) → void
Argumentos
id (series box) Un objeto box.
bottom (series int/float) Valor del precio del borde inferior.
Ver también
box.new
box.get_bottom
box.set_bottom_right_point()

Establece la ubicación de la esquina inferior derecha del recuadro id a point.
Sintaxis
box.set_bottom_right_point(id, point) → void
Argumentos
id (series box) Objeto box.
point (chart.point) Objeto chart.point.
box.set_extend()

Establece el tipo de extensión del borde del objeto box. Cuando se usa extend.none, los bordes horizontales empiezan en el borde izquierdo y terminan en el derecho. Con extend.left o extend.right, los bordes horizontales se extienden indefinidamente hacia la izquierda o derecha del recuadro, respectivamente. Con extend.both, los bordes horizontales se extienden en ambos lados.
Sintaxis
box.set_extend(id, extend) → void
Argumentos
id (series box) Un objeto box.
extend (series string) Nuevo tipo de extensión.
Ver también
box.new
extend.none
box.set_left()

Establece la coordenada del borde izquierdo del recuadro.
Sintaxis
box.set_left(id, left) → void
Argumentos
id (series box) Un objeto box.
left (series int) Índice de barra o tiempo de barra del borde izquierdo. Tenga en cuenta que los objetos posicionados utilizando xloc.bar_index no se pueden dibujar más allá de 500 barras.
Ver también
box.new
box.get_left
box.set_lefttop()

Establece las coordenadas de la izquierda y superior del recuadro.
Sintaxis
box.set_lefttop(id, left, top) → void
Argumentos
id (series box) Un objeto box.
left (series int) Índice de barra o tiempo de barra del borde izquierdo.
top (series int/float) Valor del precio del borde superior.
Ver también
box.new
box.get_left
box.get_top
box.set_right()

Establece la coordenada derecha del recuadro.
Sintaxis
box.set_right(id, right) → void
Argumentos
id (series box) Un objeto box.
right (series int) Índice de barra o tiempo de barra del borde derecho. Tenga en cuenta que los objetos posicionados que utilizan xloc.bar_index no se pueden dibujar más allá de 500 barras.
Ver también
box.new
box.get_right
box.set_rightbottom()

Establece las coordenadas derecha e inferior del recuadro.
Sintaxis
box.set_rightbottom(id, right, bottom) → void
Argumentos
id (series box) Un objeto box.
right (series int) Índice de barra y tiempo de barra del borde derecho.
bottom (series int/float) Valor del precio del borde inferior.
Ver también
box.new
box.get_right
box.get_bottom
box.set_text()

Función que establece el texto del recuadro.
Sintaxis
box.set_text(id, text) → void
Argumentos
id (series box) Un objeto box.
text (series string) El texto que se mostrará dentro del recuadro.
Ver también
box.set_text_color
box.set_text_size
box.set_text_valign
box.set_text_halign
box.set_text_color()

Función que establece el color del texto dentro del recuadro.
Sintaxis
box.set_text_color(id, text_color) → void
Argumentos
id (series box) Un objeto box.
text_color (series color) Color del texto.
Ver también
box.set_text
box.set_text_size
box.set_text_valign
box.set_text_halign
box.set_text_font_family()

Función que establece la familia de fuentes del texto dentro del recuadro.
Sintaxis
box.set_text_font_family(id, text_font_family) → void
Argumentos
id (series box) Un objeto box.
text_font_family (series string) Familia de fuentes del texto. Valores posibles: font.family_default, font.family_monospace.
Ejemplo

//@version=5
indicator("Example of setting the box font")
if barstate.islastconfirmedhistory
    b = box.new(bar_index, open-ta.tr, bar_index-50, open-ta.tr*5, text="monospace")
    box.set_text_font_family(b, font.family_monospace)
Ver también
box.new
font.family_default
font.family_monospace
box.set_text_halign()

Función que establece la alineación horizontal del texto del recuadro.
Sintaxis
box.set_text_halign(id, text_halign) → void
Argumentos
id (series box) Un objeto box.
text_halign (series string) Alineación horizontal del texto del recuadro. Valores posibles: text.align_left, text.align_center, text.align_right.
Ver también
box.set_text
box.set_text_size
box.set_text_valign
box.set_text_color
box.set_text_size()

Función que establece el tamaño del texto del recuadro.
Sintaxis
box.set_text_size(id, text_size) → void
Argumentos
id (series box) Un objeto box.
text_size (series string) Tamaño del texto. Valores posibles: size.auto, size.tiny, size.small, size.normal, size.large, size.huge.
Ver también
box.set_text
box.set_text_color
box.set_text_valign
box.set_text_halign
box.set_text_valign()

Función que establece la alineación vertical del texto del recuadro.
Sintaxis
box.set_text_valign(id, text_valign) → void
Argumentos
id (series box) Un objeto box.
text_valign (series string) Alineación vertical del texto del recuadro. Valores posibles: text.align_top, text.align_center, text.align_bottom.
Ver también
box.set_text
box.set_text_size
box.set_text_color
box.set_text_halign
box.set_text_wrap()

Función que establece el modo de envoltura del texto dentro del recuadro.
Sintaxis
box.set_text_wrap(id, text_wrap) → void
Argumentos
id (series box) Un objeto box.
text_wrap (series string) El modo de envoltura. Valores posibles: text.wrap_auto, text.wrap_none.
Ver también
box.set_text
box.set_text_size
box.set_text_valign
box.set_text_halign
box.set_text_color
box.set_top()

Establece la coordenada superior del recuadro.
Sintaxis
box.set_top(id, top) → void
Argumentos
id (series box) Un objeto box.
top (series int/float) Valor del precio del borde superior.
Ver también
box.new
box.get_top
box.set_top_left_point()

Establece la ubicación de la esquina superior izquierda del recuadro id a point.
Sintaxis
box.set_top_left_point(id, point) → void
Argumentos
id (series box) Objeto box.
point (chart.point) Objeto chart.point.
chart.point.copy()

Crea una copia de un objeto chart.point con el id especificado.
Sintaxis
chart.point.copy(id) → chart.point
Argumentos
id (chart.point) Objeto chart.point.
chart.point.from_index()

Devuelve un objeto chart.point con index como coordenada x, así como price como coordenada y.
Sintaxis
chart.point.from_index(index, price) → chart.point
Argumentos
index (series int) La coordenada x del punto, expresada como un valor de índice de barra.
price (series int/float) La coordenada y del punto.
Observaciones
Los valores del campo time de las instancias chart.point devueltas por esta función serán na, lo que significa que no funcionarán con ellos los objetos de dibujo con valores xloc establecidos en xloc.bar_time.
chart.point.from_time()

Devuelve un objeto chart.point con time como coordenada x, así como price como coordenada y.
Sintaxis
chart.point.from_time(time, price) → chart.point
Argumentos
time (series int) La coordenada x del punto, expresada en milisegundos como un valor de tiempo UNIX.
price (series int/float) La coordenada y del punto.
Observaciones
Los valores del campo index de las instancias chart.point devueltas por esta función serán na, lo que significa que no funcionarán con ellos los objetos de dibujo con valores xloc establecidos en xloc.bar_index.
chart.point.new()

Crea un nuevo objeto chart.point con los valores time, index y price especificados.
Sintaxis
chart.point.new(time, index, price) → chart.point
Argumentos
time (series int) La coordenada x del punto, expresada en milisegundos como un valor de tiempo UNIX.
index (series int) La coordenada x del punto, expresada como un valor de índice de barra.
price (series int/float) La coordenada y del punto.
Observaciones
Que un objeto de dibujo utilice el campo time o index de un punto como coordenada x depende del tipo xloc utilizado en la llamada a la función que devolvió el dibujo.
Es importante tener en cuenta que esta función no verifica que los valores time y index hagan referencia a la misma barra.
Ver también
polyline.new
chart.point.now()

Devuelve un objeto chart.point con price como coordenada y
Sintaxis
chart.point.now(price) → chart.point
Argumentos
price (series int/float) Coordenada y del punto. Opcional. El valor por defecto es close.
Observaciones
La instancia chart.point devuelta por esta función registra valores para sus campos index y time en la barra en la que se ejecutó, haciéndola adecuada para su uso con objetos de dibujo de cualquier tipo xloc.
color()4 sobrecargas

Convierte na en color
Sintaxis y sobrecargas
color(x) → const color
color(x) → input color
color(x) → simple color
color(x) → series color
Argumentos
x (const color) Valor a convertir al tipo especificado, normalmente na.
Devoluciones
Valor del argumento después de convertirse en color.
Ver también
float
int
bool
string
line
label
color.b()4 sobrecargas

Recupera el valor del componente azul del color.
Sintaxis y sobrecargas
color.b(color) → const float
color.b(color) → input float
color.b(color) → simple float
color.b(color) → series float
Argumentos
color (const color) Color.
Ejemplo

//@version=5
indicator("color.b", overlay=true)
plot(color.b(color.blue))
Devoluciones
Valor (de 0 a 255) del componente azul del color.
color.from_gradient()

En función de la posición relativa del valor en el rango bottom_value a top_value, la función devuelve el color del gradiente especificado por bottom_color a top_color.
Sintaxis
color.from_gradient(value, bottom_value, top_value, bottom_color, top_color) → series color
Argumentos
value (series int/float) Valor para calcular el color dependiente de la posición.
bottom_value (series int/float) Valor de la posición inferior correspondiente a bottom_color.
top_value (series int/float) Valor de la posición superior correspondiente a top_color.
bottom_color (series color) Color de la posición inferior.
top_color (series color) Color de la posición superior.
Ejemplo

//@version=5
indicator("color.from_gradient", overlay=true)
color1 = color.from_gradient(close, low, high, color.yellow, color.lime)
color2 = color.from_gradient(ta.rsi(close, 7), 0, 100, color.rgb(255, 0, 0), color.rgb(0, 255, 0, 50))
plot(close, color=color1)
plot(ta.rsi(close,7), color=color2)
Devoluciones
Color que se calcula a partir del gradiente lineal entre bottom_color a top_color.
Observaciones
Utilizar esta función afecta a los colores que se muestran en la pestaña "Configuración/Estilo" del script. Para obtener más información, consulte el Manual de usuario.
color.g()4 sobrecargas

Recupera el valor del componente verde del color.
Sintaxis y sobrecargas
color.g(color) → const float
color.g(color) → input float
color.g(color) → simple float
color.g(color) → series float
Argumentos
color (const color) Color.
Ejemplo

//@version=5
indicator("color.g", overlay=true)
plot(color.g(color.green))
Devoluciones
Valor (de 0 a 255) del componente verde del color.
color.new()4 sobrecargas

El color de la función aplica la transparencia especificada al color indicado.
Sintaxis y sobrecargas
color.new(color, transp) → const color
color.new(color, transp) → input color
color.new(color, transp) → simple color
color.new(color, transp) → series color
Argumentos
color (const color) Color al que aplicar la transparencia.
transp (const int/float) Los valores posibles van desde el 0 (no transparente) hasta 100 (invisible).
Ejemplo

//@version=5
indicator("color.new", overlay=true)
plot(close, color=color.new(color.red, 50))
Devoluciones
Color con transparencia especificada.
Observaciones
Utilizar argumentos que no son constantes (por ejemplo, 'simple', 'input' o 'series') afecta a los colores que se muestran en la pestaña "Configuración/Estilo" del script. Para obtener más información, consulte el Manual de usuario.
color.r()4 sobrecargas

Recupera el valor del componente rojo del color.
Sintaxis y sobrecargas
color.r(color) → const float
color.r(color) → input float
color.r(color) → simple float
color.r(color) → series float
Argumentos
color (const color) Color.
Ejemplo

//@version=5
indicator("color.r", overlay=true)
plot(color.r(color.red))
Devoluciones
Valor (de 0 a 255) del componente rojo del color.
color.rgb()4 sobrecargas

Crea un nuevo color con transparencia utilizando el modelo de color RGB (rojo, verde y azul).
Sintaxis y sobrecargas
color.rgb(red, green, blue, transp) → const color
color.rgb(red, green, blue, transp) → input color
color.rgb(red, green, blue, transp) → simple color
color.rgb(red, green, blue, transp) → series color
Argumentos
red (const int/float) Componente de color rojo. Valores posibles: de 0 a 255.
green (const int/float) Componente de color verde. Valores posibles: de 0 a 255.
blue (const int/float) Componente de color azul. Valores posibles: de 0 a 255.
transp (const int/float) Opcional. Transparencia del color. Los valores posibles van desde el 0 (opaco) al 100 (invisible). El valor por defecto es 0.
Ejemplo

//@version=5
indicator("color.rgb", overlay=true)
plot(close, color=color.rgb(255, 0, 0, 50))
Devoluciones
Color con transparencia especificada.
Observaciones
Utilizar argumentos que no son constantes (por ejemplo, 'simple', 'input' o 'series') afecta a los colores que se muestran en la pestaña "Configuración/Estilo" del script. Para obtener más información, consulte el Manual de usuario.
color.t()4 sobrecargas

Recupera la transparencia del color.
Sintaxis y sobrecargas
color.t(color) → const float
color.t(color) → input float
color.t(color) → simple float
color.t(color) → series float
Argumentos
color (const color) Color.
Ejemplo

//@version=5
indicator("color.t", overlay=true)
plot(color.t(color.new(color.red, 50)))
Devoluciones
Valor (de 0 a 100) de la transparencia del color.
dayofmonth()2 sobrecargas

Sintaxis y sobrecargas
dayofmonth(time) → series int
dayofmonth(time, timezone) → series int
Argumentos
time (series int) Tiempo UNIX en milisegundos.
Devoluciones
Día del mes (en zona horaria del mercado) para el tiempo UNIX proporcionado.
Observaciones
Tiempo UNIX es el número de milisegundos que han transcurrido desde las 00:00:00 UTC, del 1 de enero de 1970.
Observe que esta función devuelve el día en función de la fecha del comienzo de la barra. Para las sesiones overnight (por ejemplo, EURUSD, en la que la sesión del lunes empieza el domingo, a las 17:00 UTC-4) este valor puede ser 1 punto inferior al día del trading.
Ver también
dayofmonth
time
year
month
dayofweek
hour
minute
second
dayofweek()2 sobrecargas

Sintaxis y sobrecargas
dayofweek(time) → series int
dayofweek(time, timezone) → series int
Argumentos
time (series int) Tiempo UNIX en milisegundos.
Devoluciones
Día de la semana (en zona horaria del mercado) para el tiempo UNIX proporcionado.
Observaciones
Observe que esta función devuelve el día en función de la fecha del comienzo de la barra. Para las sesiones overnight (por ejemplo, EURUSD, en la que la sesión del lunes empieza el domingo, a las 17:00) este valor puede ser 1 punto inferior al día del trading.
Tiempo UNIX es el número de milisegundos que han transcurrido desde las 00:00:00 UTC, del 1 de enero de 1970.
Ver también
dayofweek
time
year
month
dayofmonth
hour
minute
second
fill()3 sobrecargas

Rellena el fondo entre dos plots (trazados) o líneas horizontales (hlines) con un color determinado.
Sintaxis y sobrecargas
fill(hline1, hline2, color, title, editable, fillgaps, display) → void
fill(plot1, plot2, color, title, editable, show_last, fillgaps, display) → void
fill(plot1, plot2, top_value, bottom_value, top_color, bottom_color, title, display, fillgaps, editable) → void
Argumentos
hline1 (hline) El primer objeto hline. Argumento obligatorio.
hline2 (hline) Segundo objeto hline. Argumento obligatorio.
color (series color) Color del relleno del fondo. Puede utilizar constantes como 'color= color.red' o 'color=#ff001a', así como expresiones complejas de tipo 'color = close >= open ? color.green : color.red'. Argumento opcional.
title (const string) Titulo del objeto fill creado. Argumento opcional.
editable (const bool) Si es true, entonces el estilo de fill se podrá editar en el cuadro de diálogo Formato. El valor por defecto es true.
fillgaps (const bool) Controla las continuas ejecuciones de gaps, es decir, cuando una de las llamadas a plot() devuelve un valor na. Cuando es true, la última ejecución es la que continuará en los gaps. El valor por defecto es false.
display (input plot_simple_display) Controla dónde se muestra el fill. Los valores posibles son: display.none, display.all. Por defecto es display.all.
Relleno entre dos líneas horizontales
Ejemplo

//@version=5
indicator("Fill between hlines", overlay = false)
h1 = hline(20)
h2 = hline(10)
fill(h1, h2, color = color.new(color.blue, 90))
Relleno entre dos trazados
Ejemplo

//@version=5
indicator("Fill between plots", overlay = true)
p1 = plot(open)
p2 = plot(close)
fill(p1, p2, color = color.new(color.green, 90))
Relleno del gradiente entre dos líneas horizontales
Ejemplo

//@version=5
indicator("Gradient Fill between hlines", overlay = false)
topVal = input.int(100)
botVal = input.int(0)
topCol = input.color(color.red)
botCol = input.color(color.blue)
topLine = hline(100, color = topCol, linestyle = hline.style_solid)
botLine = hline(0,   color = botCol, linestyle = hline.style_solid)
fill(topLine, botLine, topVal, botVal, topCol, botCol)
Ver también
plot
barcolor
bgcolor
hline
color.new
fixnan()4 sobrecargas

Para una serie determinada, se sustituyen los valores de NaN con el valor anterior sin NaN más próximo.
Sintaxis y sobrecargas
fixnan(source) → series color
fixnan(source) → series int
fixnan(source) → series float
fixnan(source) → series bool
Argumentos
source (series color) Fuente utilizada para el cálculo.
Devoluciones
Serie sin gaps na
Ver también
na
na
nz
float()4 sobrecargas

Convierte na en float
Sintaxis y sobrecargas
float(x) → const float
float(x) → input float
float(x) → simple float
float(x) → series float
Argumentos
x (const int/float) Valor a convertir al tipo especificado, normalmente na.
Devoluciones
Valor del argumento después de convertirse en float.
Ver también
int
bool
color
string
line
label
hline()

Representa una linea horizontal a un nivel de precio fijo establecido.
Sintaxis
hline(price, title, color, linestyle, linewidth, editable, display) → hline
Argumentos
price (input int/float) Valor del precio al que se representará el objeto. Argumento obligatorio.
title (const string) Titulo del objeto.
color (input color) Color de la línea representada. Debe ser un valor constante (no una expresión). Argumento opcional.
linestyle (input hline_style) Estilo de la línea renderizada. Valores posibles: hline.style_solid, hline.style_dotted, hline.style_dashed. Argumento opcional.
linewidth (input int) Ancho de la línea representada. El valor por defecto es 1.
editable (const bool) Si es true, entonces el estilo hline se podrá editar en el cuadro de diálogo Formato. El valor por defecto es true.
display (input plot_simple_display) Controla dónde se muestra la hline. Los valores posibles son: display.none, display.all. Por defecto es display.all.
Ejemplo

//@version=5
indicator("input.hline", overlay=true)
hline(3.14, title='Pi', color=color.blue, linestyle=hline.style_dotted, linewidth=2)

// You may fill the background between any two hlines with a fill() function:
h1 = hline(20)
h2 = hline(10)
fill(h1, h2, color=color.new(color.green, 90))
Devoluciones
Objeto hline, que se puede utilizar en fill
Ver también
fill
hour()2 sobrecargas

Sintaxis y sobrecargas
hour(time) → series int
hour(time, timezone) → series int
Argumentos
time (series int) Tiempo UNIX en milisegundos.
Devoluciones
Hora (en zona horaria del mercado) para el tiempo UNIX proporcionado.
Observaciones
Tiempo UNIX es el número de milisegundos que han transcurrido desde las 00:00:00 UTC, del 1 de enero de 1970.
Ver también
hour
time
year
month
dayofmonth
dayofweek
minute
second
indicator()

Esta sentencia de declaración designa el script como un indicador y establece una serie de propiedades relacionadas con el indicador.
Sintaxis
indicator(title, shorttitle, overlay, format, precision, scale, max_bars_back, timeframe, timeframe_gaps, explicit_plot_zorder, max_lines_count, max_labels_count, max_boxes_count, calc_bars_count, max_polylines_count, dynamic_requests, behind_chart) → void
Argumentos
title (const string) Título del script. Se muestra en el gráfico cuando no se utiliza el argumento shorttitle, y se convierte en el título por defecto de la publicación al publicar el script.
shorttitle (const string) Nombre de visualización del script en los gráficos. Si se especifica, sustituirá al argumento title en la mayoría de las ventanas relacionadas con los gráficos. Opcional. Por defecto es el argumento utilizado para title.
overlay (const bool) Si es true, se mostrará el indicador sobre el gráfico. Si es false, se añadirá en un panel aparte. Opcional. Por defecto es false.
format (const string) Especifica el formato de los valores mostrados del script. Valores posibles: format.inherit, format.price, format.volume, format.percent. Opcional. El valor por defecto es format.inherit.
precision (const int) Especifica el número de dígitos después del punto flotante de los valores mostrados del script. Debe ser un entero no negativo no mayor que 16. Si format se establece en format.inherit y se especifica precision, el formato se establecerá en format.price. Cuando el parámetro format de la función utiliza format.volume, el parámetro precision no afectará al resultado, ya que las reglas de precisión decimal definidas por format.volume prevalecen sobre otros ajustes de precisión. Es opcional. El valor por defecto se hereda de la precisión del símbolo del gráfico.
scale (const scale_type) Escala de precios utilizada. Valores posibles: scale.right, scale.left, scale.none. El valor scale.none solo puede aplicarse en combinación con overlay = true. Opcional. Por defecto, el script utiliza la misma escala que el gráfico.
max_bars_back (const int) Longitud del búfer histórico que mantiene el script para cada variable y función y que determina cuántos valores anteriores se pueden referenciar utilizando el operador de referencia histórica []. El tiempo de ejecución runtime de Pine Script® detecta, de forma automática, el tamaño del búfer necesario. Solo se precisa este parámetro cuando se produce un error en el tiempo de ejecución debido a algún fallo en la detección automática. Puede obtener más información sobre la mecánica subyacente del búfer histórico en nuestro Centro de ayuda. Es opcional. El valor por defecto es 0.
timeframe (const string) Añade la funcionalidad de múltiples intervalos de tiempo a los scripts simples. Cuando se especifique, se incluirá en la sección "Cálculo" de la pestaña "Configuración/entradas" del script. El valor por defecto del campo será el argumento proporcionado, cuyo formato debe ajustarse a las especificaciones de la cadena de intervalo de tiempo. Para especificar el intervalo de tiempo del gráfico, utilice una cadena vacía o la variable timeframe.period. El parámetro no se puede usar con scripts que utilicen dibujos Pine Script®. Es opcional. El valor por defecto es timeframe.period.
timeframe_gaps (const bool) Especifica cómo se muestran los valores del indicador en las barras del gráfico cuando el timeframe es superior al del gráfico. Si es true, solo aparece un valor en una barra del gráfico cuando se encuentre disponible el valor timeframe más alto, de lo contrario devuelve na (por lo que se produce un "gap"). Con false, lo que de otro modo serían gaps, se rellenan con el último valor conocido devuelto, evitando los valores na. Cuando se especifique, aparecerá la casilla de verificación "Esperar a que se cierren los intervalos de tiempo" en la sección "Cálculo" de la pestaña "Configuración/Entradas" del script. Es opcional. El valor por defecto es true.
explicit_plot_zorder (const bool) Especifica el orden en el que se dibujan los plots, fills y hlines del script. Si es true, los trazados se dibujan por orden de aparición en el código del script, es decir, los más recientes por encima de los anteriores. Esto solo es aplicable a las funciones plot*(), fill y hline. Es opcional. El valor por defecto es false.
max_lines_count (const int) Cantidad de dibujos que se muestran en el último line. Valores posibles: de 1 a 500. El recuento es aproximado; puede que se muestren más dibujos de los indicados. Es opcional. El valor por defecto es 50.
max_labels_count (const int) Cantidad de dibujos que se muestran en el último label. Valores posibles: de 1 a 500. El recuento es aproximado; puede que se muestren más dibujos de los indicados. Es opcional. El valor por defecto es 50.
max_boxes_count (const int) Cantidad de dibujos que se muestran en el último box. Valores posibles: de 1 a 500. El recuento es aproximado; puede que se muestren más dibujos de los indicados. Es opcional. El valor por defecto es 50.
calc_bars_count (const int) Limita el cálculo inicial de un script al último número de barras especificado. Cuando se especifique, se incluirá un campo de "Barras calculadas" en la sección "Cálculo" de la pestaña "Configuración/Entradas" del script. Es opcional. El valor por defecto es 0, en cuyo caso el script se ejecuta en todas las barras disponibles.
max_polylines_count (const int) Cantidad de dibujos que se muestran en el último polyline. Valores posibles: de 1 a 100. El recuento es aproximado; es posible que se muestren más dibujos que los especificados. Opcional. El valor por defecto es 50.
dynamic_requests (const bool) Especifica si el script puede llamar dinámicamente a funciones del espacio de nombres request.*(). Las llamadas dinámicas request.*() están permitidas dentro de los ámbitos locales de estructuras condicionales (por ejemplo, if), bucles (por ejemplo, for) y funciones exportadas. Además, estas llamadas permiten argumentos «en serie» para muchos de sus parámetros. Opcional. Por defecto es false. Para obtener más información consulte la sección Solicitudes dinámicas del Manual de usuario.
behind_chart (const bool) Controla si los trazados y dibujos del script en el panel del gráfico principal aparecen detrás de la pantalla del gráfico (si es true), o delante de ella (si es false). Opcional. Por defecto es true.
Ejemplo

//@version=5
indicator("My script", shorttitle="Script")
plot(close)
Observaciones
Cada script de indicador debe tener una llamada a indicator.
Ver también
strategy
library
input()6 sobrecargas

Añade un parámetro de entrada a la pestaña Entradas en la Configuración del script que permite especificar las opciones de configuración para los que utilizan el script. Esta función detecta automáticamente el tipo de argumento utilizado para 'defval' y utiliza el widget de parámetros de entrada correspondiente.
Sintaxis y sobrecargas
input(defval, title, tooltip, inline, group, display) → input color
input(defval, title, tooltip, inline, group, display) → input string
input(defval, title, tooltip, inline, group, display) → input int
input(defval, title, tooltip, inline, group, display) → input float
input(defval, title, inline, group, tooltip, display) → series float
input(defval, title, tooltip, inline, group, display) → input bool
Argumentos
defval (const int/float/bool/string/color or source-type built-ins) Establece el valor por defecto de la variable de entrada propuesto en la pestaña Configuración/Entradas del script, donde el usuario puede modificarlo. Las variables integradas de tipo source son variables float que especifican la fuente del cálculo: close, hlc3, etc.
title (const string) Título de la entrada. Si no se especifica, se utiliza el nombre de la variable como título de la entrada. Si se especifica el título, pero está vacío, el nombre será una cadena vacía.
tooltip (const string) La cadena que se mostrará al usuario al pasar el cursor por encima del icono de la descripción emergente.
inline (const string) Combina todas las llamadas de entrada utilizando el mismo argumento en una línea. La cadena utilizada como argumento no se muestra. Solo se utiliza para identificar entradas pertenecientes a la misma línea.
group (const string) Crea un encabezado sobre todas las entradas y utiliza la misma cadena del argumento de grupo. La cadena también se utiliza como texto del encabezado.
display (const plot_display) Controla dónde mostrará el script la información de la entrada, aparte de dentro de la configuración del script. Esta opción permite eliminar una entrada específica de la Línea de estado del script o de la Ventana de datos para garantizar que sólo se muestren allí las entradas más necesarias. Valores posibles: display.none, display.data_window, display.status_line, display.all. Es opcional. El valor por defecto depende del tipo de valor pasado a defval: display.none para los valores bool y color, display.all para todo lo demás.
Ejemplo

//@version=5
indicator("input", overlay=true)
i_switch = input(true, "On/Off")
plot(i_switch ? open : na)

i_len = input(7, "Length")
i_src = input(close, "Source")
plot(ta.sma(i_src, i_len))

i_border = input(142.50, "Price Border")
hline(i_border)
bgcolor(close > i_border ? color.green : color.red)

i_col = input(color.red, "Plot Color")
plot(close, color=i_col)

i_text = input("Hello!", "Message")
l = label.new(bar_index, high, text=i_text)
label.delete(l[1])
Devoluciones
Valor de la variable de entrada.
Observaciones
El resultado de la función input se debe asignar en todo momento a una variable. Consulte los ejemplos anteriores.
Ver también
input.bool
input.color
input.int
input.float
input.string
input.symbol
input.timeframe
input.text_area
input.session
input.source
input.time
input.bool()

Añade un parámetro de entrada a la pestaña Entradas de la Configuración del script que permite especificar las opciones para los que utilizan el script. Esta función marca los parámetros de entrada del script.
Sintaxis
input.bool(defval, title, tooltip, inline, group, confirm, display) → input bool
Argumentos
defval (const bool) Determina el valor por defecto de la variable de entrada propuesta en la pestaña "Configuración/Entradas" del script, donde el usuario puede modificarlo.
title (const string) Título de la entrada. Si no se especifica, se utiliza el nombre de la variable como título de la entrada. Si se especifica el título, pero está vacío, el nombre será una cadena vacía.
tooltip (const string) La cadena que se mostrará al usuario al pasar el cursor por encima del icono de la descripción emergente.
inline (const string) Combina todas las llamadas de entrada utilizando el mismo argumento en una línea. La cadena utilizada como argumento no se muestra. Solo se utiliza para identificar entradas pertenecientes a la misma línea.
group (const string) Crea un encabezado sobre todas las entradas y utiliza la misma cadena del argumento de grupo. La cadena también se utiliza como texto del encabezado.
confirm (const bool) Si es true, se pedirá al usuario que confirme el valor de entrada antes de añadir el indicador al gráfico. El valor por defecto es false.
display (const plot_display) Controla dónde mostrará el script la información de la entrada, aparte de la que contenga la configuración del propio script. Esta opción permite eliminar una entrada específica de la línea de estado del script o de la Ventana de datos para garantizar que solo se muestren allí las entradas más necesarias. Valores posibles: display.none, display.data_window, display.status_line, display.all. Es opcional. El valor por defecto es display.none.
Ejemplo

//@version=5
indicator("input.bool", overlay=true)
i_switch = input.bool(true, "On/Off")
plot(i_switch ? open : na)
Devoluciones
Valor de la variable de entrada.
Observaciones
El resultado de la función input.bool se debe asignar en todo momento a una variable. Consulte los ejemplos anteriores.
Ver también
input.int
input.float
input.string
input.text_area
input.symbol
input.timeframe
input.session
input.source
input.color
input.time
input
input.color()

Añade un parámetro de entrada en la pestaña Entradas en la Configuración del script, lo que permite proporcionar opciones de configuración a los usuarios del script. Esta función añade una herramienta para escoger los colores y que permite al usuario seleccionar el color, así como la transparencia a partir de una paleta o de un valor hexadecimal.
Sintaxis
input.color(defval, title, tooltip, inline, group, confirm, display) → input color
Argumentos
defval (const color) Determina el valor por defecto de la variable de entrada propuesta en la pestaña "Configuración/Entradas" del script, donde el usuario puede modificarlo.
title (const string) Título de la entrada. Si no se especifica, se utiliza el nombre de la variable como título de la entrada. Si se especifica el título, pero está vacío, el nombre será una cadena vacía.
tooltip (const string) La cadena que se mostrará al usuario al pasar el cursor por encima del icono de la descripción emergente.
inline (const string) Combina todas las llamadas de entrada utilizando el mismo argumento en una línea. La cadena utilizada como argumento no se muestra. Solo se utiliza para identificar entradas pertenecientes a la misma línea.
group (const string) Crea un encabezado sobre todas las entradas y utiliza la misma cadena del argumento de grupo. La cadena también se utiliza como texto del encabezado.
confirm (const bool) Si es true, se pedirá al usuario que confirme el valor de entrada antes de añadir el indicador al gráfico. El valor por defecto es false.
display (const plot_display) Controla dónde mostrará el script la información de la entrada, aparte de la que contenga la configuración del propio script. Esta opción permite eliminar una entrada específica de la línea de estado del script o de la Ventana de datos para garantizar que solo se muestren allí las entradas más necesarias. Valores posibles: display.none, display.data_window, display.status_line, display.all. Es opcional. El valor por defecto es display.none.
Ejemplo

//@version=5
indicator("input.color", overlay=true)
i_col = input.color(color.red, "Plot Color")
plot(close, color=i_col)
Devoluciones
Valor de la variable de entrada.
Observaciones
El resultado de la función input.color se debe asignar en todo momento a una variable. Consulte los ejemplos anteriores.
Ver también
input.bool
input.int
input.float
input.string
input.text_area
input.symbol
input.timeframe
input.session
input.source
input.time
input
input.enum()

Añade una entrada en la pestaña Entradas de la Configuración de su script, que proporciona diversas opciones a los usuarios del script para realizar ajustes. Esta función añade un desplegable con opciones basadas en los campos enum pasados a sus parámetros defval y options.
El texto de cada opción del desplegable resultante corresponde a los títulos de los campos incluidos. Si el título de un campo no se especifica en la declaración del enum, su título es la representación en forma de cadena de su nombre.
Sintaxis
input.enum(defval, title, options, tooltip, inline, group, confirm, display) → input enum
Argumentos
defval (const enum) Determina el valor por defecto de la entrada, aunque los usuarios pueden cambiarlo en la pestaña "Configuración/Entradas" del script. Cuando el parámetro options tiene una tupla especificada de campos enum, esta debe incluir defval.
title (const string) Título de la entrada. Si no se especifica, se utiliza el nombre de la variable como título de la entrada. Si se especifica el título, pero está vacío, el nombre será una cadena vacía.
options (tuple of enum fields: [enumName.field1, enumName.field2, ...]) Lista de opciones a escoger. Es opcional. Por defecto, los títulos de todos los campos del enum están disponibles en el desplegable. Pasar una tupla como argumento options limita la lista solo a los campos incluidos.
tooltip (const string) La cadena que se mostrará al usuario al pasar el cursor por encima del icono de la descripción emergente.
inline (const string) Combina todas las llamadas de entrada utilizando el mismo argumento en una línea. La cadena utilizada como argumento no se muestra. Solo se utiliza para identificar entradas pertenecientes a la misma línea.
group (const string) Crea un encabezado sobre todas las entradas y utiliza la misma cadena del argumento de grupo. La cadena también se utiliza como texto del encabezado.
confirm (const bool) Si es true, se pedirá al usuario que confirme el valor introducido antes de añadir el indicador al gráfico. El valor por defecto es false.
display (const plot_display) Controla dónde mostrará el script la información de la entrada, aparte de la que contenga la configuración del propio script. Esta opción permite eliminar una entrada específica de la línea de estado del script o de la Ventana de datos para garantizar que solo se muestren allí las entradas más necesarias. Valores posibles: display.none, display.data_window, display.status_line, display.all. Es opcional. El valor por defecto es display.all.
Ejemplo

//@version=5
indicator("Session highlight", overlay = true)

//@enum        Contains fields with popular timezones as titles.
//@field exch  Has an empty string as the title to represent the chart timezone.
enum tz
    utc  = "UTC"
    exch = ""
    ny   = "America/New_York"
    chi  = "America/Chicago"
    lon  = "Europe/London"
    tok  = "Asia/Tokyo"

//@variable The session string.
selectedSession = input.session("1200-1500", "Session")
//@variable The selected timezone. The input's dropdown contains the fields in the `tz` enum.
selectedTimezone = input.enum(tz.utc, "Session Timezone")

//@variable Is `true` if the current bar's time is in the specified session.
bool inSession = false
if not na(time("", selectedSession, str.tostring(selectedTimezone)))
    inSession := true

// Highlight the background when `inSession` is `true`.
bgcolor(inSession ? color.new(color.green, 90) : na, title = "Active session highlight")
Devoluciones
Valor de la variable de entrada.
Observaciones
Todos los campos incluidos en los argumentos defval y options deben pertenecer al mismo enum.
Ver también
input.text_area
input.bool
input.int
input.float
input.symbol
input.timeframe
input.session
input.source
input.color
input.time
input
input.float()2 sobrecargas

Añade un parámetro de entrada en la pestaña Entradas en la Configuración del script, lo que permite proporcionar opciones de configuración a los usuarios del script. Esta función añade un campo para introducir un valor de tipo float a las entradas del script.
Sintaxis y sobrecargas
input.float(defval, title, options, tooltip, inline, group, confirm, display) → input float
input.float(defval, title, minval, maxval, step, tooltip, inline, group, confirm, display) → input float
Argumentos
defval (const int/float) Establece el valor por defecto de la variable de entrada propuesto en la pestaña Configuración/Entradas del script, donde el usuario puede modificarlo. Cuando se utiliza una lista de valores con el parámetro options, el valor debe corresponder a uno de esta lista.
title (const string) Título de la entrada. Si no se especifica, se utiliza el nombre de la variable como título de la entrada. Si se especifica el título, pero está vacío, el nombre será una cadena vacía.
options (tuple of const int/float values: [val1, val2, ...]) Lista separada por comas de las opciones del menú desplegable encerradas entre corchetes: [val1, val2, ...]. Los parámetros minval, maxval y step no se pueden utilizar con este parámetro.
tooltip (const string) La cadena que se mostrará al usuario al pasar el cursor por encima del icono de la descripción emergente.
inline (const string) Combina todas las llamadas de entrada utilizando el mismo argumento en una línea. La cadena utilizada como argumento no se muestra. Solo se utiliza para identificar entradas pertenecientes a la misma línea.
group (const string) Crea un encabezado sobre todas las entradas y utiliza la misma cadena del argumento de grupo. La cadena también se utiliza como texto del encabezado.
confirm (const bool) Si es true, se pedirá al usuario que confirme el valor de entrada antes de añadir el indicador al gráfico. El valor por defecto es false.
display (const plot_display) Controla dónde mostrará el script la información de la entrada, aparte de la que contenga la configuración del propio script. Esta opción permite eliminar una entrada específica de la línea de estado del script o de la Ventana de datos para garantizar que solo se muestren allí las entradas más necesarias. Valores posibles: display.none, display.data_window, display.status_line, display.all. Es opcional. El valor por defecto es display.all.
Ejemplo

//@version=5
indicator("input.float", overlay=true)
i_angle1 = input.float(0.5, "Sin Angle", minval=-3.14, maxval=3.14, step=0.02)
plot(math.sin(i_angle1) > 0 ? close : open, "sin", color=color.green)

i_angle2 = input.float(0, "Cos Angle", options=[-3.14, -1.57, 0, 1.57, 3.14])
plot(math.cos(i_angle2) > 0 ? close : open, "cos", color=color.red)
Devoluciones
Valor de la variable de entrada.
Observaciones
El resultado de la función input.float se debe asignar en todo momento a una variable. Consulte los ejemplos anteriores.
Ver también
input.bool
input.int
input.string
input.text_area
input.symbol
input.timeframe
input.session
input.source
input.color
input.time
input
input.int()2 sobrecargas

Añade una entrada a la pestaña Entradas en la Configuración del script, lo que permite especificar las opciones de configuración para los usuarios del script. Esta función añade un campo para entradas de números enteros a las entradas del script.
Sintaxis y sobrecargas
input.int(defval, title, options, tooltip, inline, group, confirm, display) → input int
input.int(defval, title, minval, maxval, step, tooltip, inline, group, confirm, display) → input int
Argumentos
defval (const int) Establece el valor por defecto de la variable de entrada propuesto en la pestaña Configuración/Entradas del script, donde el usuario puede modificarlo. Cuando se utiliza una lista de valores con el parámetro options, el valor debe corresponder a uno de esta lista.
title (const string) Título de la entrada. Si no se especifica, se utiliza el nombre de la variable como título de la entrada. Si se especifica el título, pero está vacío, el nombre será una cadena vacía.
options (tuple of const int values: [val1, val2, ...]) Lista separada por comas de las opciones del menú desplegable encerradas entre corchetes: [val1, val2, ...]. Los parámetros minval, maxval y step no se pueden utilizar con este parámetro.
tooltip (const string) La cadena que se mostrará al usuario al pasar el cursor por encima del icono de la descripción emergente.
inline (const string) Combina todas las llamadas de entrada utilizando el mismo argumento en una línea. La cadena utilizada como argumento no se muestra. Solo se utiliza para identificar entradas pertenecientes a la misma línea.
group (const string) Crea un encabezado sobre todas las entradas y utiliza la misma cadena del argumento de grupo. La cadena también se utiliza como texto del encabezado.
confirm (const bool) Si es true, se pedirá al usuario que confirme el valor de entrada antes de añadir el indicador al gráfico. El valor por defecto es false.
display (const plot_display) Controla dónde mostrará el script la información de la entrada, aparte de la que contenga la configuración del propio script. Esta opción permite eliminar una entrada específica de la línea de estado del script o de la Ventana de datos para garantizar que solo se muestren allí las entradas más necesarias. Valores posibles: display.none, display.data_window, display.status_line, display.all. Es opcional. El valor por defecto es display.all.
Ejemplo

//@version=5
indicator("input.int", overlay=true)
i_len1 = input.int(10, "Length 1", minval=5, maxval=21, step=1)
plot(ta.sma(close, i_len1))

i_len2 = input.int(10, "Length 2", options=[5, 10, 21])
plot(ta.sma(close, i_len2))
Devoluciones
Valor de la variable de entrada.
Observaciones
El resultado de la función input.int se debe asignar en todo momento a una variable. Consulte los ejemplos anteriores.
Ver también
input.bool
input.float
input.string
input.text_area
input.symbol
input.timeframe
input.session
input.source
input.color
input.time
input
input.price()

Añade entradas de precios a la pestaña de Configuración/Entradas del script. El uso del parámetro confirm = true activa el modo interactivo de entrada en el que se puede seleccionar el precio haciendo clic en el gráfico.
Sintaxis
input.price(defval, title, tooltip, inline, group, confirm, display) → input float
Argumentos
defval (const int/float) Determina el valor por defecto de la variable de entrada propuesta en la pestaña "Configuración/Entradas" del script, donde el usuario puede modificarlo.
title (const string) Título de la entrada. Si no se especifica, se utiliza el nombre de la variable como título de la entrada. Si se especifica el título, pero está vacío, el nombre será una cadena vacía.
tooltip (const string) La cadena que se mostrará al usuario al pasar el cursor por encima del icono de la descripción emergente.
inline (const string) Combina todas las llamadas de entrada utilizando el mismo argumento en una línea. La cadena utilizada como argumento no se muestra. Solo se utiliza para identificar entradas pertenecientes a la misma línea.
group (const string) Crea un encabezado sobre todas las entradas y utiliza la misma cadena del argumento de grupo. La cadena también se utiliza como texto del encabezado.
confirm (const bool) Si es true, se activa el modo de configuración de entrada interactiva y la selección se realiza haciendo clic en el gráfico al añadir un indicador o seleccionando un indicador y arrastrándolo después. Parámetro opcional. El valor por defecto es false.
display (const plot_display) Controla dónde mostrará el script la información de la entrada, aparte de la que contenga la configuración del propio script. Esta opción permite eliminar una entrada específica de la línea de estado del script o de la Ventana de datos para garantizar que solo se muestren allí las entradas más necesarias. Valores posibles: display.none, display.data_window, display.status_line, display.all. Es opcional. El valor por defecto es display.all.
Ejemplo

//@version=5
indicator("input.price", overlay=true)
price1 = input.price(title="Date", defval=42)
plot(price1)

price2 = input.price(54, title="Date")
plot(price2)
Devoluciones
Valor de la variable de entrada.
Observaciones
Cuando se utiliza el modo interactivo, se puede combinar una entrada de tiempo con una de precio cuando ambas llamadas a la función utilizan el mismo argumento para su parámetro inline.
Ver también
input.bool
input.int
input.float
input.string
input.text_area
input.symbol
input.resolution
input.session
input.source
input.color
input
input.session()

Añade un parámetro de entrada en la pestaña Entradas en la Configuración del script, lo que permite proporcionar opciones de configuración a los usuarios del script. Esta función añade dos menús desplegables que permiten al usuario especificar el inicio de una sesión a través del selector específico y devuelve el resultado como una cadena.
Sintaxis
input.session(defval, title, options, tooltip, inline, group, confirm, display) → input string
Argumentos
defval (const string) Establece el valor por defecto de la variable de entrada propuesto en la pestaña Configuración/Entradas del script, desde donde el usuario puede modificarlo. Cuando se utiliza una lista de valores con el parámetro options, el valor debe corresponder a uno de esta lista.
title (const string) Título de la entrada. Si no se especifica, se utiliza el nombre de la variable como título de la entrada. Si se especifica el título, pero está vacío, el nombre será una cadena vacía.
options (tuple of const string values: [val1, val2, ...]) Lista de opciones para elegir.
tooltip (const string) La cadena que se mostrará al usuario al pasar el cursor por encima del icono de la descripción emergente.
inline (const string) Combina todas las llamadas de entrada utilizando el mismo argumento en una línea. La cadena utilizada como argumento no se muestra. Solo se utiliza para identificar entradas pertenecientes a la misma línea.
group (const string) Crea un encabezado sobre todas las entradas y utiliza la misma cadena del argumento de grupo. La cadena también se utiliza como texto del encabezado.
confirm (const bool) Si es true, se pedirá al usuario que confirme el valor de entrada antes de añadir el indicador al gráfico. El valor por defecto es false.
display (const plot_display) Controla dónde mostrará el script la información de la entrada, aparte de la que contenga la configuración del propio script. Esta opción permite eliminar una entrada específica de la línea de estado del script o de la Ventana de datos para garantizar que solo se muestren allí las entradas más necesarias. Valores posibles: display.none, display.data_window, display.status_line, display.all. Es opcional. El valor por defecto es display.all.
Ejemplo

//@version=5
indicator("input.session", overlay=true)
i_sess = input.session("1300-1700", "Session", options=["0930-1600", "1300-1700", "1700-2100"])
t = time(timeframe.period, i_sess)
bgcolor(time == t ? color.green : na)
Devoluciones
Valor de la variable de entrada.
Observaciones
El resultado de la función input.session se debe asignar en todo momento a una variable. Consulte los ejemplos anteriores.
Ver también
input.bool
input.int
input.float
input.string
input.text_area
input.symbol
input.timeframe
input.source
input.color
input.time
input
input.source()

Añade un parámetro de entrada en la pestaña Entradas de la Configuración del script, lo que permite proporcionar opciones de configuración a los usuarios del script. Esta función añade un menú desplegable que permite al usuario seleccionar la fuente del cálculo, por ejemplo, close, hl2, etc.. El usuario también puede seleccionar como fuente un parámetro de salida de cualquier otro indicador en el gráfico.
Sintaxis
input.source(defval, title, tooltip, inline, group, display, confirm) → series float
Argumentos
defval (open/high/low/close/hl2/hlc3/ohlc4/hlcc4) Determina el valor por defecto de la variable de entrada propuesta en la pestaña "Configuración/Entradas" del script, donde el usuario puede modificarlo.
title (const string) Título de la entrada. Si no se especifica, se utiliza el nombre de la variable como título de la entrada. Si se especifica el título, pero está vacío, el nombre será una cadena vacía.
tooltip (const string) La cadena que se mostrará al usuario al pasar el cursor por encima del icono de la descripción emergente.
inline (const string) Combina todas las llamadas de entrada utilizando el mismo argumento en una línea. La cadena utilizada como argumento no se muestra. Solo se utiliza para identificar entradas pertenecientes a la misma línea.
group (const string) Crea un encabezado sobre todas las entradas y utiliza la misma cadena del argumento de grupo. La cadena también se utiliza como texto del encabezado.
display (const plot_display) Controla dónde mostrará el script la información de la entrada, aparte de la que contenga la configuración del propio script. Esta opción permite eliminar una entrada específica de la línea de estado del script o de la Ventana de datos para garantizar que solo se muestren allí las entradas más necesarias. Valores posibles: display.none, display.data_window, display.status_line, display.all. Es opcional. El valor por defecto es display.all.
confirm (const bool) Si es true, se pedirá al usuario que confirme el valor de entrada antes de añadir el indicador al gráfico. El valor por defecto es false.
Ejemplo

//@version=5
indicator("input.source", overlay=true)
i_src = input.source(close, "Source")
plot(i_src)
Devoluciones
Valor de la variable de entrada.
Observaciones
El resultado de la función input.source se debe asignar en todo momento a una variable. Consulte los ejemplos anteriores.
Ver también
input.bool
input.int
input.float
input.string
input.text_area
input.symbol
input.timeframe
input.session
input.color
input.time
input
input.string()

Añade un parámetro de entrada en la pestaña Entradas en la Configuración del script, lo que permite proporcionar opciones de configuración a los usuarios del script. Esta función añade un campo para introducir una cadena a las entradas del script.
Sintaxis
input.string(defval, title, options, tooltip, inline, group, confirm, display) → input string
Argumentos
defval (const string) Establece el valor por defecto de la variable de entrada propuesto en la pestaña Configuración/Entradas del script, desde donde el usuario puede modificarlo. Cuando se utiliza una lista de valores con el parámetro options, el valor debe corresponder a uno de esta lista.
title (const string) Título de la entrada. Si no se especifica, se utiliza el nombre de la variable como título de la entrada. Si se especifica el título, pero está vacío, el nombre será una cadena vacía.
options (tuple of const string values: [val1, val2, ...]) Lista de opciones para elegir.
tooltip (const string) La cadena que se mostrará al usuario al pasar el cursor por encima del icono de la descripción emergente.
inline (const string) Combina todas las llamadas de entrada utilizando el mismo argumento en una línea. La cadena utilizada como argumento no se muestra. Solo se utiliza para identificar entradas pertenecientes a la misma línea.
group (const string) Crea un encabezado sobre todas las entradas y utiliza la misma cadena del argumento de grupo. La cadena también se utiliza como texto del encabezado.
confirm (const bool) Si es true, se pedirá al usuario que confirme el valor de entrada antes de añadir el indicador al gráfico. El valor por defecto es false.
display (const plot_display) Controla dónde mostrará el script la información de la entrada, aparte de la que contenga la configuración del propio script. Esta opción permite eliminar una entrada específica de la línea de estado del script o de la Ventana de datos para garantizar que solo se muestren allí las entradas más necesarias. Valores posibles: display.none, display.data_window, display.status_line, display.all. Es opcional. El valor por defecto es display.all.
Ejemplo

//@version=5
indicator("input.string", overlay=true)
i_text = input.string("Hello!", "Message")
l = label.new(bar_index, high, i_text)
label.delete(l[1])
Devoluciones
Valor de la variable de entrada.
Observaciones
El resultado de la función input.string se debe asignar en todo momento a una variable. Consulte los ejemplos anteriores.
Ver también
input.text_area
input.bool
input.int
input.float
input.symbol
input.timeframe
input.session
input.source
input.color
input.time
input
input.symbol()

Añade un parámetro de entrada en la pestaña Entradas en la Configuración del script, lo que permite proporcionar opciones de configuración a los usuarios del script. Esta función añade un campo que permite al usuario seleccionar un símbolo específico utilizando la búsqueda de símbolos y los devuelve junto con el prefijo del mercado, en forma de cadena.
Sintaxis
input.symbol(defval, title, tooltip, inline, group, confirm, display) → input string
Argumentos
defval (const string) Determina el valor por defecto de la variable de entrada propuesta en la pestaña "Configuración/Entradas" del script, donde el usuario puede modificarlo.
title (const string) Título de la entrada. Si no se especifica, se utiliza el nombre de la variable como título de la entrada. Si se especifica el título, pero está vacío, el nombre será una cadena vacía.
tooltip (const string) La cadena que se mostrará al usuario al pasar el cursor por encima del icono de la descripción emergente.
inline (const string) Combina todas las llamadas de entrada utilizando el mismo argumento en una línea. La cadena utilizada como argumento no se muestra. Solo se utiliza para identificar entradas pertenecientes a la misma línea.
group (const string) Crea un encabezado sobre todas las entradas y utiliza la misma cadena del argumento de grupo. La cadena también se utiliza como texto del encabezado.
confirm (const bool) Si es true, se pedirá al usuario que confirme el valor de entrada antes de añadir el indicador al gráfico. El valor por defecto es false.
display (const plot_display) Controla dónde mostrará el script la información de la entrada, aparte de la que contenga la configuración del propio script. Esta opción permite eliminar una entrada específica de la línea de estado del script o de la Ventana de datos para garantizar que solo se muestren allí las entradas más necesarias. Valores posibles: display.none, display.data_window, display.status_line, display.all. Es opcional. El valor por defecto es display.all.
Ejemplo

//@version=5
indicator("input.symbol", overlay=true)
i_sym = input.symbol("DELL", "Symbol")
s = request.security(i_sym, 'D', close)
plot(s)
Devoluciones
Valor de la variable de entrada.
Observaciones
El resultado de la función input.symbol se debe asignar en todo momento a una variable. Consulte los ejemplos anteriores.
Ver también
input.bool
input.int
input.float
input.string
input.text_area
input.timeframe
input.session
input.source
input.color
input.time
input
input.text_area()

Añade una entrada en la pestaña Entradas en la Configuración del script, lo que permite proporcionar opciones de configuración a los usuarios del script. Esta función añade un campo para introducir texto multilínea.
Sintaxis
input.text_area(defval, title, tooltip, group, confirm, display) → input string
Argumentos
defval (const string) Determina el valor por defecto de la variable de entrada propuesta en la pestaña "Configuración/Entradas" del script, donde el usuario puede modificarlo.
title (const string) Título de la entrada. Si no se especifica, se utiliza el nombre de la variable como título de la entrada. Si se especifica el título, pero está vacío, el nombre será una cadena vacía.
tooltip (const string) La cadena que se mostrará al usuario al pasar el cursor por encima del icono de la descripción emergente.
group (const string) Crea un encabezado sobre todas las entradas y utiliza la misma cadena del argumento de grupo. La cadena también se utiliza como texto del encabezado.
confirm (const bool) Si es true, se pedirá al usuario que confirme el valor de entrada antes de añadir el indicador al gráfico. El valor por defecto es false.
display (const plot_display) Controla dónde mostrará el script la información de la entrada, aparte de la que contenga la configuración del propio script. Esta opción permite eliminar una entrada específica de la línea de estado del script o de la Ventana de datos para garantizar que solo se muestren allí las entradas más necesarias. Valores posibles: display.none, display.data_window, display.status_line, display.all. Es opcional. El valor por defecto es display.none.
Ejemplo

//@version=5
indicator("input.text_area")
i_text = input.text_area(defval = "Hello \nWorld!", title = "Message")
plot(close)
Devoluciones
Valor de la variable de entrada.
Observaciones
El resultado de la función input.text_area se debe asignar en todo momento a una variable. Consulte los ejemplos anteriores.
Ver también
input.string
input.bool
input.int
input.float
input.symbol
input.timeframe
input.session
input.source
input.color
input.time
input
input.time()

Añade la entrada del horario (time input) a la pestaña "Configuración/Entradas" del script. Esta función añade dos widgets de entrada en la misma línea: uno para la fecha y otro para la hora. La función devuelve el valor de la fecha/hora en formato UNIX. Cuando se utiliza confirm = true se activa el modo de entrada interactivo donde se puede seleccionar un momento determinado haciendo clic en el gráfico.
Sintaxis
input.time(defval, title, tooltip, inline, group, confirm, display) → input int
Argumentos
defval (const int) Determina el valor por defecto de la variable de entrada propuesto en la pestaña Configuración/Entradas, desde donde el usuario puede modificarlo. El valor puede ser una función timestamp, pero solo si se utiliza un argumento de fecha en formato de cadena constante.
title (const string) Título de la entrada. Si no se especifica, se utiliza el nombre de la variable como título de la entrada. Si se especifica el título, pero está vacío, el nombre será una cadena vacía.
tooltip (const string) La cadena que se mostrará al usuario al pasar el cursor por encima del icono de la descripción emergente.
inline (const string) Combina todas las llamadas de entrada utilizando el mismo argumento en una línea. La cadena utilizada como argumento no se muestra. Solo se utiliza para identificar entradas pertenecientes a la misma línea.
group (const string) Crea un encabezado sobre todas las entradas y utiliza la misma cadena del argumento de grupo. La cadena también se utiliza como texto del encabezado.
confirm (const bool) Si es true, se activa el modo de configuración de entrada interactiva y la selección se realiza haciendo clic en el gráfico al añadir un indicador o seleccionando un indicador y arrastrándolo después. Parámetro opcional. El valor por defecto es false.
display (const plot_display) Controla dónde mostrará el script la información de la entrada, aparte de la que contenga la configuración del propio script. Esta opción permite eliminar una entrada específica de la línea de estado del script o de la Ventana de datos para garantizar que solo se muestren allí las entradas más necesarias. Valores posibles: display.none, display.data_window, display.status_line, display.all. Es opcional. El valor por defecto es display.none.
Ejemplo

//@version=5
indicator("input.time", overlay=true)
i_date = input.time(timestamp("20 Jul 2021 00:00 +0300"), "Date")
l = label.new(i_date, high, "Date", xloc=xloc.bar_time)
label.delete(l[1])
Devoluciones
Valor de la variable de entrada.
Observaciones
Cuando se utiliza el modo interactivo, se puede combinar una entrada de precio con una de tiempo si ambas llamadas a la función utilizan el mismo argumento para su parámetro inline.
Ver también
input.bool
input.int
input.float
input.string
input.text_area
input.symbol
input.timeframe
input.session
input.source
input.color
input
input.timeframe()

Añade un parámetro de entrada en la pestaña Entradas en la Configuración del script, lo que permite proporcionar opciones de configuración a los usuarios del script. Esta función añade un menú desplegable que permite al usuario seleccionar un intervalo de tiempo específico a través del selector de intervalos y lo devuelve en forma de cadena. El selector incluye los intervalos personalizados que puede haber añadido el usuario a través del menú desplegable de Intervalos de tiempo del gráfico.
Sintaxis
input.timeframe(defval, title, options, tooltip, inline, group, confirm, display) → input string
Argumentos
defval (const string) Establece el valor por defecto de la variable de entrada propuesto en la pestaña Configuración/Entradas del script, desde donde el usuario puede modificarlo. Cuando se utiliza una lista de valores con el parámetro options, el valor debe corresponder a uno de esta lista.
title (const string) Título de la entrada. Si no se especifica, se utiliza el nombre de la variable como título de la entrada. Si se especifica el título, pero está vacío, el nombre será una cadena vacía.
options (tuple of const string values: [val1, val2, ...]) Lista de opciones para elegir.
tooltip (const string) La cadena que se mostrará al usuario al pasar el cursor por encima del icono de la descripción emergente.
inline (const string) Combina todas las llamadas de entrada utilizando el mismo argumento en una línea. La cadena utilizada como argumento no se muestra. Solo se utiliza para identificar entradas pertenecientes a la misma línea.
group (const string) Crea un encabezado sobre todas las entradas y utiliza la misma cadena del argumento de grupo. La cadena también se utiliza como texto del encabezado.
confirm (const bool) Si es true, se pedirá al usuario que confirme el valor de entrada antes de añadir el indicador al gráfico. El valor por defecto es false.
display (const plot_display) Controla dónde mostrará el script la información de la entrada, aparte de la que contenga la configuración del propio script. Esta opción permite eliminar una entrada específica de la línea de estado del script o de la Ventana de datos para garantizar que solo se muestren allí las entradas más necesarias. Valores posibles: display.none, display.data_window, display.status_line, display.all. Es opcional. El valor por defecto es display.all.
Ejemplo

//@version=5
indicator("input.timeframe", overlay=true)
i_res = input.timeframe('D', "Resolution", options=['D', 'W', 'M'])
s = request.security("AAPL", i_res, close)
plot(s)
Devoluciones
Valor de la variable de entrada.
Observaciones
El resultado de la función input.timeframe se debe asignar en todo momento a una variable. Consulte los ejemplos anteriores.
Ver también
input.bool
input.int
input.float
input.string
input.text_area
input.symbol
input.session
input.source
input.color
input.time
input
int()4 sobrecargas

Convierte na o trunca el valor float a int
Sintaxis y sobrecargas
int(x) → const int
int(x) → input int
int(x) → simple int
int(x) → series int
Argumentos
x (const int/float) Valor a convertir al tipo especificado, normalmente na.
Devoluciones
Valor del argumento después de convertirse en int.
Ver también
float
bool
color
string
line
label
label()

Convierte na en label
Sintaxis
label(x) → series label
Argumentos
x (series label) Valor a convertir al tipo especificado, normalmente na.
Devoluciones
Valor del argumento después de convertirse en label.
Ver también
float
int
bool
color
string
line
label.copy()

Clona el objeto label.
Sintaxis
label.copy(id) → series label
Argumentos
id (series label) Objeto label.
Ejemplo

//@version=5
indicator('Last 100 bars highest/lowest', overlay = true)
LOOKBACK = 100
highest = ta.highest(LOOKBACK)
highestBars = ta.highestbars(LOOKBACK)
lowest = ta.lowest(LOOKBACK)
lowestBars = ta.lowestbars(LOOKBACK)
if barstate.islastconfirmedhistory
    var labelHigh = label.new(bar_index + highestBars, highest, str.tostring(highest), color = color.green)
    var labelLow = label.copy(labelHigh)
    label.set_xy(labelLow, bar_index + lowestBars, lowest)
    label.set_text(labelLow, str.tostring(lowest))
    label.set_color(labelLow, color.red)
    label.set_style(labelLow, label.style_label_up)
Devoluciones
Nuevo objeto label ID que puede pasarse a las funciones label.setXXX y label.getXXX.
Ver también
label.new
label.delete
label.delete()

Borra el objeto label especificado. Si ya se ha eliminado, no hace nada.
Sintaxis
label.delete(id) → void
Argumentos
id (series label) Objeto label a eliminar.
Ver también
label.new
label.get_text()

Devuelve el texto de este objeto label.
Sintaxis
label.get_text(id) → series string
Argumentos
id (series label) Objeto label.
Ejemplo

//@version=5
indicator("label.get_text")
my_label = label.new(time, open, text="Open bar text", xloc=xloc.bar_time)
a = label.get_text(my_label)
label.new(time, close, text = a + " new", xloc=xloc.bar_time)
Devoluciones
Objeto string que contiene el texto de esta etiqueta.
Ver también
label.new
label.get_x()

Devuelve el tiempo UNIX o el índice de barras (en función del último valor xloc establecido) de la posición de esta etiqueta.
Sintaxis
label.get_x(id) → series int
Argumentos
id (series label) Objeto label.
Ejemplo

//@version=5
indicator("label.get_x")
my_label = label.new(time, open, text="Open bar text", xloc=xloc.bar_time)
a = label.get_x(my_label)
plot(time - label.get_x(my_label)) //draws zero plot
Devoluciones
UNIX timestamp (en milisegundos) o índice de barras.
Ver también
label.new
label.get_y()

Devuelve el precio de la posición de esta etiqueta.
Sintaxis
label.get_y(id) → series float
Argumentos
id (series label) Objeto label.
Devoluciones
Valor de punto flotante que representa el precio.
Ver también
label.new
label.new()2 sobrecargas

Crea un nuevo objeto label
Sintaxis y sobrecargas
label.new(point, text, xloc, yloc, color, style, textcolor, size, textalign, tooltip, text_font_family, force_overlay) → series label
label.new(x, y, text, xloc, yloc, color, style, textcolor, size, textalign, tooltip, text_font_family, force_overlay) → series label
Argumentos
point (chart.point) Objeto chart.point que especifica la ubicación de la etiqueta.
text (series string) Texto de la etiqueta. El valor por defecto es una cadena vacía.
xloc (series string) Véase la descripción del argumento x. Valores posibles: xloc.bar_index y xloc.bar_time. El valor por defecto es xloc.bar_index.
yloc (series string) Los valores posibles son yloc.price, yloc.abovebar, yloc.belowbar. Si yloc=yloc.price, el argumento y especifica el precio de la posición de la etiqueta. Si yloc=yloc.abovebar, la etiqueta se sitúa por encima de la barra. Si yloc=yloc.belowbar, la etiqueta se sitúa por debajo de la barra. El valor por defecto es yloc.price.
color (series color) Color del borde de la etiqueta y la flecha.
style (series string) Estilo de etiqueta. Valores posibles: label.style_none, label.style_xcross, label.style_cross, label.style_triangleup, label.style_triangledown, label.style_flag, label.style_circle, label.style_arrowup, label.style_arrowdown, label.style_label_up, label.style_label_down, label.style_label_left, label.style_label_right, label.style_label_lower_left, label.style_label_lower_right, label.style_label_upper_left, label.style_label_upper_right, label.style_label_center, label.style_square, label.style_diamond, label.style_text_outline. El valor por defecto es label.style_label_down.
textcolor (series color) Color del texto.
size (series string) Tamaño de la etiqueta. Valores posibles: size.auto, size.tiny, size.small, size.normal, size.large, size.huge. El valor por defecto es size.normal.
textalign (series string) Alineación del texto de la etiqueta. Valores posibles: text.align_left, text.align_center, text.align_right. El valor por defecto es text.align_center.
tooltip (series string) Pase el cursor para ver la etiqueta de la descripción emergente.
text_font_family (series string) Familia de fuentes del texto. Opcional. El valor por defecto es font.family_default. Valores posibles: font.family_default, font.family_monospace.
force_overlay (const bool) Si es true, el dibujo se mostrará en el panel principal del gráfico, incluso cuando el script ocupe un panel separado. Es opcional. Por defecto es false.
Ejemplo

//@version=5
indicator("label.new")
var label1 = label.new(bar_index, low, text="Hello, world!", style=label.style_circle)
label.set_x(label1, 0)
label.set_xloc(label1, time, xloc.bar_time)
label.set_color(label1, color.red)
label.set_size(label1, size.large)
Devoluciones
Objeto ID de etiqueta que se puede pasar a las funciones label.setXXX y label.getXXX.
Ver también
label.delete
label.set_x
label.set_y
label.set_xy
label.set_xloc
label.set_yloc
label.set_color
label.set_textcolor
label.set_style
label.set_size
label.set_textalign
label.set_tooltip
label.set_color()

Establece el borde de la etiqueta y el color de la flecha.
Sintaxis
label.set_color(id, color) → void
Argumentos
id (series label) Objeto label.
color (series color) Nuevo borde de la etiqueta y color de la flecha.
Ver también
label.new
label.set_point()

Establece la ubicación de la etiqueta id a point.
Sintaxis
label.set_point(id, point) → void
Argumentos
id (series label) Objeto label.
point (chart.point) Objeto chart.point.
label.set_size()

Establece la flecha y el tamaño del texto del objeto label especificado.
Sintaxis
label.set_size(id, size) → void
Argumentos
id (series label) Objeto label.
size (series string) Valores posibles: size.auto, size.tiny, size.small, size.normal, size.large, size.huge. El valor por defecto es size.auto.
Ver también
size.auto
size.tiny
size.small
size.normal
size.large
size.huge
label.new
label.set_style()

Establece el estilo de la etiqueta.
Sintaxis
label.set_style(id, style) → void
Argumentos
id (series label) Objeto label.
style (series string) Nuevo estilo de etiqueta. Valores posibles: label.style_none, label.style_xcross, label.style_cross, label.style_triangleup, label.style_triangledown, label.style_flag, label.style_circle, label.style_arrowup, label.style_arrowdown, label.style_label_up, label.style_label_down, label.style_label_left, label.style_label_right, label.style_label_lower_left, label.style_label_lower_right, label.style_label_upper_left, label.style_label_upper_right, label.style_label_center, label.style_square, label.style_diamond, label.style_text_outline.
Ver también
label.new
label.set_text()

Establece el texto de la etiqueta
Sintaxis
label.set_text(id, text) → void
Argumentos
id (series label) Objeto label.
text (series string) Nuevo texto de la etiqueta.
Ver también
label.new
label.set_text_font_family()

Función que establece la familia de fuentes del texto dentro de la etiqueta.
Sintaxis
label.set_text_font_family(id, text_font_family) → void
Argumentos
id (series label) Un objeto label.
text_font_family (series string) Familia de fuentes del texto. Valores posibles: font.family_default, font.family_monospace.
Ejemplo

//@version=5
indicator("Example of setting the label font")
if barstate.islastconfirmedhistory
    l = label.new(bar_index, 0, "monospace", yloc=yloc.abovebar)
    label.set_text_font_family(l, font.family_monospace)
Ver también
label.new
font.family_default
font.family_monospace
label.set_textalign()

Establece la alineación para el texto de la etiqueta.
Sintaxis
label.set_textalign(id, textalign) → void
Argumentos
id (series label) Objeto label.
textalign (series string) Alineación del texto de la etiqueta. Valores posibles: text.align_left, text.align_center, text.align_right.
Ver también
text.align_left
text.align_center
text.align_right
label.new
label.set_textcolor()

Establece el color del texto de la etiqueta.
Sintaxis
label.set_textcolor(id, textcolor) → void
Argumentos
id (series label) Objeto label.
textcolor (series color) Nuevo color del texto.
Ver también
label.new
label.set_tooltip()

Establece el texto de la descripción emergente.
Sintaxis
label.set_tooltip(id, tooltip) → void
Argumentos
id (series label) Objeto label.
tooltip (series string) Texto de la descripción emergente.
Ver también
label.new
label.set_x()

Establece el índice o tiempo de la barra (en función de xloc) de la posición de la etiqueta.
Sintaxis
label.set_x(id, x) → void
Argumentos
id (series label) Objeto label.
x (series int) Nuevo índice de barras o tiempo de barras de la posición de la etiqueta. Tenga en cuenta que los objetos posicionados utilizando xloc.bar_index no se pueden dibujar más allá de 500 barras.
Ver también
label.new
label.set_xloc()

Establece la ubicación x y el nuevo valor del índice/tiempo de la barra.
Sintaxis
label.set_xloc(id, x, xloc) → void
Argumentos
id (series label) Objeto label.
x (series int) Nuevo índice o tiempo de la barra de la posición de la etiqueta.
xloc (series string) Nuevo valor de la ubicación x.
Ver también
xloc.bar_index
xloc.bar_time
label.new
label.set_xy()

Establece el índice/tiempo de la barra y el precio de la posición de la etiqueta.
Sintaxis
label.set_xy(id, x, y) → void
Argumentos
id (series label) Objeto label.
x (series int) Nuevo índice de barras o tiempo de barras de la posición de la etiqueta. Tenga en cuenta que los objetos posicionados utilizando xloc.bar_index no se pueden dibujar más allá de 500 barras.
y (series int/float) Nuevo precio de la posición de la etiqueta.
Ver también
label.new
label.set_y()

Establece el precio de la posición de la etiqueta
Sintaxis
label.set_y(id, y) → void
Argumentos
id (series label) Objeto label.
y (series int/float) Nuevo precio de la posición de la etiqueta.
Ver también
label.new
label.set_yloc()

Establece un nuevo algoritmo de cálculo de la ubicación y.
Sintaxis
label.set_yloc(id, yloc) → void
Argumentos
id (series label) Objeto label.
yloc (series string) Nuevo valor de la ubicación y.
Ver también
yloc.price
yloc.abovebar
yloc.belowbar
label.new
library()

Sentencia de declaración que identifica un script como una biblioteca.
Sintaxis
library(title, overlay, dynamic_requests) → void
Argumentos
title (const string) Título de la biblioteca y su identificador. No puede contener espacios, caracteres especiales o comenzar con un dígito. Se utiliza como título por defecto de la publicación y para identificar de forma exclusiva la biblioteca en la sentencia import, cuando otro script la utiliza. También se utiliza como nombre del script en el gráfico.
overlay (const bool) Si es true, se añadirá la biblioteca sobre el gráfico. Si es false, se incluirá en un panel separado. Parámetro opcional. El valor por defecto es false.
dynamic_requests (const bool) Especifica si el script puede llamar dinámicamente a funciones del espacio de nombres request.*(). Las llamadas dinámicas request.*() están permitidas dentro de los ámbitos locales de estructuras condicionales (por ejemplo, if), bucles (por ejemplo, for) y funciones exportadas. Además, estas llamadas permiten argumentos «en serie» para muchos de sus parámetros. Opcional. Por defecto es false. Para obtener más información consulte la sección Solicitudes dinámicas del Manual de usuario.
Ejemplo

//@version=5
// @description Math library
library("num_methods", overlay = true)
// Calculate "sinh()" from the float parameter `x`
export sinh(float x) =>
    (math.exp(x) - math.exp(-x)) / 2.0
plot(sinh(0))
Ver también
indicator
strategy
line()

Convierte na en line
Sintaxis
line(x) → series line
Argumentos
x (series line) Valor a convertir al tipo especificado, normalmente na.
Devoluciones
Valor del argumento después de convertirse en line.
Ver también
float
int
bool
color
string
label
line.copy()

Clona el objeto line.
Sintaxis
line.copy(id) → series line
Argumentos
id (series line) Objeto line.
Ejemplo

//@version=5
indicator('Last 100 bars price range', overlay = true)
LOOKBACK = 100
highest = ta.highest(LOOKBACK)
lowest = ta.lowest(LOOKBACK)
if barstate.islastconfirmedhistory
    var lineTop = line.new(bar_index[LOOKBACK], highest, bar_index, highest, color = color.green)
    var lineBottom = line.copy(lineTop)
    line.set_y1(lineBottom, lowest)
    line.set_y2(lineBottom, lowest)
    line.set_color(lineBottom, color.red)
Devoluciones
Nuevo objeto line ID, que se puede pasar a las funciones line.setXXX y line.getXXX.
Ver también
line.new
line.delete
line.delete()

Borra el objeto line especificado. Si ya se ha eliminado, no hace nada.
Sintaxis
line.delete(id) → void
Argumentos
id (series line) Objeto line a eliminar.
Ver también
line.new
line.get_price()

Devuelve el nivel del precio de una línea en un índice de barras dado.
Sintaxis
line.get_price(id, x) → series float
Argumentos
id (series line) Objeto line.
x (series int) Índice de barras que precisa un precio.
Ejemplo

//@version=5
indicator("GetPrice", overlay=true)
var line l = na
if bar_index == 10
    l := line.new(0, high[5], bar_index, high)
plot(line.get_price(l, bar_index), color=color.green)
Devoluciones
Valor del precio de la línea 'id' en el índice de barras 'x'.
Observaciones
Se considera que la línea se ha creado usando 'extend=extend.both'.
Esta función solo se puede llamar para líneas creadas mediante 'xloc.bar_index'. Generará un error si intenta llamarla para una línea creada con 'xloc.bar_time'.
Ver también
line.new
line.get_x1()

Devuelve el tiempo UNIX o el índice de barras (en función del último valor xloc establecido) del primer punto de la línea.
Sintaxis
line.get_x1(id) → series int
Argumentos
id (series line) Objeto line.
Ejemplo

//@version=5
indicator("line.get_x1")
my_line = line.new(time, open, time + 60 * 60 * 24, close, xloc=xloc.bar_time)
a = line.get_x1(my_line)
plot(time - line.get_x1(my_line)) //draws zero plot
Devoluciones
UNIX timestamp (en milisegundos) o índice de barras.
Ver también
line.new
line.get_x2()

Devuelve el tiempo UNIX o el índice de barras (en función del último valor xloc establecido) del segundo punto de la línea.
Sintaxis
line.get_x2(id) → series int
Argumentos
id (series line) Objeto line.
Devoluciones
UNIX timestamp (en milisegundos) o índice de barras.
Ver también
line.new
line.get_y1()

Devuelve el precio del primer punto de la línea.
Sintaxis
line.get_y1(id) → series float
Argumentos
id (series line) Objeto line.
Devoluciones
Valor del precio.
Ver también
line.new
line.get_y2()

Devuelve el precio del segundo punto de la línea.
Sintaxis
line.get_y2(id) → series float
Argumentos
id (series line) Objeto line.
Devoluciones
Valor del precio.
Ver también
line.new
line.new()2 sobrecargas

Crea un nuevo objeto line.
Sintaxis y sobrecargas
line.new(first_point, second_point, xloc, extend, color, style, width, force_overlay) → series line
line.new(x1, y1, x2, y2, xloc, extend, color, style, width, force_overlay) → series line
Argumentos
first_point (chart.point) Objeto chart.point que especifica la coordenada inicial de la línea.
second_point (chart.point) Objeto chart.point que especifica la coordenada final de la línea.
xloc (series string) Véase la descripción del argumento x1. Valores posibles: xloc.bar_index y xloc.bar_time. El valor por defecto es xloc.bar_index.
extend (series string) Cuando sea extend=extend.none, se dibuja un segmento que comienza en el punto (x1, y1) y termina en el punto (x2, y2). Si extend es igual a extend.right o extend.left, se dibuja un rayo que comienza en el punto (x1, y1) o en (x2, y2), respectivamente. Si extend=extend.both, se dibuja una línea recta que pasa por estos puntos. El valor por defecto es extend.none.
color (series color) Color de línea.
style (series string) Estilo de línea. Los valores posibles son: line.style_solid, line.style_dotted, line.style_dashed, line.style_arrow_left, line.style_arrow_right, line.style_arrow_both.
width (series int) Ancho de línea en píxeles.
force_overlay (const bool) Si es true, el dibujo se mostrará en el panel principal del gráfico, incluso cuando el script ocupe un panel separado. Es opcional. Por defecto es false.
Ejemplo

//@version=5
indicator("line.new")
var line1 = line.new(0, low, bar_index, high, extend=extend.right)
var line2 = line.new(time, open, time + 60 * 60 * 24, close, xloc=xloc.bar_time, style=line.style_dashed)
line.set_x2(line1, 0)
line.set_xloc(line1, time, time + 60 * 60 * 24, xloc.bar_time)
line.set_color(line2, color.green)
line.set_width(line2, 5)
Devoluciones
Objeto line ID que se puede pasar a las funciones line.setXXX y line.getXXX.
Ver también
line.delete
line.set_x1
line.set_y1
line.set_xy1
line.set_x2
line.set_y2
line.set_xy2
line.set_xloc
line.set_color
line.set_extend
line.set_style
line.set_width
line.set_color()

Establece el color de la línea
Sintaxis
line.set_color(id, color) → void
Argumentos
id (series line) Objeto line.
color (series color) Nuevo color de linea
Ver también
line.new
line.set_extend()

Establece el tipo de extensión de este objeto line. Si extend=extend.none, se dibuja un segmento que empieza en el punto (x1, y1) y acaba en el punto (x2, y2). Si extend es igual a extend.right o extend.left, se dibuja un rayo que empieza en el punto (x1, y1) o en el (x2, y2), respectivamente. Si extend=extend.both, se dibuja una línea recta que atraviesa estos puntos.
Sintaxis
line.set_extend(id, extend) → void
Argumentos
id (series line) Objeto line.
extend (series string) Nuevo tipo de extensión.
Ver también
extend.none
extend.right
extend.left
extend.both
line.new
line.set_first_point()

Establece el primer punto de la línea id en point.
Sintaxis
line.set_first_point(id, point) → void
Argumentos
id (series line) Objeto line.
point (chart.point) Objeto chart.point.
line.set_second_point()

Establece el segundo punto de la línea id en point.
Sintaxis
line.set_second_point(id, point) → void
Argumentos
id (series line) Objeto line.
point (chart.point) Objeto chart.point.
line.set_style()

Establece el estilo de línea
Sintaxis
line.set_style(id, style) → void
Argumentos
id (series line) Objeto line.
style (series string) Nuevo estilo de línea.
Ver también
line.style_solid
line.style_dotted
line.style_dashed
line.style_arrow_left
line.style_arrow_right
line.style_arrow_both
line.new
line.set_width()

Establece el ancho de línea.
Sintaxis
line.set_width(id, width) → void
Argumentos
id (series line) Objeto line.
width (series int) Nuevo ancho de línea en píxeles.
Ver también
line.new
line.set_x1()

Establece el índice o tiempo de la barra (según el xloc) del primer punto.
Sintaxis
line.set_x1(id, x) → void
Argumentos
id (series line) Objeto line.
x (series int) Índice de barra o tiempo. Tenga en cuenta que los objetos posicionados utilizando xloc.bar_index no se pueden dibujar más allá de 500 barras.
Ver también
line.new
line.set_x2()

Establece el índice o tiempo de la barra (según el xloc) del segundo punto.
Sintaxis
line.set_x2(id, x) → void
Argumentos
id (series line) Objeto line.
x (series int) Índice de barra o tiempo. Tenga en cuenta que los objetos posicionados utilizando xloc.bar_index no se pueden dibujar más allá de 500 barras.
Ver también
line.new
line.set_xloc()

Establece la ubicación x y los nuevos valores del indice/tiempo de la barra.
Sintaxis
line.set_xloc(id, x1, x2, xloc) → void
Argumentos
id (series line) Objeto line.
x1 (series int) Índice o tiempo de la barra del primer punto.
x2 (series int) Índice o tiempo de la barra del segundo punto.
xloc (series string) Nuevo valor de la ubicación x.
Ver también
xloc.bar_index
xloc.bar_time
line.new
line.set_xy1()

Establece el índice/tiempo de la barra, así como el precio del primer punto.
Sintaxis
line.set_xy1(id, x, y) → void
Argumentos
id (series line) Objeto line.
x (series int) Índice de barra o tiempo. Tenga en cuenta que los objetos posicionados utilizando xloc.bar_index no se pueden dibujar más allá de 500 barras.
y (series int/float) Precio.
Ver también
line.new
line.set_xy2()

Establece el índice/tiempo de la barra y el precio del segundo punto
Sintaxis
line.set_xy2(id, x, y) → void
Argumentos
id (series line) Objeto line.
x (series int) Índice o tiempo de la barra.
y (series int/float) Precio.
Ver también
line.new
line.set_y1()

Establece el precio del primer punto
Sintaxis
line.set_y1(id, y) → void
Argumentos
id (series line) Objeto line.
y (series int/float) Precio.
Ver también
line.new
line.set_y2()

Establece el precio del segundo punto.
Sintaxis
line.set_y2(id, y) → void
Argumentos
id (series line) Objeto line.
y (series int/float) Precio.
Ver también
line.new
linefill()

Convierte na en linefill.
Sintaxis
linefill(x) → series linefill
Argumentos
x (series linefill) Valor a convertir al tipo especificado, normalmente na.
Devoluciones
Valor del argumento después de convertirse en linefill.
Ver también
float
int
bool
color
string
line
label
linefill.delete()

Elimina el objeto linefill especificado. En el caso de que ya se hubiera eliminado, no realiza ninguna acción.
Sintaxis
linefill.delete(id) → void
Argumentos
id (series linefill) Objeto linefill.
linefill.get_line1()

Devuelve el ID de la primera línea utilizada en el linefill id.
Sintaxis
linefill.get_line1(id) → series line
Argumentos
id (series linefill) Objeto linefill.
linefill.get_line2()

Devuelve el ID de la segunda línea utilizada en el linefill id.
Sintaxis
linefill.get_line2(id) → series line
Argumentos
id (series linefill) Objeto linefill.
linefill.new()

Crea un nuevo objeto linefill y lo muestra en el gráfico, llenando el espacio entre line1 yline2 con el color especificado encolor.
Sintaxis
linefill.new(line1, line2, color) → series linefill
Argumentos
line1 (series line) Primer objeto line.
line2 (series line) Segundo objeto line.
color (series color) Color utilizado para rellenar el espacio entre las líneas.
Devoluciones
ID de un objeto linefill que se puede pasar a otras funciones linefill.*().
Observaciones
Si se elimina alguna de las dos líneas, también se elimina el objeto linefill. Si se mueven las líneas (por ejemplo, mediante funciones line.set_xy), el objeto linefill también se mueve.
Si ambas líneas se extienden en la misma dirección con respecto a las propias líneas (por ejemplo, ambas tienen extend.right como valor de su parámetro extend=), el espacio entre las extensiones de las líneas también se rellenará.
linefill.set_color()

Función que establece el color pasado al objeto linefill.
Sintaxis
linefill.set_color(id, color) → void
Argumentos
id (series linefill) Objeto linefill.
color (series color) Color del objeto linefill.
log.error()2 sobrecargas

Convierte la cadena de formato y el valor o valores en una cadena formateada, y envía el resultado al menú «Registros de Pine» etiquetado con el nivel de depuración «error».
La cadena de formato puede contener texto literal y un marcador de posición entre llaves {} para cada valor que se desee formatear. Cada marcador de posición consiste en el índice del argumento requerido (comenzando en 0) que lo reemplazará y un especificador de formato opcional. El índice representa la posición de ese argumento en la lista de argumentos de la función.
Sintaxis y sobrecargas
log.error(message) → void
log.error(formatString, arg0, arg1, ...) → void
Argumentos
message (series string) Mensaje de registro.
Ejemplo

//@version=5
strategy("My strategy", overlay = true, margin_long = 100, margin_short = 100, process_orders_on_close = true)
bracketTickSizeInput = input.int(1000, "Stoploss/Take-Profit distance (in ticks)")

longCondition = ta.crossover(ta.sma(close, 14), ta.sma(close, 28))
if (longCondition)
    limitLevel = close * 1.01
    log.info("Long limit order has been placed at {0}", limitLevel)
    strategy.order("My Long Entry Id", strategy.long, limit = limitLevel)

    log.info("Exit orders have been placed: Take-profit at {0}, Stop-loss at {1}", close)
    strategy.exit("Exit", "My Long Entry Id", profit = bracketTickSizeInput, loss = bracketTickSizeInput)

if strategy.opentrades > 10
    log.warning("{0} positions opened in the same direction in a row. Try adjusting `bracketTickSizeInput`", strategy.opentrades)

last10Perc = strategy.initial_capital / 10 > strategy.equity
if (last10Perc and not last10Perc[1])
    log.error("The strategy has lost 90% of the initial capital!")
Devoluciones
La cadena formateada.
Observaciones
Los corchetes dentro de un patrón sin comillas deben estar equilibrados. Por ejemplo, "ab {0} de" y "ab '}' de" son patrones válidos, pero "ab {0'}' de", "ab } de" y "''{''" no lo son.
Función que puede aplicar un formato adicional a algunos valores dentro del {}. La lista de opciones de formatos adicionales se puede encontrar en la sección EJEMPLO del artículo str.format.
La cadena utilizada como argumento formatString puede contener caracteres de comillas simples ('). No obstante, todas las comillas simples de esa cadena deben ir emparejadas para evitar resultados de formato inesperados.
Se puede acceder al botón «Registros de Pine...» desde el desplegable «Más» en el Editor de Pine y desde el desplegable «Más» en la línea de estado de cualquier script que utilice funciones log.*().
log.info()2 sobrecargas

Convierte la cadena de formato y el valor o valores en una cadena formateada, además de enviar el resultado al menú «Registros de Pine» etiquetado con el nivel de depuración «info».
La cadena de formato puede contener texto literal y un marcador de posición entre llaves {} para cada valor que se desee formatear. Cada marcador de posición consiste en el índice del argumento requerido (comenzando en 0) que lo reemplazará y un especificador de formato opcional. El índice representa la posición de ese argumento en la lista de argumentos de la función.
Sintaxis y sobrecargas
log.info(message) → void
log.info(formatString, arg0, arg1, ...) → void
Argumentos
message (series string) Mensaje de registro.
Ejemplo

//@version=5
strategy("My strategy", overlay = true, margin_long = 100, margin_short = 100, process_orders_on_close = true)
bracketTickSizeInput = input.int(1000, "Stoploss/Take-Profit distance (in ticks)")

longCondition = ta.crossover(ta.sma(close, 14), ta.sma(close, 28))
if (longCondition)
    limitLevel = close * 1.01
    log.info("Long limit order has been placed at {0}", limitLevel)
    strategy.order("My Long Entry Id", strategy.long, limit = limitLevel)

    log.info("Exit orders have been placed: Take-profit at {0}, Stop-loss at {1}", close)
    strategy.exit("Exit", "My Long Entry Id", profit = bracketTickSizeInput, loss = bracketTickSizeInput)

if strategy.opentrades > 10
    log.warning("{0} positions opened in the same direction in a row. Try adjusting `bracketTickSizeInput`", strategy.opentrades)

last10Perc = strategy.initial_capital / 10 > strategy.equity
if (last10Perc and not last10Perc[1])
    log.error("The strategy has lost 90% of the initial capital!")
Devoluciones
La cadena formateada.
Observaciones
Los corchetes dentro de un patrón sin comillas deben estar equilibrados. Por ejemplo, "ab {0} de" y "ab '}' de" son patrones válidos, pero "ab {0'}' de", "ab } de" y "''{''" no lo son.
Función que puede aplicar un formato adicional a algunos valores dentro del {}. La lista de opciones de formatos adicionales se puede encontrar en la sección EJEMPLO del artículo str.format.
La cadena utilizada como argumento formatString puede contener caracteres de comillas simples ('). No obstante, todas las comillas simples de esa cadena deben ir emparejadas para evitar resultados de formato inesperados.
Se puede acceder al botón «Registros de Pine...» desde el desplegable «Más» en el Editor de Pine y desde el desplegable «Más» en la línea de estado de cualquier script que utilice funciones log.*().
log.warning()2 sobrecargas

Convierte la cadena de formato y el valor o valores en una cadena formateada, y envía el resultado al menú «Registros de Pine» etiquetado con el nivel de depuración «warning».
La cadena de formato puede contener texto literal y un marcador de posición entre llaves {} para cada valor que se desee formatear. Cada marcador de posición consiste en el índice del argumento requerido (comenzando en 0) que lo reemplazará y un especificador de formato opcional. El índice representa la posición de ese argumento en la lista de argumentos de la función.
Sintaxis y sobrecargas
log.warning(message) → void
log.warning(formatString, arg0, arg1, ...) → void
Argumentos
message (series string) Mensaje de registro.
Ejemplo

//@version=5
strategy("My strategy", overlay = true, margin_long = 100, margin_short = 100, process_orders_on_close = true)
bracketTickSizeInput = input.int(1000, "Stoploss/Take-Profit distance (in ticks)")

longCondition = ta.crossover(ta.sma(close, 14), ta.sma(close, 28))
if (longCondition)
    limitLevel = close * 1.01
    log.info("Long limit order has been placed at {0}", limitLevel)
    strategy.order("My Long Entry Id", strategy.long, limit = limitLevel)

    log.info("Exit orders have been placed: Take-profit at {0}, Stop-loss at {1}", close)
    strategy.exit("Exit", "My Long Entry Id", profit = bracketTickSizeInput, loss = bracketTickSizeInput)

if strategy.opentrades > 10
    log.warning("{0} positions opened in the same direction in a row. Try adjusting `bracketTickSizeInput`", strategy.opentrades)

last10Perc = strategy.initial_capital / 10 > strategy.equity
if (last10Perc and not last10Perc[1])
    log.error("The strategy has lost 90% of the initial capital!")
Devoluciones
La cadena formateada.
Observaciones
Los corchetes dentro de un patrón sin comillas deben estar equilibrados. Por ejemplo, "ab {0} de" y "ab '}' de" son patrones válidos, pero "ab {0'}' de", "ab } de" y "''{''" no lo son.
Función que puede aplicar un formato adicional a algunos valores dentro del {}. La lista de opciones de formatos adicionales se puede encontrar en la sección EJEMPLO del artículo str.format.
La cadena utilizada como argumento formatString puede contener caracteres de comillas simples ('). No obstante, todas las comillas simples de esa cadena deben ir emparejadas para evitar resultados de formato inesperados.
Se puede acceder al botón «Registros de Pine...» desde el desplegable «Más» en el Editor de Pine y desde el desplegable «Más» en la línea de estado de cualquier script que utilice funciones log.*().
map.clear()

Borra el map, eliminando todos los pares clave-valor del mismo.
Sintaxis
map.clear(id) → void
Argumentos
id (any map type) Objeto map.
Ejemplo

//@version=5
indicator("map.clear example")
oddMap = map.new<int, bool>()
oddMap.put(1, true)
oddMap.put(2, false)
oddMap.put(3, true)
map.clear(oddMap)
plot(oddMap.size())
Ver también
map.new<type,type>
map.put_all
map.keys
map.values
map.remove
map.contains()

Devuelve true si encuentra key en el map id, de lo contrario false.
Sintaxis
map.contains(id, key) → series bool
Argumentos
id (any map type) Objeto map.
key (series <type of the map's elements>) La clave para buscar en el map.
Ejemplo

//@version=5
indicator("map.includes example")
a = map.new<string, float>()
a.put("open", open)
p = close
if map.contains(a, "open")
    p := a.get("open")
plot(p)
Ver también
map.new<type,type>
map.put
map.keys
map.values
map.size
map.copy()

Crea una copia de un map existente.
Sintaxis
map.copy(id) → map<keyType, valueType>
Argumentos
id (any map type) Objeto map a copiar.
Ejemplo

//@version=5
indicator("map.copy example")
a = map.new<string, int>()
a.put("example", 1)
b = map.copy(a)
a := map.new<string, int>()
a.put("example", 2)
plot(a.get("example"))
plot(b.get("example"))
Devoluciones
Copia del map id.
Ver también
map.new<type,type>
map.put
map.keys
map.values
map.get
map.size
map.get()

Devuelve el valor asociado al key especificado en el map id.
Sintaxis
map.get(id, key) → <value_type>
Argumentos
id (any map type) Objeto map.
key (series <type of the map's elements>) La clave del valor a recuperar.
Ejemplo

//@version=5
indicator("map.get example")
a = map.new<int, int>()
size = 10
for i = 0 to size
    a.put(i, size-i)
plot(map.get(a, 1))
Ver también
map.new<type,type>
map.put
map.keys
map.values
map.contains
map.keys()

Devuelve un array con todas las claves del map id. El array resultante es una copia y cualquier cambio posterior no se reflejará en el map original.
Sintaxis
map.keys(id) → array<type>
Argumentos
id (any map type) Objeto map.
Ejemplo

//@version=5
indicator("map.keys example")
a = map.new<string, float>()
a.put("open", open)
a.put("high", high)
a.put("low", low)
a.put("close", close)
keys = map.keys(a)
ohlc = 0.0
for key in keys
    ohlc += a.get(key)
plot(ohlc/4)
Observaciones
Los map mantienen el orden de inserción. Los elementos dentro del array devuelto por esta función también seguirán el mismo orden de inserción.
Ver también
map.new<type,type>
map.put
map.get
map.values
map.size
map.new<type,type>()

Crea un nuevo objeto map: una colección que consiste en pares clave-valor, donde todas las claves son del tipo keyType y todos los valores son del tipo valueType.
keyType solo puede ser un type primitivo o enum. Por ejemplo: int, float, bool, string, color.
valueType puede ser de cualquier tipo excepto array<>, matrix<> y map<>. Se permiten tipos definidos por el usuario, incluso si incluyen array<>, matrix<> o map<> como uno de sus campos.
Sintaxis
map.new<keyType, valueType>() → map<keyType, valueType>
Ejemplo

//@version=5
indicator("map.new<string, int> example")
a = map.new<string, int>()
a.put("example", 1)
label.new(bar_index, close, str.tostring(a.get("example")))
Devoluciones
ID de un objeto map que puede utilizarse en otras funciones map.*().
Observaciones
Cada clave es única y solo puede aparecer una vez. Si se añade un nuevo valor con una clave ya contenida en el map, este sustituye al antiguo valor asociado a la clave.
Los map mantienen el orden de inserción. Tenga en cuenta que este orden no cambia cuando se inserta un par con un key que ya esté en el map. En estos casos, el nuevo par sustituye al par existente con el key.
Ver también
map.put
map.keys
map.values
map.get
array.new<type>
map.put()

Introduce un nuevo par clave-valor en el map id.
Sintaxis
map.put(id, key, value) → <value_type>
Argumentos
id (any map type) Objeto map.
key (series <type of the map's elements>) La clave a introducir en el map.
value (series <type of the map's elements>) El valor de la clave a introducir en el map.
Ejemplo

//@version=5
indicator("map.put example")
a = map.new<string, float>()
map.put(a, "first", 10)
map.put(a, "second", 15)
prevFirst = map.put(a, "first", 20)
currFirst = a.get("first")
plot(prevFirst)
plot(currFirst)
Devoluciones
El valor anterior asociado a key si la clave ya estaba presente en el map, o na si la clave es nueva.
Observaciones
Los map mantienen el orden de inserción. Tenga en cuenta que este orden no cambia cuando se inserta un par con un key que ya esté en el map. En estos casos, el nuevo par sustituye al par existente con el key.
Ver también
map.new<type,type>
map.put_all
map.keys
map.values
map.remove
map.put_all()

Coloca todos los pares clave-valor del map id2 en el map id.
Sintaxis
map.put_all(id, id2) → void
Argumentos
id (any map type) Un objeto map al que añadir.
id2 (any map type) Un objeto map a añadir.
Ejemplo

//@version=5
indicator("map.put_all example")
a = map.new<string, float>()
b = map.new<string, float>()
a.put("first", 10)
a.put("second", 15)
b.put("third", 20)
map.put_all(a, b)
plot(a.get("third"))
Ver también
map.new<type,type>
map.put
map.keys
map.values
map.remove
map.remove()

Elimina un par clave-valor del map id.
Sintaxis
map.remove(id, key) → <value_type>
Argumentos
id (any map type) Objeto map.
key (series <type of the map's elements>) La clave del par a eliminar del map.
Ejemplo

//@version=5
indicator("map.remove example")
a = map.new<string, color>()
a.put("firstColor", color.green)
oldColorValue = map.remove(a, "firstColor")
plot(close, color = oldColorValue)
Devoluciones
El valor anterior asociado a key si la clave estaba presente en el map, o na si no existía dicha clave.
Ver también
map.new<type,type>
map.put
map.keys
map.values
map.clear
map.size()

Devuelve el número de pares clave-valor en el map id.
Sintaxis
map.size(id) → series int
Argumentos
id (any map type) Objeto map.
Ejemplo

//@version=5
indicator("map.size example")
a = map.new<int, int>()
size = 10
for i = 0 to size
    a.put(i, size-i)
plot(map.size(a))
Ver también
map.new<type,type>
map.put
map.keys
map.values
map.get
map.values()

Devuelve un array con todos los valores del map id. El array resultante es una copia y cualquier cambio posterior no se reflejará en el map original.
Sintaxis
map.values(id) → array<type>
Argumentos
id (any map type) Objeto map.
Ejemplo

//@version=5
indicator("map.values example")
a = map.new<string, float>()
a.put("open", open)
a.put("high", high)
a.put("low", low)
a.put("close", close)
values = map.values(a)
ohlc = 0.0
for value in values
    ohlc += value
plot(ohlc/4)
Observaciones
Los map mantienen el orden de inserción. Los elementos dentro del array devuelto por esta función también seguirán el mismo orden de inserción.
Ver también
map.new<type,type>
map.put
map.get
map.keys
map.size
math.abs()8 sobrecargas

El valor absoluto de number es number si number >= 0, o -number en caso contrario.
Sintaxis y sobrecargas
math.abs(number) → const int
math.abs(number) → input int
math.abs(number) → const float
math.abs(number) → simple int
math.abs(number) → input float
math.abs(number) → series int
math.abs(number) → simple float
math.abs(number) → series float
Argumentos
number (const int) El número que se utilizará en el cálculo.
Devoluciones
Valor absoluto de number.
math.acos()4 sobrecargas

La función acos devuelve el arcocoseno (en radianes) del número, de forma que el cos(acos(y)) = y para y en rango [-1, 1].
Sintaxis y sobrecargas
math.acos(angle) → const float
math.acos(angle) → input float
math.acos(angle) → simple float
math.acos(angle) → series float
Argumentos
angle (const int/float) El valor, en radianes, que se utilizará en el cálculo.
Devoluciones
Arcocoseno de un valor; El ángulo devuelto está en el rango [0, Pi] o na si 'y' está fuera del rango [-1, 1].
math.asin()4 sobrecargas

La función asin devuelve el arcoseno (en radianes) de número, de forma que sin(asin(y)) = y para y en el rango [-1, 1].
Sintaxis y sobrecargas
math.asin(angle) → const float
math.asin(angle) → input float
math.asin(angle) → simple float
math.asin(angle) → series float
Argumentos
angle (const int/float) El valor, en radianes, que se utilizará en el cálculo.
Devoluciones
Arcoseno de un valor; El ángulo devuelto está en el rango [-Pi/2, Pi/2] o na cuando 'y' está fuera del rango [-1, 1].
math.atan()4 sobrecargas

La función atan devuelve la arcotangente (en radianes) del número, de forma que tan(atan(y)) = y para cualquier y.
Sintaxis y sobrecargas
math.atan(angle) → const float
math.atan(angle) → input float
math.atan(angle) → simple float
math.atan(angle) → series float
Argumentos
angle (const int/float) El valor, en radianes, que se utilizará en el cálculo.
Devoluciones
Arcotangente de un valor; el ángulo devuelto está en el rango [-Pi/2, Pi/2].
math.avg()2 sobrecargas

Calcula el promedio de todas las series dadas (por elementos).
Sintaxis y sobrecargas
math.avg(number0, number1, ...) → simple float
math.avg(number0, number1, ...) → series float
Argumentos
number0, number1, ... (simple int/float) Secuencia de números que se utilizará en el cálculo.
Devoluciones
Media.
Ver también
math.sum
ta.cum
ta.sma
math.ceil()4 sobrecargas

La función ceil devuelve el número entero más pequeño (más cercano al infinito negativo) que es mayor o igual que el argumento.
Sintaxis y sobrecargas
math.ceil(number) → const int
math.ceil(number) → input int
math.ceil(number) → simple int
math.ceil(number) → series int
Argumentos
number (const int/float) El número que se utilizará en el cálculo.
Devoluciones
Menor número entero que sea mayor o igual al número dado.
Ver también
math.floor
math.round
math.cos()4 sobrecargas

La función cos devuelve el coseno trigonométrico de un ángulo.
Sintaxis y sobrecargas
math.cos(angle) → const float
math.cos(angle) → input float
math.cos(angle) → simple float
math.cos(angle) → series float
Argumentos
angle (const int/float) Ángulo, en radianes.
Devoluciones
Coseno trigonométrico de un ángulo.
math.exp()4 sobrecargas

La función exp de number es e elevado a la potencia de number, donde e es el número de Euler.
Sintaxis y sobrecargas
math.exp(number) → const float
math.exp(number) → input float
math.exp(number) → simple float
math.exp(number) → series float
Argumentos
number (const int/float) El número que se utilizará en el cálculo.
Devoluciones
Valor que representa e elevado a la potencia de number.
Ver también
math.pow
math.floor()4 sobrecargas

Sintaxis y sobrecargas
math.floor(number) → const int
math.floor(number) → input int
math.floor(number) → simple int
math.floor(number) → series int
Argumentos
number (const int/float) El número que se utilizará en el cálculo.
Devoluciones
Mayor número entero que sea menor o igual al número dado.
Ver también
math.ceil
math.round
math.log()4 sobrecargas

El logaritmo natural de cualquier number > 0 es el y único de forma que e^y = number.
Sintaxis y sobrecargas
math.log(number) → const float
math.log(number) → input float
math.log(number) → simple float
math.log(number) → series float
Argumentos
number (const int/float) El número que se utilizará en el cálculo.
Devoluciones
Logaritmo natural de number.
Ver también
math.log10
math.log10()4 sobrecargas

El logaritmo común (o de base 10) de number es la potencia a la que hay que elevar 10 para obtener el number. 10^y = number.
Sintaxis y sobrecargas
math.log10(number) → const float
math.log10(number) → input float
math.log10(number) → simple float
math.log10(number) → series float
Argumentos
number (const int/float) El número que se utilizará en el cálculo.
Devoluciones
Logaritmo en base 10 de number.
Ver también
math.log
math.max()8 sobrecargas

Devuelve el mayor de múltiples valores.
Sintaxis y sobrecargas
math.max(number0, number1, ...) → const int
math.max(number0, number1, ...) → const float
math.max(number0, number1, ...) → input int
math.max(number0, number1, ...) → simple int
math.max(number0, number1, ...) → input float
math.max(number0, number1, ...) → series int
math.max(number0, number1, ...) → simple float
math.max(number0, number1, ...) → series float
Argumentos
number0, number1, ... (const int) Secuencia de números que se utilizará en el cálculo.
Ejemplo

//@version=5
indicator("math.max", overlay=true)
plot(math.max(close, open))
plot(math.max(close, math.max(open, 42)))
Devoluciones
El mayor de los múltiples valores dados.
Ver también
math.min
math.min()8 sobrecargas

Devuelve el menor de múltiples valores.
Sintaxis y sobrecargas
math.min(number0, number1, ...) → const int
math.min(number0, number1, ...) → const float
math.min(number0, number1, ...) → input int
math.min(number0, number1, ...) → simple int
math.min(number0, number1, ...) → input float
math.min(number0, number1, ...) → series int
math.min(number0, number1, ...) → simple float
math.min(number0, number1, ...) → series float
Argumentos
number0, number1, ... (const int) Secuencia de números que se utilizará en el cálculo.
Ejemplo

//@version=5
indicator("math.min", overlay=true)
plot(math.min(close, open))
plot(math.min(close, math.min(open, 42)))
Devoluciones
El menor de varios valores dados.
Ver también
math.max
math.pow()4 sobrecargas

Función potencial matemática.
Sintaxis y sobrecargas
math.pow(base, exponent) → const float
math.pow(base, exponent) → input float
math.pow(base, exponent) → simple float
math.pow(base, exponent) → series float
Argumentos
base (const int/float) Especifique la base a utilizar.
exponent (const int/float) Especifica el exponente.
Ejemplo

//@version=5
indicator("math.pow", overlay=true)
plot(math.pow(close, 2))
Devoluciones
base elevada a la potencia de exponent. Si base es una serie se calcula por elementos.
Ver también
math.sqrt
math.exp
math.random()

Devuelve un valor pseudoaleatorio. La función generará una secuencia diferente de valores para cada ejecución del script. Cuando se utiliza el mismo valor para el argumento opcional seed, se genera una secuencia repetible.
Sintaxis
math.random(min, max, seed) → series float
Argumentos
min (series int/float) Límite inferior del rango de valores aleatorios. El valor no está incluido en el rango. El valor por defecto es 0.
max (series int/float) Límite superior del rango de valores aleatorios. El valor no está incluido en el rango. El valor por defecto es 1.
seed (series int) Argumento opcional. Cuando se utiliza el mismo seed, permite realizar llamadas sucesivas a la función para que se genere una secuencia repetible de valores.
Devoluciones
Valor aleatorio.
math.round()8 sobrecargas

Devuelve el valor del number redondeado al número entero más cercano, con media redondeada hacia arriba. Si se utiliza el parámetro precision, devuelve un valor float redondeado según el número de decimales especificado.
Sintaxis y sobrecargas
math.round(number) → const int
math.round(number) → input int
math.round(number) → simple int
math.round(number) → series int
math.round(number, precision) → const float
math.round(number, precision) → input float
math.round(number, precision) → simple float
math.round(number, precision) → series float
Argumentos
number (const int/float) El valor a redondear.
Devoluciones
El valor de number se redondea al número entero más cercano o conforme a la precisión.
Observaciones
Tenga en cuenta que para los valores "na" la función devuelve "na".
Ver también
math.ceil
math.floor
math.round_to_mintick()2 sobrecargas

Devuelve el valor redondeado al mintick del símbolo, es decir, al valor más cercano que se puede dividir entre syminfo.mintick, sin el resto, con media redondeada hacia arriba.
Sintaxis y sobrecargas
math.round_to_mintick(number) → simple float
math.round_to_mintick(number) → series float
Argumentos
number (simple int/float) El valor a redondear.
Devoluciones
number redondeado a la precisión del tick.
Observaciones
Tenga en cuenta que para los valores "na" la función devuelve "na".
Ver también
math.ceil
math.floor
math.sign()4 sobrecargas

El signo (signum) de number es cero si number es cero, 1.0 si number es mayor que cero, -1.0 si number es menor que cero.
Sintaxis y sobrecargas
math.sign(number) → const float
math.sign(number) → input float
math.sign(number) → simple float
math.sign(number) → series float
Argumentos
number (const int/float) El número que se utilizará en el cálculo.
Devoluciones
Signo del argumento.
math.sin()4 sobrecargas

La función sin devuelve el seno trigonométrico de un ángulo.
Sintaxis y sobrecargas
math.sin(angle) → const float
math.sin(angle) → input float
math.sin(angle) → simple float
math.sin(angle) → series float
Argumentos
angle (const int/float) Ángulo, en radianes.
Devoluciones
Seno trigonométrico de un ángulo.
math.sqrt()4 sobrecargas

La raíz cuadrada de cualquier number >= 0 es el y >= 0 único de forma que y^2 = number.
Sintaxis y sobrecargas
math.sqrt(number) → const float
math.sqrt(number) → input float
math.sqrt(number) → simple float
math.sqrt(number) → series float
Argumentos
number (const int/float) El número que se utilizará en el cálculo.
Devoluciones
Raíz cuadrada de number.
Ver también
math.pow
math.sum()

La función sum devuelve la suma deslizante de los últimos valores y de x.
Sintaxis
math.sum(source, length) → series float
Argumentos
source (series int/float) Serie de valores a procesar.
length (series int) Número de barras (longitud)
Devoluciones
Suma de source respecto a length barras históricas.
Observaciones
Se ignoran los valores na de la serie source; la función calcula sobre la cantidad length de valores no na.
Ver también
ta.cum
for
math.tan()4 sobrecargas

La función tan devuelve la tangente trigonométrica de un ángulo.
Sintaxis y sobrecargas
math.tan(angle) → const float
math.tan(angle) → input float
math.tan(angle) → simple float
math.tan(angle) → series float
Argumentos
angle (const int/float) Ángulo, en radianes.
Devoluciones
Tangente trigonométrica de un ángulo.
math.todegrees()

Devuelve un ángulo prácticamente equivalente en grados de un ángulo medido en radianes.
Sintaxis
math.todegrees(radians) → series float
Argumentos
radians (series int/float) Ángulo en radianes.
Devoluciones
Valor del ángulo en grados.
math.toradians()

Devuelve un ángulo prácticamente equivalente en radianes de un ángulo medido en grados.
Sintaxis
math.toradians(degrees) → series float
Argumentos
degrees (series int/float) Ángulo en grados.
Devoluciones
Valor de un ángulo en radianes.
matrix.add_col()2 sobrecargas

Función que añade una columna en el índice column de la matriz id. La columna puede consistir de valores na, aunque también se puede utilizar una matriz para proporcionar valores.
Sintaxis y sobrecargas
matrix.add_col(id, column) → void
matrix.add_col(id, column, array_id) → void
Argumentos
id (any matrix type) Objeto matriz.
column (series int) Índice de la columna una vez añadida la nueva columna. Opcional. El valor por defecto es matrix.columns.
Añadir una columna a la matriz
Ejemplo

//@version=5
indicator("`matrix.add_col()` Example 1")

// Create a 2x3 "int" matrix containing values `0`.
m = matrix.new<int>(2, 3, 0)

// Add a column  with `na` values to the matrix.
matrix.add_col(m)

// Display matrix elements.
if barstate.islastconfirmedhistory
    var t = table.new(position.top_right, 2, 2, color.green)
    table.cell(t, 0, 0, "Matrix elements:")
    table.cell(t, 0, 1, str.tostring(m))
Añadir un array como columna a la matriz
Ejemplo

//@version=5
indicator("`matrix.add_col()` Example 2")

if barstate.islastconfirmedhistory
    // Create an empty matrix object. 
    var m = matrix.new<int>()
    
    // Create an array with values `1` and `3`.
    var a = array.from(1, 3)
    
    // Add the `a` array as the first column of the empty matrix.
    matrix.add_col(m, 0, a)
    
    // Display matrix elements.
    var t = table.new(position.top_right, 2, 2, color.green)
    table.cell(t, 0, 0, "Matrix elements:")
    table.cell(t, 0, 1, str.tostring(m))
Observaciones
En lugar de añadir columnas a una matriz vacía, resulta mucho más eficaz declarar una matriz con dimensiones explícitas y llenarla de valores. Añadir una columna es también mucho más lento que añadir una fila con la función matrix.add_row.
Ver también
matrix.new<type>
matrix.get
matrix.set
matrix.columns
matrix.rows
matrix.add_row
matrix.add_row()2 sobrecargas

Función que añade una fila en el índice row de la matriz id. La fila puede consistir en valores na, aunque también se puede utilizar una matriz para proporcionar valores.
Sintaxis y sobrecargas
matrix.add_row(id, row) → void
matrix.add_row(id, row, array_id) → void
Argumentos
id (any matrix type) Objeto matriz.
row (series int) Índice de la fila una vez añadida la nueva fila. Opcional. El valor por defecto es matrix.rows.
Añadir fila a la matriz
Ejemplo

//@version=5
indicator("`matrix.add_row()` Example 1")

// Create a 2x3 "int" matrix containing values `0`.
m = matrix.new<int>(2, 3, 0)

// Add a row with `na` values to the matrix.
matrix.add_row(m)

// Display matrix elements.
if barstate.islastconfirmedhistory
    var t = table.new(position.top_right, 2, 2, color.green)
    table.cell(t, 0, 0, "Matrix elements:")
    table.cell(t, 0, 1, str.tostring(m))
Añadir un array como fila a la matriz
Ejemplo

//@version=5
indicator("`matrix.add_row()` Example 2")

if barstate.islastconfirmedhistory
    // Create an empty matrix object. 
    var m = matrix.new<int>()
    
    // Create an array with values `1` and `2`.
    var a = array.from(1, 2)
    
    // Add the `a` array as the first row of the empty matrix.
    matrix.add_row(m, 0, a)
    
    // Display matrix elements.
    var t = table.new(position.top_right, 2, 2, color.green)
    table.cell(t, 0, 0, "Matrix elements:")
    table.cell(t, 0, 1, str.tostring(m))
Observaciones
La indexación de las filas y columnas comienza a partir de cero. En lugar de añadir filas a una matriz vacía, resulta mucho más eficaz declarar una matriz con dimensiones explícitas y rellenarla de valores.
Ver también
matrix.new<type>
matrix.get
matrix.set
matrix.columns
matrix.rows
matrix.add_col
matrix.avg()2 sobrecargas

La función calcula la media de todos los elementos de la matriz.
Sintaxis y sobrecargas
matrix.avg(id) → series float
matrix.avg(id) → series int
Argumentos
id (matrix<int/float>) Objeto matriz.
Ejemplo

//@version=5
indicator("`matrix.avg()` Example")

// Create a 2x2 matrix.
var m = matrix.new<int>(2, 2, na)
// Fill the matrix with values.
matrix.set(m, 0, 0, 1)
matrix.set(m, 0, 1, 2)
matrix.set(m, 1, 0, 3)
matrix.set(m, 1, 1, 4)

// Get the average value of the matrix.
var x = matrix.avg(m)

plot(x, 'Matrix average value')
Devoluciones
Valor medio de la matriz id.
Ver también
matrix.new<type>
matrix.get
matrix.set
matrix.columns
matrix.rows
matrix.col()

Función que crea un array unidimensional a partir de los elementos de una columna de la matriz.
Sintaxis
matrix.col(id, column) → array<type>
Argumentos
id (any matrix type) Objeto matriz.
column (series int) Índice de la columna requerida.
Ejemplo

//@version=5
indicator("`matrix.col()` Example", "", true)

// Create a 2x3 "float" matrix from `hlc3` values.
m = matrix.new<float>(2, 3, hlc3)

// Return an array with the values of the first column of matrix `m`.
a = matrix.col(m, 0)

// Plot the first value from the array `a`.
plot(array.get(a, 0))
Devoluciones
Array ID que contiene los valores column de la matriz id.
Observaciones
La indexación de las filas comienza en 0.
Ver también
matrix.new<type>
matrix.get
array.get
matrix.col
matrix.columns
matrix.columns()

Función que devuelve el número de columnas de la matriz.
Sintaxis
matrix.columns(id) → series int
Argumentos
id (any matrix type) Objeto matriz.
Ejemplo

//@version=5
indicator("`matrix.columns()` Example")

// Create a 2x6 matrix with values `0`.
var m = matrix.new<int>(2, 6, 0)

// Get the quantity of columns in matrix `m`.
var x = matrix.columns(m)

// Display using a label.
if barstate.islastconfirmedhistory
    label.new(bar_index, high, "Columns: " + str.tostring(x) + "\n" + str.tostring(m))
Devoluciones
Número de columnas de la matriz id.
Ver también
matrix.new<type>
matrix.get
matrix.set
matrix.col
matrix.row
matrix.rows
matrix.concat()

Función que añade la matriz m2 a la matriz m1.
Sintaxis
matrix.concat(id1, id2) → matrix<type>
Argumentos
id1 (any matrix type) Objeto de la matriz a concatenar.
id2 (any matrix type) Objeto de la matriz cuyos elementos se añadirán a id1.
Ejemplo

//@version=5
indicator("`matrix.concat()` Example")

// Create a 2x4 "int" matrix containing values `0`.
m1 = matrix.new<int>(2, 4, 0)
// Create a 2x4 "int" matrix containing values `1`.
m2 = matrix.new<int>(2, 4, 1)

// Append matrix `m2` to `m1`.
matrix.concat(m1, m2)

// Display matrix elements.
if barstate.islastconfirmedhistory
    var t = table.new(position.top_right, 2, 2, color.green)
    table.cell(t, 0, 0, "Matrix Elements:")
    table.cell(t, 0, 1, str.tostring(m1))
Devoluciones
Devuelve la matriz id1 concatenada con la matriz id2.
Observaciones
El número de columnas de ambas matrices debe ser idéntico.
Ver también
matrix.new<type>
matrix.get
matrix.set
matrix.columns
matrix.rows
matrix.copy()

Función que crea una nueva matriz que es una copia de la original.
Sintaxis
matrix.copy(id) → matrix<type>
Argumentos
id (any matrix type) Objeto de matriz a copiar.
Ejemplo

//@version=5
indicator("`matrix.copy()` Example")

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Create a 2x3 "float" matrix with `1` values.
    var m1 = matrix.new<float>(2, 3, 1)
    
    // Copy the matrix to a new one.
    // Note that unlike what `matrix.copy()` does, 
    // the simple assignment operation `m2 = m1`
    // would NOT create a new copy of the `m1` matrix.
    // It would merely create a copy of its ID referencing the same matrix.
    var m2 = matrix.copy(m1)
    
    // Display using a table.
    var t = table.new(position.top_right, 5, 2, color.green)
    table.cell(t, 0, 0, "Original Matrix:")
    table.cell(t, 0, 1, str.tostring(m1))
    table.cell(t, 1, 0, "Matrix Copy:")
    table.cell(t, 1, 1, str.tostring(m2))
Devoluciones
Nuevo objeto de matriz de id copiada.
Ver también
matrix.new<type>
matrix.get
matrix.set
matrix.columns
matrix.rows
matrix.det()2 sobrecargas

Función que devuelve el determinante de una matriz cuadrada.
Sintaxis y sobrecargas
matrix.det(id) → series float
matrix.det(id) → series int
Argumentos
id (matrix<int/float>) Objeto matriz.
Ejemplo

//@version=5
indicator("`matrix.det` Example")

// Create a 2x2 matrix.
var m = matrix.new<float>(2, 2, na)
// Fill the matrix with values.
matrix.set(m, 0, 0,  3)
matrix.set(m, 0, 1,  7)
matrix.set(m, 1, 0,  1)
matrix.set(m, 1, 1, -4)

// Get the determinant of the matrix. 
var x = matrix.det(m)

plot(x, 'Matrix determinant')
Devoluciones
Valor determinante de la matriz id.
Observaciones
Cálculo de funciones basado en el algoritmo de descomposición LU.
Ver también
matrix.new<type>
matrix.set
matrix.is_square
matrix.diff()2 sobrecargas

La función devuelve una nueva matriz resultante de la resta entre las matrices id1 y id2, o de la matriz id1 y un escalar id2 (un valor numérico).
Sintaxis y sobrecargas
matrix.diff(id1, id2) → matrix<int>
matrix.diff(id1, id2) → matrix<float>
Argumentos
id1 (matrix<int>) Matriz de la que hay que restar.
id2 (series int/float/matrix<int>) Objeto de la matriz o valor escalar a restar.
Diferencia entre dos matrices
Ejemplo

//@version=5
indicator("`matrix.diff()` Example 1")

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Create a 2x3 matrix containing values `5`.
    var m1 = matrix.new<float>(2, 3, 5) 
    // Create a 2x3 matrix containing values `4`.
    var m2 = matrix.new<float>(2, 3, 4) 
    // Create a new matrix containing the difference between matrices `m1` and `m2`.
    var m3 = matrix.diff(m1, m2) 
    
    // Display using a table.
    var t = table.new(position.top_right, 1, 2, color.green)
    table.cell(t, 0, 0, "Difference between two matrices:")
    table.cell(t, 0, 1, str.tostring(m3))
Diferencia entre una matriz y un valor escalar
Ejemplo

//@version=5
indicator("`matrix.diff()` Example 2")

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Create a 2x3 matrix with values `4`.
    var m1 = matrix.new<float>(2, 3, 4)
    
    // Create a new matrix containing the difference between the `m1` matrix and the "int" value `1`.
    var m2 = matrix.diff(m1, 1)
    
    // Display using a table.
    var t = table.new(position.top_right, 1, 2, color.green)
    table.cell(t,  0, 0, "Difference between a matrix and a scalar:")
    table.cell(t,  0, 1, str.tostring(m2))
Devoluciones
Nuevo objeto de matriz que contiene la diferencia entre id2 y id1.
Ver también
matrix.new<type>
matrix.get
matrix.set
matrix.columns
matrix.rows
matrix.eigenvalues()2 sobrecargas

Función que devuelve una matriz que contiene los eigenvalores de una matriz cuadrada.
Sintaxis y sobrecargas
matrix.eigenvalues(id) → array<float>
matrix.eigenvalues(id) → array<int>
Argumentos
id (matrix<int/float>) Objeto matriz.
Ejemplo

//@version=5
indicator("`matrix.eigenvalues()` Example")

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Create a 2x2 matrix. 
    var m1 = matrix.new<int>(2, 2, na)
    // Fill the matrix with values.
    matrix.set(m1, 0, 0, 2)
    matrix.set(m1, 0, 1, 4)
    matrix.set(m1, 1, 0, 6)
    matrix.set(m1, 1, 1, 8)
    
    // Get the eigenvalues of the matrix.
    tr = matrix.eigenvalues(m1)
    
    // Display matrix elements.
    var t = table.new(position.top_right, 2, 2, color.green)
    table.cell(t, 0, 0, "Matrix elements:")
    table.cell(t, 0, 1, str.tostring(m1))
    table.cell(t, 1, 0, "Array of Eigenvalues:")
    table.cell(t, 1, 1, str.tostring(tr)) 
Devoluciones
Matriz que contiene los valores propios de la matriz id.
Observaciones
La función se calcula mediante el "The Implicit QL Algorithm" (algoritmo QL implícito).
Ver también
matrix.new<type>
matrix.set
matrix.eigenvectors
matrix.eigenvectors()2 sobrecargas

Devuelve una matriz de eigenvectores, en la que cada columna es un vector propio de la matriz id.
Sintaxis y sobrecargas
matrix.eigenvectors(id) → matrix<float>
matrix.eigenvectors(id) → matrix<int>
Argumentos
id (matrix<int/float>) Objeto matriz.
Ejemplo

//@version=5
indicator("`matrix.eigenvectors()` Example")

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Create a 2x2 matrix 
    var m1 = matrix.new<int>(2, 2, 1)
    // Fill the matrix with values.
    matrix.set(m1, 0, 0, 2)
    matrix.set(m1, 0, 1, 4)
    matrix.set(m1, 1, 0, 6)
    matrix.set(m1, 1, 1, 8)
    
    // Get the eigenvectors of the matrix.
    m2 = matrix.eigenvectors(m1)
    
    // Display matrix elements.
    var t = table.new(position.top_right, 2, 2, color.green)
    table.cell(t, 0, 0, "Matrix Elements:")
    table.cell(t, 0, 1, str.tostring(m1))
    table.cell(t, 1, 0, "Matrix Eigenvectors:")
    table.cell(t, 1, 1, str.tostring(m2))  
Devoluciones
Nueva matriz que contiene los vectores propios de la matriz id.
Observaciones
La función se calcula mediante el "The Implicit QL Algorithm" (algoritmo QL implícito).
Ver también
matrix.new<type>
matrix.get
matrix.set
matrix.eigenvalues
matrix.elements_count()

Función que devuelve el número total de todos los elementos de la matriz.
Sintaxis
matrix.elements_count(id) → series int
Argumentos
id (any matrix type) Objeto matriz.
Ver también
matrix.new<type>
matrix.columns
matrix.rows
matrix.fill()

Función que rellena un área rectangular de la matriz id definida por los índices from_column a to_column (sin incluir) y from_row a to_row (sin incluir) con value.
Sintaxis
matrix.fill(id, value, from_row, to_row, from_column, to_column) → void
Argumentos
id (any matrix type) Objeto matriz.
value (series <type of the matrix's elements>) Valor con el que se rellena
from_row (series int) Índice de la fila a partir del cual comenzará el fill (inclusive). Opcional. El valor por defecto es 0.
to_row (series int) Índice de la fila donde finalizará el fill (no es inclusivo). Opcional. El valor por defecto es matrix.rows.
from_column (series int) Índice de la columna a partir de la cual comenzará a rellenarse (inclusive). Opcional. El valor por defecto es 0.
to_column (series int) Índice de la columna donde finalizará el fill (no inclusivo). Opcional. El valor por defecto es matrix.columns.
Ejemplo

//@version=5
indicator("`matrix.fill()` Example")

// Create a 4x5 "int" matrix containing values `0`.
m = matrix.new<float>(4, 5, 0)

// Fill the intersection of rows 1 to 2 and columns 2 to 3 of the matrix with `hl2` values.
matrix.fill(m, hl2, 0, 2, 1, 3)

// Display using a label.
if barstate.islastconfirmedhistory
    label.new(bar_index, high, str.tostring(m))
Ver también
matrix.new<type>
matrix.get
matrix.set
matrix.columns
matrix.rows
matrix.get()

Función que devuelve el elemento con el índice especificado de la matriz.
Sintaxis
matrix.get(id, row, column) → <matrix_type>
Argumentos
id (any matrix type) Objeto matriz.
row (series int) Índice de la fila requerida.
column (series int) Índice de la columna requerida.
Ejemplo

//@version=5
indicator("`matrix.get()` Example", "", true)

// Create a 2x3 "float" matrix from the `hl2` values.
m = matrix.new<float>(2, 3, hl2)

// Return the value of the element at index [0, 0] of matrix `m`.
x = matrix.get(m, 0, 0)

plot(x)
Devoluciones
Valor del elemento en el índice row y column de la matriz id.
Observaciones
La indexación de las filas y columnas empieza a partir de cero.
Ver también
matrix.new<type>
matrix.set
matrix.columns
matrix.rows
matrix.inv()2 sobrecargas

Función que devuelve el inverso de una matriz cuadrada.
Sintaxis y sobrecargas
matrix.inv(id) → matrix<float>
matrix.inv(id) → matrix<int>
Argumentos
id (matrix<int/float>) Objeto matriz.
Ejemplo

//@version=5
indicator("`matrix.inv()` Example")

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Create a 2x2 matrix. 
    var m1 = matrix.new<int>(2, 2, na)
    // Fill the matrix with values.
    matrix.set(m1, 0, 0, 1)
    matrix.set(m1, 0, 1, 2)
    matrix.set(m1, 1, 0, 3)
    matrix.set(m1, 1, 1, 4)
    
    // Inverse of the matrix.
    var m2 = matrix.inv(m1)
    
    // Display matrix elements.
    var t = table.new(position.top_right, 2, 2, color.green)
    table.cell(t, 0, 0, "Original Matrix:")
    table.cell(t, 0, 1, str.tostring(m1))
    table.cell(t, 1, 0, "Inverse matrix:")
    table.cell(t, 1, 1, str.tostring(m2))
Devoluciones
Nueva matriz, inversa a la matriz id.
Observaciones
Función que se calcula mediante el algoritmo de descomposición LU.
Ver también
matrix.new<type>
matrix.set
matrix.pinv
matrix.copy
str.tostring
matrix.is_antidiagonal()

Función que determina si la matriz es antidiagonal (todos los elementos fuera de la diagonal secundaria son cero).
Sintaxis
matrix.is_antidiagonal(id) → series bool
Argumentos
id (matrix<int/float>) Objeto de la matriz a comprobar.
Devoluciones
Devuelve true si la matriz id es antidiagonal, false en caso contrario.
Observaciones
Devuelve false con matrices no cuadradas.
Ver también
matrix.new<type>
matrix.set
matrix.is_square
matrix.is_identity
matrix.is_diagonal
matrix.is_antisymmetric()

Función que determina si una matriz es antisimétrica (su transpuesta es igual a su negativo).
Sintaxis
matrix.is_antisymmetric(id) → series bool
Argumentos
id (matrix<int/float>) Objeto de la matriz a comprobar.
Devoluciones
Devuelve true, si la matriz id es antisimétrica, false en caso contrario.
Observaciones
Devuelve false con matrices no cuadradas.
Ver también
matrix.new<type>
matrix.get
matrix.set
matrix.is_square
matrix.is_binary()

Función que determina si la matriz es binaria (cuando todos los elementos de la matriz son 0 o 1).
Sintaxis
matrix.is_binary(id) → series bool
Argumentos
id (matrix<int/float>) Objeto de la matriz a comprobar.
Devoluciones
Devuelve true si la matriz id es binaria, false en caso contrario.
Ver también
matrix.new<type>
matrix.get
matrix.set
matrix.is_diagonal()

Función que determina si la matriz es diagonal (todos los elementos fuera de la diagonal principal son cero).
Sintaxis
matrix.is_diagonal(id) → series bool
Argumentos
id (matrix<int/float>) Objeto de la matriz a comprobar.
Devoluciones
Devuelve true si la matriz id es diagonal, false en caso contrario.
Observaciones
Devuelve false con matrices no cuadradas.
Ver también
matrix.new<type>
matrix.set
matrix.is_square
matrix.is_identity
matrix.is_antidiagonal
matrix.is_identity()

Función que determina si la matriz es una matriz de identidad (elementos con unos en la diagonal principal y ceros en el resto).
Sintaxis
matrix.is_identity(id) → series bool
Argumentos
id (matrix<int/float>) Objeto de la matriz a comprobar.
Devoluciones
Devuelve true si id es una matriz de identidad, false en caso contrario.
Observaciones
Devuelve false con matrices no cuadradas.
Ver también
matrix.new<type>
matrix.is_square
matrix.is_diagonal
matrix.is_square()

Función que determina si la matriz es cuadrada (tiene el mismo número de filas y columnas).
Sintaxis
matrix.is_square(id) → series bool
Argumentos
id (any matrix type) Objeto de la matriz a comprobar.
Devoluciones
Devuelve true si la matriz id es cuadrada, false en caso contrario.
Ver también
matrix.new<type>
matrix.get
matrix.set
matrix.columns
matrix.rows
matrix.is_stochastic()

Función que determina si la matriz es estocástica.
Sintaxis
matrix.is_stochastic(id) → series bool
Argumentos
id (matrix<int/float>) Objeto de la matriz a comprobar.
Devoluciones
Devuelve true si la matriz id es estocástica, false en caso contrario.
Ver también
matrix.new<type>
matrix.set
matrix.is_symmetric()

Función que determina si una matriz cuadrada es simétrica (los elementos son simétricos respecto a la diagonal principal).
Sintaxis
matrix.is_symmetric(id) → series bool
Argumentos
id (matrix<int/float>) Objeto de la matriz a comprobar.
Devoluciones
Devuelve true si la matriz id es simétrica, false en caso contrario.
Observaciones
Devuelve false con matrices no cuadradas.
Ver también
matrix.new<type>
matrix.get
matrix.set
matrix.is_square
matrix.is_triangular()

Función que determina si la matriz es triangular (si todos los elementos por encima o por debajo de la diagonal principal son cero).
Sintaxis
matrix.is_triangular(id) → series bool
Argumentos
id (matrix<int/float>) Objeto de la matriz a comprobar.
Devoluciones
Devuelve true si la matriz id es triangular, false en caso contrario.
Observaciones
Devuelve false con matrices no cuadradas.
Ver también
matrix.new<type>
matrix.set
matrix.is_square
matrix.is_zero()

Función que determina si todos los elementos de la matriz son cero.
Sintaxis
matrix.is_zero(id) → series bool
Argumentos
id (matrix<int/float>) Objeto de la matriz a comprobar.
Devoluciones
Devuelve true si todos los elementos de la matriz id son cero, false en caso contrario.
Ver también
matrix.new<type>
matrix.get
matrix.set
matrix.kron()2 sobrecargas

Función que devuelve el producto Kronecker para las matrices id1 y id2.
Sintaxis y sobrecargas
matrix.kron(id1, id2) → matrix<float>
matrix.kron(id1, id2) → matrix<int>
Argumentos
id1 (matrix<int/float>) Primer objeto de la matriz.
id2 (matrix<int/float>) Segundo objeto de la matriz.
Ejemplo

//@version=5
indicator("`matrix.kron()` Example")

// Display using a table.
if barstate.islastconfirmedhistory
    // Create two matrices with default values `1` and `2`. 
    var m1 = matrix.new<float>(2, 2, 1) 
    var m2 = matrix.new<float>(2, 2, 2) 
    
    // Calculate the Kronecker product of the matrices.
    var m3 = matrix.kron(m1, m2) 
    
    // Display matrix elements.
    var t = table.new(position.top_right, 5, 2, color.green)
    table.cell(t, 0, 0, "Matrix 1:")
    table.cell(t, 0, 1, str.tostring(m1))
    table.cell(t, 1, 1, "⊗")
    table.cell(t, 2, 0, "Matrix 2:")
    table.cell(t, 2, 1, str.tostring(m2))
    table.cell(t, 3, 1, "=")
    table.cell(t, 4, 0, "Kronecker product:")
    table.cell(t, 4, 1, str.tostring(m3))
Devoluciones
Nueva matriz que contiene el producto Kronecker de id1 y id2.
Ver también
matrix.new<type>
matrix.mult
str.tostring
table.new
matrix.max()2 sobrecargas

Función que devuelve el mayor valor de los elementos de la matriz.
Sintaxis y sobrecargas
matrix.max(id) → series float
matrix.max(id) → series int
Argumentos
id (matrix<int/float>) Objeto matriz.
Ejemplo

//@version=5
indicator("`matrix.max()` Example")

// Create a 2x2 matrix.
var m = matrix.new<int>(2, 2, na)
// Fill the matrix with values.
matrix.set(m, 0, 0, 1)
matrix.set(m, 0, 1, 2)
matrix.set(m, 1, 0, 3)
matrix.set(m, 1, 1, 4)

// Get the maximum value in the matrix.
var x = matrix.max(m)

plot(x, 'Matrix maximum value')
Devoluciones
Valor máximo de la matriz id.
Ver también
matrix.new<type>
matrix.min
matrix.avg
matrix.sort
matrix.median()2 sobrecargas

Función que calcula la mediana (el valor "medio") de los elementos de la matriz.
Sintaxis y sobrecargas
matrix.median(id) → series float
matrix.median(id) → series int
Argumentos
id (matrix<int/float>) Objeto matriz.
Ejemplo

//@version=5
indicator("`matrix.median()` Example")

// Create a 2x2 matrix.
m = matrix.new<int>(2, 2, na)
// Fill the matrix with values.
matrix.set(m, 0, 0, 1)
matrix.set(m, 0, 1, 2)
matrix.set(m, 1, 0, 3)
matrix.set(m, 1, 1, 4)

// Get the median of the matrix.
x = matrix.median(m)

plot(x, 'Median of the matrix')
Observaciones
Tenga en cuenta que los elementos na de la matriz no se consideran a la hora de calcular la mediana.
Ver también
matrix.new<type>
matrix.mode
matrix.sort
matrix.avg
matrix.min()2 sobrecargas

Función que devuelve el valor más pequeño de los elementos de la matriz.
Sintaxis y sobrecargas
matrix.min(id) → series float
matrix.min(id) → series int
Argumentos
id (matrix<int/float>) Objeto matriz.
Ejemplo

//@version=5
indicator("`matrix.min()` Example")

// Create a 2x2 matrix.
var m = matrix.new<int>(2, 2, na)
// Fill the matrix with values.
matrix.set(m, 0, 0, 1)
matrix.set(m, 0, 1, 2)
matrix.set(m, 1, 0, 3)
matrix.set(m, 1, 1, 4)

// Get the minimum value from the matrix.
var x = matrix.min(m)

plot(x, 'Matrix minimum value')
Devoluciones
Valor más pequeño de la matriz id.
Ver también
matrix.new<type>
matrix.max
matrix.avg
matrix.sort
matrix.mode()2 sobrecargas

Función que calcula el modo de la matriz, siendo éste el valor más frecuente de los elementos de la matriz. Cuando hay múltiples valores que aparecen con la misma frecuencia, la función devuelve el menor de los valores.
Sintaxis y sobrecargas
matrix.mode(id) → series float
matrix.mode(id) → series int
Argumentos
id (matrix<int/float>) Objeto matriz.
Ejemplo

//@version=5
indicator("`matrix.mode()` Example")

// Create a 2x2 matrix.
var m = matrix.new<int>(2, 2, na)
// Fill the matrix with values.
matrix.set(m, 0, 0, 0)
matrix.set(m, 0, 1, 0)
matrix.set(m, 1, 0, 1)
matrix.set(m, 1, 1, 1)

// Get the mode of the matrix.
var x = matrix.mode(m)

plot(x, 'Mode of the matrix')
Devoluciones
Valor más frecuente de la matriz id. Si no existe ninguno, devuelve en su lugar el valor más pequeño.
Observaciones
Tenga en cuenta que los elementos na de la matriz no se consideran a la hora de calcular el modo.
Ver también
matrix.new<type>
matrix.set
matrix.median
matrix.sort
matrix.avg
matrix.mult()4 sobrecargas

La función devuelve una nueva matriz resultante del producto entre las matrices id1 y id2, o entre una matriz id1 y un escalar id2 (un valor numérico), o entre una matriz id1 y un vector id2 (un array de valores).
Sintaxis y sobrecargas
matrix.mult(id1, id2) → array<int>
matrix.mult(id1, id2) → array<float>
matrix.mult(id1, id2) → matrix<int>
matrix.mult(id1, id2) → matrix<float>
Argumentos
id1 (matrix<int>) Primer objeto de la matriz.
id2 (array<int>) Segundo objeto de la matriz, valor o array
Producto de dos matrices
Ejemplo

//@version=5
indicator("`matrix.mult()` Example 1")

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Create a 6x2 matrix containing values `5`.
    var m1 = matrix.new<float>(6, 2, 5) 
    // Create a 2x3 matrix containing values `4`.
    // Note that it must have the same quantity of rows as there are columns in the first matrix.
    var m2 = matrix.new<float>(2, 3, 4) 
    // Create a new matrix from the multiplication of the two matrices.
    var m3 = matrix.mult(m1, m2) 
    
    // Display using a table.
    var t = table.new(position.top_right, 1, 2, color.green)
    table.cell(t, 0, 0, "Product of two matrices:")
    table.cell(t, 0, 1, str.tostring(m3))
Producto de una matriz y un escalar
Ejemplo

//@version=5
indicator("`matrix.mult()` Example 2")

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Create a 2x3 matrix containing values `4`.
    var m1 = matrix.new<float>(2, 3, 4) 
    
    // Create a new matrix from the product of the two matrices.
    scalar = 5
    var m2 = matrix.mult(m1, scalar) 
    
    // Display using a table.
    var t = table.new(position.top_right, 5, 2, color.green)
    table.cell(t, 0, 0, "Matrix 1:")
    table.cell(t, 0, 1, str.tostring(m1))
    table.cell(t, 1, 1, "x")
    table.cell(t, 2, 0, "Scalar:")
    table.cell(t, 2, 1, str.tostring(scalar))
    table.cell(t, 3, 1, "=")
    table.cell(t, 4, 0, "Matrix 2:")
    table.cell(t, 4, 1, str.tostring(m2))
Producto de una matriz y un vector del array
Ejemplo

//@version=5
indicator("`matrix.mult()` Example 3")

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Create a 2x3 matrix containing values `4`.
    var m1 = matrix.new<int>(2, 3, 4)
    
    // Create an array of three elements.
    var int[] a = array.from(1, 1, 1)
    
    // Create a new matrix containing the product of the `m1` matrix and the `a` array.
    var m3 = matrix.mult(m1, a) 
    
    // Display using a table.
    var t = table.new(position.top_right, 5, 2, color.green)
    table.cell(t, 0, 0, "Matrix 1:")
    table.cell(t, 0, 1, str.tostring(m1))
    table.cell(t, 1, 1, "x")
    table.cell(t, 2, 0, "Value:")
    table.cell(t, 2, 1, str.tostring(a, " "))
    table.cell(t, 3, 1, "=")
    table.cell(t, 4, 0, "Matrix 3:")
    table.cell(t, 4, 1, str.tostring(m3))
Devoluciones
Nuevo objeto de matriz que contiene el producto de id2 y id1.
Ver también
matrix.new<type>
matrix.sum
matrix.diff
matrix.new<type>()

Función que crea un nuevo objeto matriz. Una matriz es una estructura de datos bidimensional que contiene filas y columnas. Todos los elementos de la matriz deben ser del tipo especificado en la plantilla de tipos ("<type>").
Sintaxis
matrix.new<type>(rows, columns, initial_value) → matrix<type>
Argumentos
rows (series int) Recuento inicial de filas de la matriz. Opcional. El valor por defecto es 0.
columns (series int) Recuento inicial de columnas de la matriz. Opcional. El valor por defecto es 0.
initial_value (<matrix_type>) Valor inicial de todos los elementos de la matriz. Opcional. El valor por defecto es 'na'.
Crear una matriz de elementos con el mismo valor inicial
Ejemplo

//@version=5
indicator("`matrix.new<type>()` Example 1")

// Create a 2x3 (2 rows x 3 columns) "int" matrix with values zero.
var m = matrix.new<int>(2, 3, 0)

// Display using a label.
if barstate.islastconfirmedhistory
    label.new(bar_index, high, str.tostring(m))
Crear una matriz a partir de los valores del array
Ejemplo

//@version=5
indicator("`matrix.new<type>()` Example 2")

// Function to create a matrix whose rows are filled with array values.
matrixFromArray(int rows, int columns, array<float> data) =>
    m = matrix.new<float>(rows, columns)
    for i = 0 to rows <= 0 ? na : rows - 1
        for j = 0 to columns <= 0 ? na : columns - 1
            matrix.set(m, i, j, array.get(data, i * columns + j))
    m
    
// Create a 3x3 matrix from an array of values.
var m1 = matrixFromArray(3, 3, array.from(1, 2, 3, 4, 5, 6, 7, 8, 9))
// Display using a label.
if barstate.islastconfirmedhistory
    label.new(bar_index, high, str.tostring(m1))
Crear una matriz a partir del campoinput.text_area().
Ejemplo

//@version=5
indicator("`matrix.new<type>()` Example 3")

// Function to create a matrix from a text string.
// Values in a row must be separated by a space. Each line is one row.
matrixFromInputArea(stringOfValues) =>
    var rowsArray = str.split(stringOfValues, "\n")
    var rows = array.size(rowsArray)
    var cols = array.size(str.split(array.get(rowsArray, 0), " "))
    var matrix = matrix.new<float>(rows, cols, na) 
    row = 0
    for rowString in rowsArray
        col = 0
        values = str.split(rowString, " ")
        for val in values
            matrix.set(matrix, row, col, str.tonumber(val))
            col += 1
        row += 1
    matrix


stringInput = input.text_area("1 2 3\n4 5 6\n7 8 9")
var m = matrixFromInputArea(stringInput)    

// Display using a label.
if barstate.islastconfirmedhistory
    label.new(bar_index, high, str.tostring(m))
Crear una matriz a partir de valores aleatorios
Ejemplo

//@version=5
indicator("`matrix.new<type>()` Example 4")

// Function to create a matrix with random values (0.0 to 1.0).
matrixRandom(int rows, int columns)=>
    result = matrix.new<float>(rows, columns)
    for i = 0 to rows - 1
        for j = 0 to columns - 1
            matrix.set(result, i, j, math.random())
    result

// Create a 2x3 matrix with random values.
var m = matrixRandom(2, 3)

// Display using a label.
if barstate.islastconfirmedhistory
    label.new(bar_index, high, str.tostring(m))
Devoluciones
ID del nuevo objeto de la matriz.
Ver también
matrix.set
matrix.fill
matrix.columns
matrix.rows
array.new<type>
matrix.pinv()2 sobrecargas

Función que devuelve el pseudoinverso de una matriz.
Sintaxis y sobrecargas
matrix.pinv(id) → matrix<float>
matrix.pinv(id) → matrix<int>
Argumentos
id (matrix<int/float>) Objeto matriz.
Ejemplo

//@version=5
indicator("`matrix.pinv()` Example")

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Create a 2x2 matrix. 
    var m1 = matrix.new<int>(2, 2, na)
    // Fill the matrix with values.
    matrix.set(m1, 0, 0, 1)
    matrix.set(m1, 0, 1, 2)
    matrix.set(m1, 1, 0, 3)
    matrix.set(m1, 1, 1, 4)
    
    // Pseudoinverse of the matrix.
    var m2 = matrix.pinv(m1)
    
    // Display matrix elements.
    var t = table.new(position.top_right, 2, 2, color.green)
    table.cell(t, 0, 0, "Original Matrix:")
    table.cell(t, 0, 1, str.tostring(m1))
    table.cell(t, 1, 0, "Pseudoinverse matrix:")
    table.cell(t, 1, 1, str.tostring(m2))
Devoluciones
Nueva matriz que contiene el pseudoinverso de la matriz id.
Observaciones
La función se calcula utilizando la fórmula inversa Moore-Penrose basada en el desglose de valores singulares de una matriz. Para las matrices cuadradas no singulares, la función devuelve el resultado de matrix.inv.
Ver también
matrix.new<type>
matrix.set
matrix.inv
matrix.pow()2 sobrecargas

La función calcula el producto de la matriz por sí misma power veces.
Sintaxis y sobrecargas
matrix.pow(id, power) → matrix<float>
matrix.pow(id, power) → matrix<int>
Argumentos
id (matrix<int/float>) Objeto matriz.
power (series int) Número de veces que se multiplicará por sí misma la matriz.
Ejemplo

//@version=5
indicator("`matrix.pow()` Example")

// Display using a table.
if barstate.islastconfirmedhistory
    // Create a 2x2 matrix. 
    var m1 = matrix.new<int>(2, 2, 2)
    // Calculate the power of three of the matrix.
    var m2 = matrix.pow(m1, 3)
    
    // Display matrix elements.
    var t = table.new(position.top_right, 2, 2, color.green)
    table.cell(t, 0, 0, "Original Matrix:")
    table.cell(t, 0, 1, str.tostring(m1))
    table.cell(t, 1, 0, "Matrix³:")
    table.cell(t, 1, 1, str.tostring(m2))
Devoluciones
El producto de la matriz id por sí misma power veces.
Ver también
matrix.new<type>
matrix.set
matrix.mult
matrix.rank()

Función que calcula el rango de la matriz.
Sintaxis
matrix.rank(id) → series int
Argumentos
id (any matrix type) Objeto matriz.
Ejemplo

//@version=5
indicator("`matrix.rank()` Example")

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Create a 2x2 matrix. 
    var m1 = matrix.new<int>(2, 2, na)
    // Fill the matrix with values.
    matrix.set(m1, 0, 0, 1)
    matrix.set(m1, 0, 1, 2)
    matrix.set(m1, 1, 0, 3)
    matrix.set(m1, 1, 1, 4)
    
    // Get the rank of the matrix. 
    r = matrix.rank(m1)
    
    // Display matrix elements.
    var t = table.new(position.top_right, 2, 2, color.green)
    table.cell(t, 0, 0, "Matrix elements:")
    table.cell(t, 0, 1, str.tostring(m1))
    table.cell(t, 1, 0, "Rank of the matrix:")
    table.cell(t, 1, 1, str.tostring(r)) 
Devoluciones
Rango de la matriz id
Ver también
matrix.new<type>
matrix.set
str.tostring
matrix.remove_col()

La función elimina la columna en el índice column de la matriz id y devuelve un array que contiene los valores de la columna eliminada.
Sintaxis
matrix.remove_col(id, column) → array<type>
Argumentos
id (any matrix type) Objeto matriz.
column (series int) Índice de la columna a eliminar. Opcional. El valor por defecto es matrix.columns.
Ejemplo

//@version=5
indicator("matrix_remove_col", overlay = true)

// Create a 2x2 matrix with ones.
var matrixOrig = matrix.new<int>(2, 2, 1)

// Set values to the 'matrixOrig' matrix.
matrix.set(matrixOrig, 0, 1, 2)
matrix.set(matrixOrig, 1, 0, 3)
matrix.set(matrixOrig, 1, 1, 4)


// Create a copy of the 'matrixOrig' matrix.
matrixCopy = matrix.copy(matrixOrig)

// Remove the first column from the `matrixCopy` matrix.
arr = matrix.remove_col(matrixCopy, 0)

// Display matrix elements.
if barstate.islastconfirmedhistory
    var t = table.new(position.top_right, 3, 2, color.green)
    table.cell(t, 0, 0, "Original Matrix:")
    table.cell(t, 0, 1, str.tostring(matrixOrig))
    table.cell(t, 1, 0, "Removed Elements:")
    table.cell(t, 1, 1, str.tostring(arr))
    table.cell(t, 2, 0, "Result Matrix:")
    table.cell(t, 2, 1, str.tostring(matrixCopy))
Devoluciones
Array que contiene los elementos de la columna eliminada de la matriz id.
Observaciones
La indexación de las filas y columnas parten de cero. Resulta mucho más eficaz declarar matrices con dimensiones explícitas que crearlas añadiendo o eliminando columnas. Eliminar una columna también es mucho más lento que eliminar una fila mediante la función matrix.remove_row.
Ver también
matrix.new<type>
matrix.set
matrix.copy
matrix.remove_row
matrix.remove_row()

La función elimina la fila en el índice row de la matriz id y devuelve un array que contiene los valores de la fila eliminada.
Sintaxis
matrix.remove_row(id, row) → array<type>
Argumentos
id (any matrix type) Objeto matriz.
row (series int) Índice de la fila a eliminar. Opcional. El valor por defecto es matrix.rows.
Ejemplo

//@version=5
indicator("matrix_remove_row", overlay = true)

// Create a 2x2 "int" matrix containing values `1`.
var matrixOrig = matrix.new<int>(2, 2, 1)

// Set values to the 'matrixOrig' matrix.
matrix.set(matrixOrig, 0, 1, 2)
matrix.set(matrixOrig, 1, 0, 3)
matrix.set(matrixOrig, 1, 1, 4)

// Create a copy of the 'matrixOrig' matrix.
matrixCopy = matrix.copy(matrixOrig)

// Remove the first row from the matrix `matrixCopy`.
arr = matrix.remove_row(matrixCopy, 0)

// Display matrix elements.
if barstate.islastconfirmedhistory
    var t = table.new(position.top_right, 3, 2, color.green)
    table.cell(t, 0, 0, "Original Matrix:")
    table.cell(t, 0, 1, str.tostring(matrixOrig))
    table.cell(t, 1, 0, "Removed Elements:")
    table.cell(t, 1, 1, str.tostring(arr))
    table.cell(t, 2, 0, "Result Matrix:")
    table.cell(t, 2, 1, str.tostring(matrixCopy))
Devoluciones
Array que contiene los elementos de la fila eliminada de la matriz id.
Observaciones
La indexación de las filas y columnas comienzan a partir de cero. Resulta mucho más eficaz declarar matrices con dimensiones explícitas que crearlas añadiendo o eliminando filas.
Ver también
matrix.new<type>
matrix.set
matrix.copy
matrix.remove_col
matrix.reshape()

Función que reconstruye la matriz id a las dimensiones rows x cols.
Sintaxis
matrix.reshape(id, rows, columns) → void
Argumentos
id (any matrix type) Objeto matriz.
rows (series int) Número de filas de la matriz reestructurada.
columns (series int) Número de columnas de la matriz reestructurada.
Ejemplo

//@version=5
indicator("`matrix.reshape()` Example")

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Create a 2x3 matrix.
    var m1 = matrix.new<float>(2, 3)
    // Fill the matrix with values.
    matrix.set(m1, 0, 0, 1)
    matrix.set(m1, 0, 1, 2)
    matrix.set(m1, 0, 2, 3)
    matrix.set(m1, 1, 0, 4)
    matrix.set(m1, 1, 1, 5)
    matrix.set(m1, 1, 2, 6)
    
    // Copy the matrix to a new one.
    var m2 = matrix.copy(m1)
    
    // Reshape the copy to a 3x2.
    matrix.reshape(m2, 3, 2)
    
    // Display using a table.
    var t = table.new(position.top_right, 2, 2, color.green)
    table.cell(t, 0, 0, "Original matrix:")
    table.cell(t, 0, 1, str.tostring(m1))
    table.cell(t, 1, 0, "Reshaped matrix:")
    table.cell(t, 1, 1, str.tostring(m2))
Ver también
matrix.new<type>
matrix.get
matrix.set
matrix.add_row
matrix.add_col
matrix.reverse()

Función que invierte el orden de las filas y columnas de la matriz id. La primera fila y la primera columna se convierten en las últimas y las últimas en las primeras.
Sintaxis
matrix.reverse(id) → void
Argumentos
id (any matrix type) Objeto matriz.
Ejemplo

//@version=5
indicator("`matrix.reverse()` Example")

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Copy the matrix to a new one.
    var m1 = matrix.new<int>(2, 2, na)
    // Fill the matrix with values.
    matrix.set(m1, 0, 0, 1)
    matrix.set(m1, 0, 1, 2)
    matrix.set(m1, 1, 0, 3)
    matrix.set(m1, 1, 1, 4)
    
    // Copy matrix elements to a new matrix.
    var m2 = matrix.copy(m1)
    
    // Reverse the `m2` copy of the original matrix. 
    matrix.reverse(m2)
    
    // Display using a table.
    var t = table.new(position.top_right, 2, 2, color.green)
    table.cell(t, 0, 0, "Original matrix:")
    table.cell(t, 0, 1, str.tostring(m1))
    table.cell(t, 1, 0, "Reversed matrix:")
    table.cell(t, 1, 1, str.tostring(m2))
Ver también
matrix.new<type>
matrix.set
matrix.columns
matrix.rows
matrix.reshape
matrix.row()

Función que crea un array unidimensional a partir de los elementos de una fila de la matriz.
Sintaxis
matrix.row(id, row) → array<type>
Argumentos
id (any matrix type) Objeto matriz.
row (series int) Índice de la fila requerida.
Ejemplo

//@version=5
indicator("`matrix.row()` Example", "", true)

// Create a 2x3 "float" matrix from `hlc3` values.
m = matrix.new<float>(2, 3, hlc3)

// Return an array with the values of the first row of the matrix.
a = matrix.row(m, 0)

// Plot the first value from the array `a`.
plot(array.get(a, 0))
Devoluciones
Array ID que contiene los valores row de la matriz id.
Observaciones
La indexación de las filas comienza en 0.
Ver también
matrix.new<type>
matrix.get
array.get
matrix.col
matrix.rows
matrix.rows()

Función que devuelve el número de filas de la matriz.
Sintaxis
matrix.rows(id) → series int
Argumentos
id (any matrix type) Objeto matriz.
Ejemplo

//@version=5
indicator("`matrix.rows()` Example")

// Create a 2x6 matrix with values `0`.
var m = matrix.new<int>(2, 6, 0)

// Get the quantity of rows in the matrix.
var x = matrix.rows(m)

// Display using a label.
if barstate.islastconfirmedhistory
    label.new(bar_index, high, "Rows: " + str.tostring(x) + "\n" + str.tostring(m))
Devoluciones
Número de filas de la matriz id.
Ver también
matrix.new<type>
matrix.get
matrix.set
matrix.columns
matrix.row
matrix.set()

Función que asigna un value al elemento en la row y la column de la matriz id.
Sintaxis
matrix.set(id, row, column, value) → void
Argumentos
id (any matrix type) Objeto matriz.
row (series int) Índice de la fila del elemento a modificar.
column (series int) Índice de la columna del elemento a modificar.
value (series <type of the matrix's elements>) Nuevo valor a establecer.
Ejemplo

//@version=5
indicator("`matrix.set()` Example")

// Create a 2x3 "int" matrix containing values `4`.
m = matrix.new<int>(2, 3, 4)

// Replace the value of element at row 1 and column 2 with value `3`.
matrix.set(m, 0, 1, 3)

// Display using a label.
if barstate.islastconfirmedhistory
    label.new(bar_index, high, str.tostring(m))
Ver también
matrix.new<type>
matrix.get
matrix.columns
matrix.rows
matrix.sort()

Función que reordena las filas de la matriz id siguiendo el orden de los valores de la column.
Sintaxis
matrix.sort(id, column, order) → void
Argumentos
id (matrix<int/float/string>) Objeto de matriz a clasificar.
column (series int) Índice de la columna cuyos valores ordenados determinan el nuevo orden de las filas. Opcional. El valor por defecto es 0.
order (series sort_order) Orden de clasificación. Valores posibles: order.ascending (por defecto), order.descending.
Ejemplo

//@version=5
indicator("`matrix.sort()` Example")

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Create a 2x2 matrix. 
    var m1 = matrix.new<float>(2, 2, na)
    // Fill the matrix with values.
    matrix.set(m1, 0, 0, 3)
    matrix.set(m1, 0, 1, 4)
    matrix.set(m1, 1, 0, 1)
    matrix.set(m1, 1, 1, 2)
    
    // Copy the matrix to a new one.
    var m2 = matrix.copy(m1)
    // Sort the rows of `m2` using the default arguments (first column and ascending order).
    matrix.sort(m2)
    
    // Display using a table.
    if barstate.islastconfirmedhistory
        var t = table.new(position.top_right, 2, 2, color.green)
        table.cell(t, 0, 0, "Original matrix:")
        table.cell(t, 0, 1, str.tostring(m1))
        table.cell(t, 1, 0, "Sorted matrix:")
        table.cell(t, 1, 1, str.tostring(m2))
Ver también
matrix.new<type>
matrix.max
matrix.min
matrix.avg
matrix.submatrix()

Función que extrae una submatriz de la matriz id dentro de los índices especificados.
Sintaxis
matrix.submatrix(id, from_row, to_row, from_column, to_column) → matrix<type>
Argumentos
id (any matrix type) Objeto matriz.
from_row (series int) Índice de la fila desde donde comenzará la extracción (inclusive). Opcional. El valor por defecto es 0.
to_row (series int) Índice de la fila donde finalizará la extracción (no inclusivo). Opcional. El valor por defecto es matrix.rows.
from_column (series int) Índice de la columna desde donde se iniciará la extracción (inclusive). Opcional. El valor por defecto es 0.
to_column (series int) Índice de la columna donde finalizará la extracción (no inclusivo). Opcional. El valor por defecto es matrix.columns.
Ejemplo

//@version=5
indicator("`matrix.submatrix()` Example")

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Create a 2x3 matrix matrix with values `0`.
    var m1 = matrix.new<int>(2, 3, 0)
    // Fill the matrix with values.
    matrix.set(m1, 0, 0, 1)
    matrix.set(m1, 0, 1, 2)
    matrix.set(m1, 0, 2, 3)
    matrix.set(m1, 1, 0, 4)
    matrix.set(m1, 1, 1, 5)
    matrix.set(m1, 1, 2, 6)
    
    // Create a 2x2 submatrix of the `m1` matrix.
    var m2 = matrix.submatrix(m1, 0, 2, 1, 3)
    
    // Display using a table.
    var t = table.new(position.top_right, 2, 2, color.green)
    table.cell(t, 0, 0, "Original Matrix:")
    table.cell(t, 0, 1, str.tostring(m1))
    table.cell(t, 1, 0, "Submatrix:")
    table.cell(t, 1, 1, str.tostring(m2))
Devoluciones
Nuevo objeto de matriz que contiene la submatriz de la matriz id definida por los índices from_row, to_row, from_column y to_column.
Observaciones
La indexación de las filas y columnas empieza a partir de cero.
Ver también
matrix.new<type>
matrix.set
matrix.row
matrix.col
matrix.reshape
matrix.sum()2 sobrecargas

La función devuelve una nueva matriz resultante de la suma de dos matrices id1 y id2, o de una matriz id1 y un escalar id2 (un valor numérico).
Sintaxis y sobrecargas
matrix.sum(id1, id2) → matrix<int>
matrix.sum(id1, id2) → matrix<float>
Argumentos
id1 (matrix<int>) Primer objeto de la matriz.
id2 (series int/float/matrix<int>) Segundo objeto de la matriz o valor escalar.
Suma de dos matrices
Ejemplo

//@version=5
indicator("`matrix.sum()` Example 1")

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Create a 2x3 matrix containing values `5`.
    var m1 = matrix.new<float>(2, 3, 5) 
    // Create a 2x3 matrix containing values `4`.
    var m2 = matrix.new<float>(2, 3, 4) 
    // Create a new matrix that sums matrices `m1` and `m2`.
    var m3 = matrix.sum(m1, m2) 
    
    // Display using a table.
    var t = table.new(position.top_right, 1, 2, color.green)
    table.cell(t, 0, 0, "Sum of two matrices:")
    table.cell(t, 0, 1, str.tostring(m3))
Suma de una matriz y un escalar
Ejemplo

//@version=5
indicator("`matrix.sum()` Example 2")

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Create a 2x3 matrix with values `4`.
    var m1 = matrix.new<float>(2, 3, 4)
    
    // Create a new matrix containing the sum of the `m1` matrix with the "int" value `1`.
    var m2 = matrix.sum(m1, 1)
    
    // Display using a table.
    var t = table.new(position.top_right, 1, 2, color.green)
    table.cell(t, 0, 0, "Sum of a matrix and a scalar:")
    table.cell(t, 0, 1, str.tostring(m2))
Devoluciones
Nuevo objeto de matriz que contiene la suma de id2 y id1.
Ver también
matrix.new<type>
matrix.get
matrix.set
matrix.columns
matrix.rows
matrix.swap_columns()

Función que intercambia las columnas en el índice column1 y column2 en la matriz id.
Sintaxis
matrix.swap_columns(id, column1, column2) → void
Argumentos
id (any matrix type) Objeto matriz.
column1 (series int) Índice de la primera columna a intercambiar.
column2 (series int) Índice de la segunda columna a intercambiar.
Ejemplo

//@version=5
indicator("`matrix.swap_columns()` Example")

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Create a 2x2 matrix with ‘na’ values.
    var m1 = matrix.new<int>(2, 2, na)    
    // Fill the matrix with values.
    matrix.set(m1, 0, 0, 1)
    matrix.set(m1, 0, 1, 2)
    matrix.set(m1, 1, 0, 3)
    matrix.set(m1, 1, 1, 4)
    
    // Copy the matrix to a new one.
    var m2 = matrix.copy(m1)
    
    // Swap the first and second columns of the matrix copy.
    matrix.swap_columns(m2, 0, 1)

    // Display using a table.
    var t = table.new(position.top_right, 2, 2, color.green)
    table.cell(t, 0, 0, "Original matrix:")
    table.cell(t, 0, 1, str.tostring(m1))
    table.cell(t, 1, 0, "Swapped columns in copy:")
    table.cell(t, 1, 1, str.tostring(m2))
Observaciones
La indexación de las filas y columnas empieza a partir de cero.
Ver también
matrix.new<type>
matrix.get
matrix.set
matrix.columns
matrix.rows
matrix.swap_rows()

Función que intercambia las filas en el índice row1 y row2 en la matriz id.
Sintaxis
matrix.swap_rows(id, row1, row2) → void
Argumentos
id (any matrix type) Objeto matriz.
row1 (series int) Índice de la primera fila a intercambiar.
row2 (series int) Índice de la segunda fila a intercambiar.
Ejemplo

//@version=5
indicator("`matrix.swap_rows()` Example")

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Create a 3x2 matrix with ‘na’ values.
    var m1 = matrix.new<int>(3, 2, na)
    // Fill the matrix with values.
    matrix.set(m1, 0, 0, 1)
    matrix.set(m1, 0, 1, 2)
    matrix.set(m1, 1, 0, 3)
    matrix.set(m1, 1, 1, 4)
    matrix.set(m1, 2, 0, 5)
    matrix.set(m1, 2, 1, 6)
    
    // Copy the matrix to a new one.
    var m2 = matrix.copy(m1)
    
    // Swap the first and second rows of the matrix copy.
    matrix.swap_rows(m2, 0, 1)
    
    // Display using a table.
    var t = table.new(position.top_right, 2, 2, color.green)
    table.cell(t, 0, 0, "Original matrix:")
    table.cell(t, 0, 1, str.tostring(m1))
    table.cell(t, 1, 0, "Swapped rows in copy:")
    table.cell(t, 1, 1, str.tostring(m2))
Observaciones
La indexación de las filas y columnas empieza a partir de cero.
Ver también
matrix.new<type>
matrix.get
matrix.set
matrix.columns
matrix.swap_columns
matrix.trace()2 sobrecargas

Función que calcula el traza de una matriz (la suma de los elementos de la diagonal principal).
Sintaxis y sobrecargas
matrix.trace(id) → series float
matrix.trace(id) → series int
Argumentos
id (matrix<int/float>) Objeto matriz.
Ejemplo

//@version=5
indicator("`matrix.trace()` Example")

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Create a 2x2 matrix. 
    var m1 = matrix.new<int>(2, 2, na)
    // Fill the matrix with values.
    matrix.set(m1, 0, 0, 1)
    matrix.set(m1, 0, 1, 2)
    matrix.set(m1, 1, 0, 3)
    matrix.set(m1, 1, 1, 4)
    
    // Get the trace of the matrix.
    tr = matrix.trace(m1)
    
    // Display matrix elements.
    var t = table.new(position.top_right, 2, 2, color.green)
    table.cell(t, 0, 0, "Matrix elements:")
    table.cell(t, 0, 1, str.tostring(m1))
    table.cell(t, 1, 0, "Trace of the matrix:")
    table.cell(t, 1, 1, str.tostring(tr))
Devoluciones
Traza de la matriz id.
Ver también
matrix.new<type>
matrix.get
matrix.set
matrix.columns
matrix.rows
matrix.transpose()

Función que crea una nueva versión transpuesta de id. Esto intercambia el índice de fila y columna de cada elemento.
Sintaxis
matrix.transpose(id) → matrix<type>
Argumentos
id (any matrix type) Objeto matriz.
Ejemplo

//@version=5
indicator("`matrix.transpose()` Example")

// For efficiency, execute this code only once.
if barstate.islastconfirmedhistory
    // Create a 2x2 matrix. 
    var m1 = matrix.new<float>(2, 2, na)
    // Fill the matrix with values.
    matrix.set(m1, 0, 0, 1)
    matrix.set(m1, 0, 1, 2)
    matrix.set(m1, 1, 0, 3)
    matrix.set(m1, 1, 1, 4)
    
    // Create a transpose of the matrix.
    var m2 = matrix.transpose(m1)
    
    // Display using a table.
    var t = table.new(position.top_right, 2, 2, color.green)
    table.cell(t, 0, 0, "Original matrix:")
    table.cell(t, 0, 1, str.tostring(m1))
    table.cell(t, 1, 0, "Transposed matrix:")
    table.cell(t, 1, 1, str.tostring(m2))
Devoluciones
Nueva matriz que contiene la versión transpuesta de la matriz id.
Ver también
matrix.new<type>
matrix.set
matrix.columns
matrix.rows
matrix.reshape
matrix.reverse
max_bars_back()

Función que establece la cantidad máxima de barras disponibles para la referencia histórica de una variable integrada o de usuario determinada. Cuando se aplica el operador '[]' a una variable, hace referencia a un valor histórico de esa variable.
Si un argumento de un operador '[]' es un valor constante de tiempo de compilación (por ejemplo, 'v[10]', 'close[500]'), no es necesario utilizar la función 'max_bars_back' para esa variable. El compilador Pine Script® usará ese valor constante como tamaño del búfer histórico.
Si el argumento de un operador '[]' es un valor calculado en tiempo de ejecución (por ejemplo, 'v[i]' donde 'i' - es una variable de serie), Pine Script® intenta detectar de forma automática el tamaño del búfer histórico en tiempo de ejecución. A veces falla y el script se bloquea porque, al final, se refiere a valores históricos que se encuentran fuera del búfer. En ese caso, debe solucionar este problema manualmente utilizando 'max_bars_back'.
Sintaxis
max_bars_back(var, num) → void
Argumentos
var (series int/float/bool/color/label/line) Identificador de variable de serie al que hay que modificar el tamaño del búfer histórico Los valores posibles son: 'open', 'high', 'low', 'close', 'volume', 'time' o cualquier id de variable definida por el usuario.
num (const int) Tamaño del buffer histórico, es decir, el número de barras que se pueden referenciar para la variable 'var'.
Ejemplo

//@version=5
indicator("max_bars_back")
close_() => close
depth() => 400
d = depth()
v = close_()
max_bars_back(v, 500)
out = if bar_index > 0
    v[d]
else
    v
plot(out)
Devoluciones
void
Observaciones
Por el momento, 'max_bars_back' no se puede aplicar a los elementos integrados como 'hl2', 'hlc3', 'ohlc4'. Como solución alternativa, utilice diversas llamadas 'max_bars_back' (por ejemplo, en lugar de una única llamada 'max_bars_back(hl2, 100)' puede llamar dos veces a la función: 'max_bars_back(high, 100), max_bars_back(low, 100)').
Si se utiliza el parámetro 'max_bars_back' indicator o strategy, se verán afectadas todas las variables del indicador. Es probable que se produzca un consumo de memoria excesivo y que cause problemas en el tiempo de ejecución. A ser posible (es decir, cuando la causa es por una variable en lugar de una función), utilice la función max_bars_back.
Ver también
indicator
minute()2 sobrecargas

Sintaxis y sobrecargas
minute(time) → series int
minute(time, timezone) → series int
Argumentos
time (series int) Tiempo UNIX en milisegundos.
Devoluciones
Minuto (en zona horaria del mercado) para el tiempo UNIX proporcionado.
Observaciones
Tiempo UNIX es el número de milisegundos que han transcurrido desde las 00:00:00 UTC, del 1 de enero de 1970.
Ver también
minute
time
year
month
dayofmonth
dayofweek
hour
second
month()2 sobrecargas

Sintaxis y sobrecargas
month(time) → series int
month(time, timezone) → series int
Argumentos
time (series int) Tiempo UNIX en milisegundos.
Devoluciones
Mes (en zona horaria del mercado) para el tiempo UNIX proporcionado.
Observaciones
Tiempo UNIX es el número de milisegundos que han transcurrido desde las 00:00:00 UTC, del 1 de enero de 1970.
Observe que esta función devuelve el día en función de la fecha del comienzo de la barra. Para las sesiones overnight (por ejemplo, EURUSD, en la que la sesión del lunes empieza el domingo, a las 17:00 UTC-4) este valor puede ser 1 punto inferior respecto al mes desde el día del trading.
Ver también
month
time
year
dayofmonth
dayofweek
hour
minute
second
na()2 sobrecargas

Comprueba si x es na.
Sintaxis y sobrecargas
na(x) → series bool
na(x) → simple bool
Argumentos
x (<arg_type>) Valor a comprobar.
Ejemplo

//@version=5
indicator("na")
// Use the `na()` function to test for `na`.
plot(na(close[1]) ? close : close[1])
// ALTERNATIVE
// `nz()` also tests `close[1]` for `na`. It returns `close[1]` if it is not `na`, and `close` if it is.
plot(nz(close[1], close))
Devoluciones
Devuelve true si x es na, en caso contrario false.
Ver también
na
fixnan
nz
nz()16 sobrecargas

Sustituye los valores NaN con ceros (o un valor establecido) en una serie.
Sintaxis y sobrecargas
nz(source) → simple color
nz(source) → simple int
nz(source) → series color
nz(source) → series int
nz(source) → simple float
nz(source) → series float
nz(source, replacement) → simple color
nz(source, replacement) → simple int
nz(source, replacement) → series color
nz(source, replacement) → series int
nz(source, replacement) → simple float
nz(source, replacement) → series float
nz(source) → simple bool
nz(source) → series bool
nz(source, replacement) → simple bool
nz(source, replacement) → series bool
Argumentos
source (simple color) Serie de valores a procesar.
Ejemplo

//@version=5
indicator("nz", overlay=true)
plot(nz(ta.sma(close, 100)))
Devoluciones
Valor de source si no es na. Si el valor de source es na, devuelve cero, o el argumento replacement cuando se utiliza uno.
Ver también
na
na
fixnan
plot()

Traza una serie de datos en el gráfico.
Sintaxis
plot(series, title, color, linewidth, style, trackprice, histbase, offset, join, editable, show_last, display, format, precision, force_overlay) → plot
Argumentos
series (series int/float) Serie de datos a trazar . Argumento obligatorio.
title (const string) Titulo del trazado del gráfico.
color (series color) Color del plot (trazado). Puede utilizar constantes como 'color=color.red' o 'color=#ff001a', así como expresiones complejas del tipo 'color = close >= open ? color.green : color.red'. Argumento opcional.
linewidth (input int) Ancho de línea trazada. El valor por defecto es 1. No aplicable a todos los estilos.
style (input plot_style) Tipo de trazado. Los valores posibles son: plot.style_line, plot.style_stepline, plot.style_stepline_diamond, plot.style_histogram, plot.style_cross, plot.style_area, plot.style_columns, plot.style_circles, plot.style_linebr, plot.style_areabr, plot.style_steplinebr. El valor por defecto es plot.style_line.
trackprice (input bool) Si es true, la línea de precio horizontal se mostrará al nivel del último valor del indicador. El valor por defecto es false.
histbase (input int/float) Valor del precio que se utiliza como punto de referencia cuando se represente el trazado con los estilos plot.style_histogram, plot.style_columns o plot.style_area. El valor por defecto es 0.0.
offset (series int) Cambia el gráfico a la izquierda o derecha en el número determinado de barras. El valor por defecto es 0.
join (input bool) Si es true, los puntos plot (trazado gráfico) se unirán con la línea, aplicable solo a los estilos plot.style_cross y plot.style_circles. El valor por defecto es false.
editable (const bool) Si es true, el estilo plot (trazado gráfico) se podrá editar en el cuadro de diálogo Formato. El valor por defecto es true.
show_last (input int) Cuando está configurado, define el número de barras (contando a partir de la última barra y hacia atrás) que se trazan en el gráfico.
display (input plot_display) Controla dónde se muestra la información del trazado. Las opciones de visualización admiten sumas y restas, lo que significa que si se utiliza display.all - display.status_line se mostrará la información del gráfico en todas partes excepto en la línea de estado del script. display.price_scale + display.status_line mostrará el gráfico únicamente en la escala de precios y en la línea de estado. Cuando los argumentos display como display.price_scale tienen equivalentes en la configuración del gráfico controlados por el usuario, la información relevante del gráfico solo aparecerá cuando lo permita la configuración. Valores posibles: display.none, display.pane, display.data_window, display.price_scale, display.status_line, display.all. Opcional. Por defecto es display.all.
format (input string) Determina si el script formatea los valores del gráfico como precios, porcentajes o valores de volumen. El argumento pasado a este parámetro sustituye al parámetro format de las funciones indicator y strategy. Opcional. El valor por defecto es el format que utiliza la función indicator/strategy. Valores posibles: format.price, format.percent, format.volume.
precision (input int) Cantidad de dígitos que van después del punto decimal que muestran los valores del gráfico en el eje "y" del panel del gráfico, así como en la línea de estado del script y la Ventana de datos. Acepta un número entero no negativo menor o igual a16. El argumento pasado a este parámetro sustituye al precision de las funciones indicator y strategy.Cuando el parámetro format de la función utiliza format.volume, el parámetro precision no afectará al resultado, ya que las normas sobre la precisión decimal definidas por format.volume sustituyen a cualquier otro ajuste de precisión. Es opcional. El valor por defecto es precision que utiliza la función indicator/strategy.
force_overlay (const bool) Si es true, los resultados trazados se mostrarán en el panel principal del gráfico, incluso cuando el script ocupe un panel separado. Es opcional. Por defecto es false.
Ejemplo

//@version=5
indicator("plot")
plot(high+low, title='Title', color=color.new(#00ffaa, 70), linewidth=2, style=plot.style_area, offset=15, trackprice=true)

// You may fill the background between any two plots with a fill() function:
p1 = plot(open)
p2 = plot(close)
fill(p1, p2, color=color.new(color.green, 90))
Devoluciones
Objeto plot, que se puede utilizar en fill
Ver también
plotshape
plotchar
plotarrow
barcolor
bgcolor
fill
plotarrow()

Traza las flechas ascendentes y descendentes en el gráfico. La flecha ascendente se dibuja en cada valor positivo del indicador, la flecha descendente se dibuja en cada valor negativo. Si el indicador devuelve na, entonces no se dibuja ninguna flecha. Las flechas tienen diferentes alturas, cuanto mayor es el valor absoluto del indicador, más larga se dibuja la flecha.
Sintaxis
plotarrow(series, title, colorup, colordown, offset, minheight, maxheight, editable, show_last, display, format, precision, force_overlay) → void
Argumentos
series (series int/float) Serie de datos que se trazarán como flechas. Argumento obligatorio.
title (const string) Titulo del trazado del gráfico.
colorup (series color) Color de las flechas ascendentes. Argumento opcional.
colordown (series color) Color de las flechas descendentes. Argumento opcional.
offset (series int) Desplaza las flechas a la izquierda o derecha en el número determinado de barras. El valor por defecto es 0.
minheight (input int) Altura mínima posible de la flecha en píxeles. El valor por defecto es 5.
maxheight (input int) Altura máxima posible de la flecha en pixeles. El valor por defecto es 100.
editable (const bool) Si es true, el estilo plotarrow (trazado gráfico de la flecha) se podrá editar en el cuadro de diálogo Formato. El valor por defecto es true.
show_last (input int) Cuando está configurado, define el número de flechas (contando a partir de la última barra y hacia atrás) que se trazan en el gráfico.
display (input plot_display) Controla dónde se muestra la información del trazado. Las opciones de visualización admiten sumas y restas, lo que significa que si se utiliza display.all - display.status_line se mostrará la información del gráfico en todas partes excepto en la línea de estado del script. display.price_scale + display.status_line mostrará el gráfico únicamente en la escala de precios y en la línea de estado. Cuando los argumentos display como display.price_scale tienen equivalentes en la configuración del gráfico controlados por el usuario, la información relevante del gráfico solo aparecerá cuando lo permita la configuración. Valores posibles: display.none, display.pane, display.data_window, display.price_scale, display.status_line, display.all. Opcional. Por defecto es display.all.
format (input string) Determina si el script formatea los valores del gráfico como precios, porcentajes o valores de volumen. El argumento pasado a este parámetro sustituye al parámetro format de las funciones indicator y strategy. Opcional. El valor por defecto es el format que utiliza la función indicator/strategy. Valores posibles: format.price, format.percent, format.volume.
precision (input int) Cantidad de dígitos que van después del punto decimal que muestran los valores del gráfico en el eje "y" del panel del gráfico, así como en la línea de estado del script y la Ventana de datos. Acepta un número entero no negativo menor o igual a16. El argumento pasado a este parámetro sustituye al precision de las funciones indicator y strategy.Cuando el parámetro format de la función utiliza format.volume, el parámetro precision no afectará al resultado, ya que las normas sobre la precisión decimal definidas por format.volume sustituyen a cualquier otro ajuste de precisión. Es opcional. El valor por defecto es precision que utiliza la función indicator/strategy.
force_overlay (const bool) Si es true, los resultados trazados se mostrarán en el panel principal del gráfico, incluso cuando el script ocupe un panel separado. Es opcional. Por defecto es false.
Ejemplo

//@version=5
indicator("plotarrow example", overlay=true)
codiff = close - open
plotarrow(codiff, colorup=color.new(color.teal,40), colordown=color.new(color.orange, 40))
Observaciones
¡Utilice la función plotarrow junto con el parámetro 'overlay=true' indicator!
Ver también
plot
plotshape
plotchar
barcolor
bgcolor
plotbar()

Traza barras OHLC en el gráfico.
Sintaxis
plotbar(open, high, low, close, title, color, editable, show_last, display, format, precision, force_overlay) → void
Argumentos
open (series int/float) Series de datos de apertura que se utilizan como valores de apertura en las barras. Argumento obligatorio.
high (series int/float) Series de datos con máximos que se utilizan como valores máximos en las barras. Argumento obligatorio.
low (series int/float) Series de datos con mínimos que se utilizan como valores mínimos en las barras. Argumento obligatorio.
close (series int/float) Series de datos de cierre que se utilizan como valores de cierre en las barras. Argumento obligatorio.
title (const string) Título de plotbar. Argumento opcional.
color (series color) Color de las barras ohlc. Puede utilizar constantes como 'color=color.red' o 'color=#ff001a', así como expresiones complejas del tipo 'color = close >= open ? color.green : color.red'. Argumento opcional.
editable (const bool) Si es true, entonces el estilo plotbar (barra de trazado gráfico) se podrá editar en el cuadro de diálogo Formato. El valor por defecto es true.
show_last (input int) Cuando está configurado, define el número de barras (contando a partir de la última barra y hacia atrás) que se trazan en el gráfico.
display (input plot_display) Controla dónde se muestra la información del trazado. Las opciones de visualización admiten sumas y restas, lo que significa que si se utiliza display.all - display.status_line se mostrará la información del gráfico en todas partes excepto en la línea de estado del script. display.price_scale + display.status_line mostrará el gráfico únicamente en la escala de precios y en la línea de estado. Cuando los argumentos display como display.price_scale tienen equivalentes en la configuración del gráfico controlados por el usuario, la información relevante del gráfico solo aparecerá cuando lo permita la configuración. Valores posibles: display.none, display.pane, display.data_window, display.price_scale, display.status_line, display.all. Opcional. Por defecto es display.all.
format (input string) Determina si el script formatea los valores del gráfico como precios, porcentajes o valores de volumen. El argumento pasado a este parámetro sustituye al parámetro format de las funciones indicator y strategy. Opcional. El valor por defecto es el format que utiliza la función indicator/strategy. Valores posibles: format.price, format.percent, format.volume.
precision (input int) Cantidad de dígitos que van después del punto decimal que muestran los valores del gráfico en el eje "y" del panel del gráfico, así como en la línea de estado del script y la Ventana de datos. Acepta un número entero no negativo menor o igual a16. El argumento pasado a este parámetro sustituye al precision de las funciones indicator y strategy.Cuando el parámetro format de la función utiliza format.volume, el parámetro precision no afectará al resultado, ya que las normas sobre la precisión decimal definidas por format.volume sustituyen a cualquier otro ajuste de precisión. Es opcional. El valor por defecto es precision que utiliza la función indicator/strategy.
force_overlay (const bool) Si es true, los resultados trazados se mostrarán en el panel principal del gráfico, incluso cuando el script ocupe un panel separado. Es opcional. Por defecto es false.
Ejemplo

//@version=5
indicator("plotbar example", overlay=true)
plotbar(open, high, low, close, title='Title', color = open < close ? color.green : color.red)
Observaciones
Incluso cuando un valor open, high, low o close sea igual a NaN, no dibujar barra.
El valor máximo de open, high, low o close se establecerá como 'high' y el valor mínimo como 'low'.
Ver también
plotcandle
plotcandle()

Traza velas en el gráfico.
Sintaxis
plotcandle(open, high, low, close, title, color, wickcolor, editable, show_last, bordercolor, display, format, precision, force_overlay) → void
Argumentos
open (series int/float) Series de datos de apertura que se utilizan como valores de apertura en las velas. Argumento obligatorio.
high (series int/float) Series de datos con máximos que se utilizan como valores máximos en las velas. Argumento obligatorio.
low (series int/float) Series de datos con mínimos que se utilizan como valores mínimos en las velas. Argumento obligatorio.
close (series int/float) Series de datos de cierre que se utilizan como valores de cierre de las velas. Argumento obligatorio.
title (const string) Título de plotcandles. Argumento opcional.
color (series color) Color de las velas. Puede utilizar constantes como 'color=color.red' o 'color=#ff001a' , así como expresiones complejas del tipo 'color = close >= open ? color.green : color.red'. Argumento opcional.
wickcolor (series color) Color de la mecha de las velas. Argumento opcional.
editable (const bool) Si es true, entonces el estilo plotcandle (trazado gráfico de la vela) se podrá editar en el cuadro de diálogo Formato. El valor por defecto es true.
show_last (input int) Cuando está configurado, define el número de velas (contando a partir de la última barra y hacia atrás) que se trazan en el gráfico.
bordercolor (series color) Color del borde de las velas. Argumento opcional.
display (input plot_display) Controla dónde se muestra la información del trazado. Las opciones de visualización admiten sumas y restas, lo que significa que si se utiliza display.all - display.status_line se mostrará la información del gráfico en todas partes excepto en la línea de estado del script. display.price_scale + display.status_line mostrará el gráfico únicamente en la escala de precios y en la línea de estado. Cuando los argumentos display como display.price_scale tienen equivalentes en la configuración del gráfico controlados por el usuario, la información relevante del gráfico solo aparecerá cuando lo permita la configuración. Valores posibles: display.none, display.pane, display.data_window, display.price_scale, display.status_line, display.all. Opcional. Por defecto es display.all.
format (input string) Determina si el script formatea los valores del gráfico como precios, porcentajes o valores de volumen. El argumento pasado a este parámetro sustituye al parámetro format de las funciones indicator y strategy. Opcional. El valor por defecto es el format que utiliza la función indicator/strategy. Valores posibles: format.price, format.percent, format.volume.
precision (input int) Cantidad de dígitos que van después del punto decimal que muestran los valores del gráfico en el eje "y" del panel del gráfico, así como en la línea de estado del script y la Ventana de datos. Acepta un número entero no negativo menor o igual a16. El argumento pasado a este parámetro sustituye al precision de las funciones indicator y strategy.Cuando el parámetro format de la función utiliza format.volume, el parámetro precision no afectará al resultado, ya que las normas sobre la precisión decimal definidas por format.volume sustituyen a cualquier otro ajuste de precisión. Es opcional. El valor por defecto es precision que utiliza la función indicator/strategy.
force_overlay (const bool) Si es true, los resultados trazados se mostrarán en el panel principal del gráfico, incluso cuando el script ocupe un panel separado. Es opcional. Por defecto es false.
Ejemplo

//@version=5
indicator("plotcandle example", overlay=true)
plotcandle(open, high, low, close, title='Title', color = open < close ? color.green : color.red, wickcolor=color.black)
Observaciones
Incluso cuando un valor open, high, low o close sea igual a NaN, no dibujar barra.
El valor máximo de open, high, low o close se establecerá como 'high' y el valor mínimo como 'low'.
Ver también
plotbar
plotchar()

Traza figuras visuales utilizando cualquier carácter Unicode del gráfico.
Sintaxis
plotchar(series, title, char, location, color, offset, text, textcolor, editable, size, show_last, display, format, precision, force_overlay) → void
Argumentos
series (series int/float/bool) Serie de datos que se traza en forma de figuras. La serie se trata como una serie de valores booleanos para todos los valores de ubicación, excepto location.absolute. Argumento obligatorio.
title (const string) Titulo del trazado del gráfico.
char (input string) Carácter a utilizar como figura visual.
location (input string) Ubicación de las figuras en el gráfico. Los valores posibles son: location.abovebar, location.belowbar, location.top, location.bottom, location.absolute. El valor por defecto es location.abovebar.
color (series color) Color de las figuras . Puede utilizar constantes como 'color=color.red' o 'color=#ff001a', así como expresiones complejas del tipo 'color = close >= open ? color.green : color.red'. Argumento opcional.
offset (series int) Cambia las figuras a la izquierda o a la derecha en la cantidad determinada de barras. El valor por defecto es 0.
text (const string) Texto que se mostrará con la figura. Puede utilizar un texto multilínea. Para separar líneas utilice la secuencia '\n' escape. Por ejemplo: 'line one\nline two'.
textcolor (series color) Color del texto. Puede utilizar constantes como 'textcolor=color.red' o 'textcolor=#ff001a', así como expresiones complejas del tipo 'textcolor = close >= open ? color.green : color.red'. Argumento opcional.
editable (const bool) Si es true, entonces el estilo del plotchar se podrá editar en el cuadro de diálogo Formato. El valor por defecto es true.
size (const string) Tamaño de los caracteres en el gráfico. Los valores posibles son: size.auto, size.tiny, size.small, size.normal, size.large, size.huge. Por defecto es size.auto.
show_last (input int) Cuando está configurado, define el número de caracteres (contando a partir de la última barra y hacia atrás) que se trazan en el gráfico.
display (input plot_display) Controla dónde se muestra la información del trazado. Las opciones de visualización admiten sumas y restas, lo que significa que si se utiliza display.all - display.status_line se mostrará la información del gráfico en todas partes excepto en la línea de estado del script. display.price_scale + display.status_line mostrará el gráfico únicamente en la escala de precios y en la línea de estado. Cuando los argumentos display como display.price_scale tienen equivalentes en la configuración del gráfico controlados por el usuario, la información relevante del gráfico solo aparecerá cuando lo permita la configuración. Valores posibles: display.none, display.pane, display.data_window, display.price_scale, display.status_line, display.all. Opcional. Por defecto es display.all.
format (input string) Determina si el script formatea los valores del gráfico como precios, porcentajes o valores de volumen. El argumento pasado a este parámetro sustituye al parámetro format de las funciones indicator y strategy. Opcional. El valor por defecto es el format que utiliza la función indicator/strategy. Valores posibles: format.price, format.percent, format.volume.
precision (input int) Cantidad de dígitos que van después del punto decimal que muestran los valores del gráfico en el eje "y" del panel del gráfico, así como en la línea de estado del script y la Ventana de datos. Acepta un número entero no negativo menor o igual a16. El argumento pasado a este parámetro sustituye al precision de las funciones indicator y strategy.Cuando el parámetro format de la función utiliza format.volume, el parámetro precision no afectará al resultado, ya que las normas sobre la precisión decimal definidas por format.volume sustituyen a cualquier otro ajuste de precisión. Es opcional. El valor por defecto es precision que utiliza la función indicator/strategy.
force_overlay (const bool) Si es true, los resultados trazados se mostrarán en el panel principal del gráfico, incluso cuando el script ocupe un panel separado. Es opcional. Por defecto es false.
Ejemplo

//@version=5
indicator("plotchar example", overlay=true)
data = close >= open
plotchar(data, char='❄')
Observaciones
¡Utilice la función plotchar junto con el parámetro 'overlay=true' indicator!
Ver también
plot
plotshape
plotarrow
barcolor
bgcolor
plotshape()

Traza figuras visuales en el gráfico.
Sintaxis
plotshape(series, title, style, location, color, offset, text, textcolor, editable, size, show_last, display, format, precision, force_overlay) → void
Argumentos
series (series int/float/bool) Serie de datos que se traza en forma de figuras. La serie se trata como una serie de valores booleanos para todos los valores de ubicación, excepto location.absolute. Argumento obligatorio.
title (const string) Titulo del trazado del gráfico.
style (input string) Tipo de trazado. Los valores posibles son: shape.xcross, shape.cross, shape.triangleup, shape.triangledown, shape.flag, shape.circle, shape.arrowup, shape.arrowdown, shape.labelup, shape.labeldown, shape.square, shape.diamond. El valor por defecto es shape.xcross.
location (input string) Ubicación de las figuras en el gráfico. Los valores posibles son: location.abovebar, location.belowbar, location.top, location.bottom, location.absolute. El valor por defecto es location.abovebar.
color (series color) Color de las figuras . Puede utilizar constantes como 'color=color.red' o 'color=#ff001a', así como expresiones complejas del tipo 'color = close >= open ? color.green : color.red'. Argumento opcional.
offset (series int) Cambia las figuras a la izquierda o a la derecha en la cantidad determinada de barras. El valor por defecto es 0.
text (const string) Texto que se mostrará con la figura. Puede utilizar un texto multilínea. Para separar líneas utilice la secuencia '\n' escape. Por ejemplo: 'line one\nline two'.
textcolor (series color) Color del texto. Puede utilizar constantes como 'textcolor=color.red' o 'textcolor=#ff001a', así como expresiones complejas del tipo 'textcolor = close >= open ? color.green : color.red'. Argumento opcional.
editable (const bool) Si es true entonces el estilo de plotshape (figura del trazado) se podrá editar en el cuadro de diálogo Formato. El valor por defecto es true.
size (const string) Tamaño de las figuras en el gráfico. Los valores posibles son: size.auto, size.tiny, size.small, size.normal, size.large, size.huge. Por defecto es size.auto.
show_last (input int) Cuando está configurado, define la cantidad de figuras de formas (contando a partir de la última barra y hacia atrás) que se trazan en el gráfico.
display (input plot_display) Controla dónde se muestra la información del trazado. Las opciones de visualización admiten sumas y restas, lo que significa que si se utiliza display.all - display.status_line se mostrará la información del gráfico en todas partes excepto en la línea de estado del script. display.price_scale + display.status_line mostrará el gráfico únicamente en la escala de precios y en la línea de estado. Cuando los argumentos display como display.price_scale tienen equivalentes en la configuración del gráfico controlados por el usuario, la información relevante del gráfico solo aparecerá cuando lo permita la configuración. Valores posibles: display.none, display.pane, display.data_window, display.price_scale, display.status_line, display.all. Opcional. Por defecto es display.all.
format (input string) Determina si el script formatea los valores del gráfico como precios, porcentajes o valores de volumen. El argumento pasado a este parámetro sustituye al parámetro format de las funciones indicator y strategy. Opcional. El valor por defecto es el format que utiliza la función indicator/strategy. Valores posibles: format.price, format.percent, format.volume.
precision (input int) Cantidad de dígitos que van después del punto decimal que muestran los valores del gráfico en el eje "y" del panel del gráfico, así como en la línea de estado del script y la Ventana de datos. Acepta un número entero no negativo menor o igual a16. El argumento pasado a este parámetro sustituye al precision de las funciones indicator y strategy.Cuando el parámetro format de la función utiliza format.volume, el parámetro precision no afectará al resultado, ya que las normas sobre la precisión decimal definidas por format.volume sustituyen a cualquier otro ajuste de precisión. Es opcional. El valor por defecto es precision que utiliza la función indicator/strategy.
force_overlay (const bool) Si es true, los resultados trazados se mostrarán en el panel principal del gráfico, incluso cuando el script ocupe un panel separado. Es opcional. Por defecto es false.
Ejemplo

//@version=5
indicator("plotshape example 1", overlay=true)
data = close >= open
plotshape(data, style=shape.xcross)
Observaciones
¡Utilice la función plotshape junto con el parámetro 'overlay=true' indicator!
Ver también
plot
plotchar
plotarrow
barcolor
bgcolor
polyline.delete()

Elimina el objeto polyline especificado. No tiene efecto si el id no existe.
Sintaxis
polyline.delete(id) → void
Argumentos
id (series polyline) ID de la polilínea a borrar.
polyline.new()

Crea una nueva instancia polyline y la muestra en el gráfico, conectando de forma secuencial todos los puntos del array points con segmentos de línea. Los segmentos en el dibujo pueden ser rectos o curvos dependiendo del parámetro curved.
Sintaxis
polyline.new(points, curved, closed, xloc, line_color, fill_color, line_style, line_width, force_overlay) → series polyline
Argumentos
points (array<chart.point>) Un array de objetos chart.point para el dibujo a conectar de forma secuencial.
curved (series bool) Si es true, el dibujo conectará todos los puntos del array points utilizando segmentos de línea curvos. Opcional. Por defecto es false.
closed (series bool) Si es true, el dibujo también conectará el primer punto con el último punto del array points, dando como resultado una polilínea cerrada. Opcional. Por defecto es false.
xloc (series string) Determina el campo de los objetos chart.point del array points que utilizará la polilínea para sus coordenadas x. Si es xloc.bar_index, la polilínea usará el campo index de cada punto. Si es xloc.bar_time, empleará el campo time. Opcional. Por defecto es xloc.bar_index.
line_color (series color) Color de los segmentos de línea. Opcional. Por defecto es color.blue.
fill_color (series color) Color de relleno de la polilínea. Opcional. Por defecto es na.
line_style (series string) Estilo de la polilínea. Valores posibles: line.style_solid, line.style_dotted, line.style_dashed, line.style_arrow_left, line.style_arrow_right, line.style_arrow_both. Opcional. Por defecto es line.style_solid.
line_width (series int) El ancho de los segmentos de línea, expresada en píxeles. Opcional. Por defecto es 1.
force_overlay (const bool) Si es true, el dibujo se mostrará en el panel principal del gráfico, incluso cuando el script ocupe un panel separado. Es opcional. Por defecto es false.
Ejemplo

//@version=5
indicator("Polylines example", overlay = true)

//@variable If `true`, connects all points in the polyline with curved line segments. 
bool curvedInput = input.bool(false, "Curve Polyline")
//@variable If `true`, connects the first point in the polyline to the last point.
bool closedInput = input.bool(true,  "Close Polyline")
//@variable The color of the space filled by the polyline.
color fillcolor = input.color(color.new(color.blue, 90), "Fill Color")

// Time and price inputs for the polyline's points. 
p1x = input.time(0,  "p1", confirm = true, inline = "p1")
p1y = input.price(0, "  ", confirm = true, inline = "p1")
p2x = input.time(0,  "p2", confirm = true, inline = "p2")
p2y = input.price(0, "  ", confirm = true, inline = "p2")
p3x = input.time(0,  "p3", confirm = true, inline = "p3")
p3y = input.price(0, "  ", confirm = true, inline = "p3")
p4x = input.time(0,  "p4", confirm = true, inline = "p4")
p4y = input.price(0, "  ", confirm = true, inline = "p4")
p5x = input.time(0,  "p5", confirm = true, inline = "p5")
p5y = input.price(0, "  ", confirm = true, inline = "p5")

if barstate.islastconfirmedhistory
    //@variable An array of `chart.point` objects for the new polyline.
    var points = array.new<chart.point>()
    // Push new `chart.point` instances into the `points` array.
    points.push(chart.point.from_time(p1x, p1y))
    points.push(chart.point.from_time(p2x, p2y))
    points.push(chart.point.from_time(p3x, p3y))
    points.push(chart.point.from_time(p4x, p4y))
    points.push(chart.point.from_time(p5x, p5y))
    // Add labels for each `chart.point` in `points`.
    l1p1 = label.new(points.get(0), text = "p1", xloc = xloc.bar_time, color = na)
    l1p2 = label.new(points.get(1), text = "p2", xloc = xloc.bar_time, color = na)
    l2p1 = label.new(points.get(2), text = "p3", xloc = xloc.bar_time, color = na)
    l2p2 = label.new(points.get(3), text = "p4", xloc = xloc.bar_time, color = na)
    // Create a new polyline that connects each `chart.point` in the `points` array, starting from the first.
    polyline.new(points, curved = curvedInput, closed = closedInput, fill_color = fillcolor, xloc = xloc.bar_time)
Devoluciones
El ID de un nuevo objeto polilínea que un script puede utilizar en otras funciones polyline.*().
Ver también
chart.point.new
request.currency_rate()

Proporciona un tipo que puede utilizarse para convertir un valor expresado en la divisa from a otro en la divisa to.
Sintaxis
request.currency_rate(from, to, ignore_invalid_currency) → series float
Argumentos
from (series string) Divisa en la que se expresa el valor a convertir. Valores posibles: una cadena de tres letras con el código de divisa en formato ISO 4217 (por ejemplo, "USD") o una de las variables integradas que devuelven códigos de divisa, como syminfo.currency o currency.USD.
to (series string) Divisa en la que debe convertirse el valor. Valores posibles: una cadena de tres letras con el código de divisa en el formato ISO 4217 (por ejemplo, "USD") o una de las variables integradas que devuelven códigos de divisa, como syminfo.currency o currency.USD.
ignore_invalid_currency (series bool) Determina el comportamiento de la función si no se puede calcular un tipo de conversión entre las dos divisas: si es false, el script se detiene y devuelve un error de ejecución; cuando es true, la función devuelve na y la ejecución continúa. Opcional. El valor por defecto es false.
Ejemplo

//@version=5
indicator("Close in British Pounds")
rate = request.currency_rate(syminfo.currency, "GBP")
plot(close * rate)
Observaciones
Si los argumentos from y to son iguales, la función devuelve 1. Tenga en cuenta que el uso de esta variable/función puede provocar el repintado del indicador.
request.dividends()

Solicita los datos sobre los dividendos para el símbolo especificado.
Sintaxis
request.dividends(ticker, field, gaps, lookahead, ignore_invalid_symbol, currency) → series float
Argumentos
ticker (series string) Símbolo. Tenga en cuenta que el símbolo debe incluir un prefijo. Por ejemplo: "NASDAQ:AAPL" en lugar de "AAPL". Usar syminfo.ticker generará un error. En su lugar, utilice syminfo.tickerid.
field (series string) Cadena de entrada. Los valores posibles son: dividends.net, dividends.gross. El valor por defecto es dividends.gross.
gaps (simple barmerge_gaps) Estrategia de combinación de los datos solicitados (los datos solicitados se combinan automáticamente con los datos OHLC de la serie principal). Valores posibles: barmerge.gaps_on, barmerge.gaps_off. barmerge.gaps_on; los datos solicitados se combinan con posibles gaps (valores de na). barmerge.gaps_off: los datos solicitados se combinan de forma continua sin gaps; todos los gaps se rellenan con los valores existentes más próximos. El valor por defecto es barmerge.gaps_off.
lookahead (simple barmerge_lookahead) Estrategia de combinación para la posición de datos solicitada. Valores posibles: barmerge.lookahead_on, barmerge.lookahead_off. A partir de la versión 3, el valor por defecto es barmerge.lookahead_off. Tenga en cuenta que el comportamiento es el mismo en tiempo real y solo difiere respecto al histórico.
ignore_invalid_symbol (input bool) Parámetro opcional. Determina el comportamiento de la función si no se encuentra el símbolo especificado: si es false, el script se detendrá y devolverá un error de tiempo de ejecución; si es true, la función devolverá na y seguirá ejecutándose. El valor por defecto es false.
currency (series string) Divisa a la que se convierten los valores de los dividendos del símbolo asociado a la divisa (por ejemplo, dividends.gross). Los tipos de conversión utilizados se basan en los del par FX_IDC del día anterior (en relación con la barra en la que se realiza el cálculo). Opcional. Por defecto es syminfo.currency. Valores posibles: cadena de tres letras con código de divisa en formato ISO 4217 (por ejemplo, "USD") o una de las constantes del espacio de nombres currency.*, por ejemplo, currency.USD.
Ejemplo

//@version=5
indicator("request.dividends")
s1 = request.dividends("NASDAQ:BELFA")
plot(s1)
s2 = request.dividends("NASDAQ:BELFA", dividends.net, gaps=barmerge.gaps_on, lookahead=barmerge.lookahead_on)
plot(s2)
Devoluciones
Serie solicitada o n/a si no existen datos sobre los dividendos para el símbolo especificado.
Ver también
request.earnings
request.splits
request.security
syminfo.tickerid
request.earnings()

Solicita los datos sobre los beneficios para el símbolo especificado.
Sintaxis
request.earnings(ticker, field, gaps, lookahead, ignore_invalid_symbol, currency) → series float
Argumentos
ticker (series string) Símbolo. Tenga en cuenta que el símbolo debe incluir un prefijo. Por ejemplo: "NASDAQ:AAPL" en lugar de "AAPL". Usar syminfo.ticker generará un error. En su lugar, utilice syminfo.tickerid.
field (series string) Cadena de entrada. Los valores posibles son: earnings.actual, earnings.estimate, earnings.standardized. El valor por defecto es earnings.actual.
gaps (simple barmerge_gaps) Estrategia de combinación de los datos solicitados (los datos solicitados se combinan automáticamente con los datos OHLC de la serie principal). Valores posibles: barmerge.gaps_on, barmerge.gaps_off. barmerge.gaps_on; los datos solicitados se combinan con posibles gaps (valores de na). barmerge.gaps_off: los datos solicitados se combinan de forma continua sin gaps; todos los gaps se rellenan con los valores existentes más próximos. El valor por defecto es barmerge.gaps_off.
lookahead (simple barmerge_lookahead) Estrategia de combinación para la posición de datos solicitada. Valores posibles: barmerge.lookahead_on, barmerge.lookahead_off. A partir de la versión 3, el valor por defecto es barmerge.lookahead_off. Tenga en cuenta que el comportamiento es el mismo en tiempo real y solo difiere respecto al histórico.
ignore_invalid_symbol (input bool) Parámetro opcional. Determina el comportamiento de la función si no se encuentra el símbolo especificado: si es false, el script se detendrá y devolverá un error de tiempo de ejecución; si es true, la función devolverá na y seguirá ejecutándose. El valor por defecto es false.
currency (series string) Divisa a la que se convierten los valores de los beneficios del símbolo asociados a la divisa (por ejemplo, earnings.actual). Los tipos de conversión utilizados se basan en los tipos diarios del par FX_IDC del día anterior (en relación con la barra en la que se realiza el cálculo). Opcional. Por defecto es syminfo.currency. Valores posibles: cadena de tres letras con código de divisa en formato ISO 4217 (por ejemplo, "USD") o una de las constantes del espacio de nombres currency.*, por ejemplo, currency.USD.
Ejemplo

//@version=5
indicator("request.earnings")
s1 = request.earnings("NASDAQ:BELFA")
plot(s1)
s2 = request.earnings("NASDAQ:BELFA", earnings.actual, gaps=barmerge.gaps_on, lookahead=barmerge.lookahead_on)
plot(s2)
Devoluciones
Serie solicitada o n/a si no existen datos sobre los beneficios para el símbolo especificado.
Ver también
request.dividends
request.splits
request.security
syminfo.tickerid
request.economic()

Solicita los datos económicos para un símbolo. Los datos económicos incluyen información como la situación economía de un país (PIB, tasa de inflación, etc.) o de un sector concreto (producción de acero, camas de para la UCI, etc.).
Sintaxis
request.economic(country_code, field, gaps, ignore_invalid_symbol) → series float
Argumentos
country_code (series string) Código del país (por ejemplo, "EE.UU.") o de la región (por ejemplo, "UE") para el que se solicitan los datos económicos. El artículo del Centro de ayuda muestra una lista de los países y sus códigos. Los países para los que se dispone información varían en función de la métrica. El artículo del Centro de ayuda correspondiente a cada métrica relaciona los países que disponen de dicha métrica.
field (series string) Código de la métrica económica solicitada (por ejemplo, "PIB"). El artículo del Centro de ayuda relaciona las métricas y sus códigos.
gaps (simple barmerge_gaps) Especifica cómo se combinan los valores devueltos en las barras del gráfico. Valores posibles: barmerge.gaps_off, barmerge.gaps_on. Con barmerge.gaps_on, solo aparece un valor en la barra del gráfico actual cuando se encuentra disponible por primera vez desde el contexto de la función, de lo contrario devuelve na (por lo que se produce un "gap"). Con barmerge.gaps_off, lo que de otro modo serían gaps se rellenan con el último valor conocido devuelto, evitando los valores na. Opcional. Por defecto es barmerge.gaps_off.
ignore_invalid_symbol (input bool) Determina el comportamiento de la función si no se encuentra el símbolo especificado: si es false, el script se detendrá y devolverá un error de ejecución; si es true, la función devolverá na y la ejecución continuará. Opcional. Por defecto es false.
Ejemplo

//@version=5
indicator("US GDP")
e = request.economic("US", "GDP")
plot(e)
Devoluciones
Series solicitadas.
Observaciones
También se puede acceder a los datos económicos desde los gráficos, del mismo que accede a cualquier símbolo normal. Utilice "ECONOMIC" para el nombre de mercado de valores y {country_code}{field} para el ticker. De este modo, el nombre de los datos del PIB de EE.UU. es "ECONOMIC:USGDP".
Ver también
request.financial
request.quandl
request.financial()

Solicita series financieras para el símbolo.
Sintaxis
request.financial(symbol, financial_id, period, gaps, ignore_invalid_symbol, currency) → series float
Argumentos
symbol (series string) Símbolo. Tenga en cuenta que el símbolo debe contener un prefijo. Por ejemplo: "NASDAQ:AAPL" en lugar de "AAPL".
financial_id (series string) Identificador financiero. Puede encontrar la lista de identificadores disponibles a través de nuestro Centro de ayuda.
period (series string) Período del informe. Los valores posibles son "TTM", "FY", "FQ", "FH", "D".
gaps (simple barmerge_gaps) Estrategia de combinación para los datos solicitados (los datos solicitados se combinan automáticamente con la serie principal: datos OHLC). Los valores posibles son: barmerge.gaps_on, barmerge.gaps_off. barmerge.gaps_on; los datos solicitados se combinan con posibles gaps (valores na). barmerge.gaps_off; los datos solicitados se combinan de forma continua sin gaps, todos los gaps se rellenan con los valores anteriores más cercanos. El valor por defecto es barmerge.gaps_off.
ignore_invalid_symbol (input bool) Parámetro opcional. Determina el comportamiento de la función si no se encuentra el símbolo especificado: si es false, el script se detendrá y devolverá un error de tiempo de ejecución; si es true, la función devolverá na y seguirá ejecutándose. El valor por defecto es false.
currency (series string) Divisa a la que se convierten los datos financieros del símbolo (por ejemplo, los ingresos netos). Los tipos de conversión utilizados se basan en los tipos diarios del par FX_IDC del día anterior (en relación con la barra en la que se realiza el cálculo). Opcional. Por defecto es syminfo.currency. Valores posibles: cadena de tres letras con código de divisa en formato ISO 4217 (por ejemplo, "USD") o una de las constantes del espacio de nombres currency.*, por ejemplo, currency.USD.
Ejemplo

//@version=5
indicator("request.financial")
f = request.financial("NASDAQ:MSFT", "ACCOUNTS_PAYABLE", "FY")
plot(f)
Devoluciones
Series solicitadas.
Ver también
request.security
syminfo.tickerid
request.quandl()

Solicita los datos de un símbolo en Nasdaq Data Link (antes Quandl).
Sintaxis
request.quandl(ticker, gaps, index, ignore_invalid_symbol) → series float
Argumentos
ticker (series string) Símbolo. Tenga en cuenta que el nombre de una serie temporal y la fuente de datos Quandl deben dividirse mediante una barra diagonal. Por ejemplo: "CFTC/SB_FO_ALL".
gaps (simple barmerge_gaps) Estrategia de combinación para los datos solicitados (los datos solicitados se combinan automáticamente con la serie principal: datos OHLC). Los valores posibles son: barmerge.gaps_on, barmerge.gaps_off. barmerge.gaps_on; los datos solicitados se combinan con posibles gaps (valores na). barmerge.gaps_off; los datos solicitados se combinan de forma continua sin gaps, todos los gaps se rellenan con los valores anteriores más cercanos. El valor por defecto es barmerge.gaps_off.
index (series int) Índice de columnas de series temporales de Quandl.
ignore_invalid_symbol (input bool) Parámetro opcional. Determina el comportamiento de la función si no se encuentra el símbolo especificado: si es false, el script se detendrá y devolverá un error de tiempo de ejecución; si es true, la función devolverá na y seguirá ejecutándose. El valor por defecto es false.
Ejemplo

//@version=5
indicator("request.quandl")
f = request.quandl("CFTC/SB_FO_ALL", barmerge.gaps_off, 0)
plot(f)
Devoluciones
Series solicitadas.
Ver también
request.security
syminfo.tickerid
request.security()

Solicita el resultado de una expresión a partir de un contexto especificado (símbolo e intervalo de tiempo).
Sintaxis
request.security(symbol, timeframe, expression, gaps, lookahead, ignore_invalid_symbol, currency, calc_bars_count) → series <type>
Argumentos
symbol (series string) Símbolo o identificador de ticker de los datos solicitados. Utilice una cadena vacía o syminfo.tickerid para solicitar datos utilizando el símbolo del gráfico. Para recuperar datos con modificadores adicionales (sesiones con horario ampliado, ajustes de dividendos, tipos de gráficos no estándar como Heikin Ashi y Renko, etc.), cree un identificador de ticker personalizado en la solicitud utilizando las funciones del espacio de nombres ticker.*.
timeframe (series string) Intervalo de tiempo de los datos solicitados. Utilice una cadena vacía o timeframe.period para solicitar los del gráfico o del timeframe especificado en la función indicator. Para solicitar datos con un intervalo diferente, proporcione una cadena de intervalo de tiempo válida. Para obtener más información consulte aquí sobre cómo especificar cadenas de intervalos de tiempo.
expression (variable, function, object, array, matrix, or map of series int/float/bool/string/color/enum, or a tuple of these) Expresión a calcular y devolver del contexto solicitado. Puede aceptar una variable integrada como close, una definida por el usuario, una expresión como ta.change(close) / (high - low), una llamada a una función que no utilice dibujos de Pine Script®, un objeto, una colección o una tupla de expresiones.
gaps (simple barmerge_gaps) Determina cómo se fusionan los valores devueltos en las barras del gráfico. Valores posibles: barmerge.gaps_on, barmerge.gaps_off. Con barmerge.gaps_on solo aparece un valor en la barra del gráfico actual cuando se encuentre disponible por primera vez desde el contexto de la función, de lo contrario devuelve na (por lo que se produce un "gap"). Con barmerge.gaps_off, lo que de otro modo se considerarían gaps, se rellenan con el último valor devuelto, evitando los valores na. Opcional. El valor por defecto es barmerge.gaps_off.
lookahead (simple barmerge_lookahead) Solo en las barras históricas se devuelven los datos del intervalo de tiempo antes de que finalice. Valores posibles: barmerge.lookahead_on, barmerge.lookahead_off. No tiene efecto sobre los valores en tiempo real. Es opcional. El valor por defecto es barmerge.lookahead_off a partir de la v3 de Pine Script®. En las versiones v1 y v2, el valor por defecto es barmerge.lookahead_on. ADVERTENCIA: Si utiliza barmerge.lookahead_on en intervalos de tiempo superiores al del gráfico sin compensar el argumento expression como en close[1], es posible que se produzcan algunos fallos futuros en los scripts, ya que la función devolverá el precio close antes de conocer realmente el contexto actual. Tal y como se explica en la página Repintado del Manual de usuario, esto generará unos resultados confusos.
ignore_invalid_symbol (input bool) Determina el comportamiento de la función si no se encuentra el símbolo especificado: si es false, el script se detendrá y lanzará un error de ejecución; si es true, la función devolverá na y la ejecución continuará. Opcional. Por defecto es false.
currency (series string) Divisa en la que se van a convertir los valores expresados en unidades monetarias (open, high, low, close, etc.) o expresiones que utilicen dichos valores. Los tipos de conversión utilizados se basan en los tipos diarios de los pares FX_IDC del día anterior (respecto a la barra en la que se realiza el cálculo). Valores posibles: una cadena de tres letras con el código de la moneda en formato ISO 4217 (por ejemplo, 'USD') o una de las constantes de currency.* namespace, por ejemplo currency.USD. Tenga en cuenta que los valores literales como 200 no se convierten. Es opcional. El valor por defecto es syminfo.currency.
calc_bars_count (simple int) Si se especifica, la función solo solicitará la cantidad de valores a partir del final del histórico del símbolo y calculará expression como si estos valores fueran los únicos datos disponibles, lo que podría mejorar la velocidad de cálculo en algunos casos. Opcional. El valor por defecto es de 100 000, que es el límite para todos los planes no profesionales de TradingView.
Ejemplo

//@version=5
indicator("Simple `request.security()` calls")
// Returns 1D close of the current symbol.
dailyClose = request.security(syminfo.tickerid, "1D", close)
plot(dailyClose)

// Returns the close of "AAPL" from the same timeframe as currently open on the chart.
aaplClose = request.security("AAPL", timeframe.period, close)
plot(aaplClose)
Ejemplo

//@version=5
indicator("Advanced `request.security()` calls")
// This calculates a 10-period moving average on the active chart.
sma = ta.sma(close, 10)
// This sends the `sma` calculation for execution in the context of the "AAPL" symbol at a "240" (4 hours) timeframe.
aaplSma = request.security("AAPL", "240", sma)
plot(aaplSma)

// To avoid differences on historical and realtime bars, you can use this technique, which only returns a value from the higher timeframe on the bar after it completes:
indexHighTF = barstate.isrealtime ? 1 : 0
indexCurrTF = barstate.isrealtime ? 0 : 1
nonRepaintingClose = request.security(syminfo.tickerid, "1D", close[indexHighTF])[indexCurrTF]
plot(nonRepaintingClose, "Non-repainting close")

// Returns the 1H close of "AAPL", extended session included. The value is dividend-adjusted.
extendedTicker = ticker.modify("NASDAQ:AAPL", session = session.extended, adjustment = adjustment.dividends)
aaplExtAdj = request.security(extendedTicker, "60", close)
plot(aaplExtAdj)

// Returns the result of a user-defined function.
// The `max` variable is mutable, but we can pass it to `request.security()` because it is wrapped in a function.
allTimeHigh(source) =>
    var max = source
    max := math.max(max, source)
allTimeHigh1D = request.security(syminfo.tickerid, "1D", allTimeHigh(high))

// By using a tuple `expression`, we obtain several values with only one `request.security()` call.
[open1D, high1D, low1D, close1D, ema1D] = request.security(syminfo.tickerid, "1D", [open, high, low, close, ta.ema(close, 10)])
plotcandle(open1D, high1D, low1D, close1D)
plot(ema1D)

// Returns an array containing the OHLC values of the chart's symbol from the 1D timeframe.
ohlcArray = request.security(syminfo.tickerid, "1D", array.from(open, high, low, close))
plotcandle(array.get(ohlcArray, 0), array.get(ohlcArray, 1), array.get(ohlcArray, 2), array.get(ohlcArray, 3))
Devoluciones
Resultado determinado por expression.
Observaciones
Los scripts que utilizan esta función pueden calcularse de forma diferente en las barras históricas y en tiempo real, lo que puede resultar en un repintado.
Un único script no puede contener más de 40 llamadas únicas a la función request.*(). Una llamada es única solo en el caso de que no llame a la misma función con los mismos argumentos.
Cuando se realizan dos llamadas a una función request.*() para evaluar la misma expresión desde el mismo contexto con diferentes valores calc_bars_count, la segunda llamada solicita el mismo número de barras históricas que la primera. Por ejemplo, si un script llama a request.security("AAPL", "", close, calc_bars_count = 3) after it calls request.security("AAPL", "", close, calc_bars_count = 5), la segunda llamada también utilizará cinco barras de datos históricos, y, no tres.
El símbolo de una llamada request.() puede ser heredado si no se especifica con precisión, es decir, si el argumento symbol es una cadena vacía o syminfo.tickerid. Del mismo modo, el intervalo de tiempo de una llamada request.() puede heredarse si el argumento timeframe es una cadena vacía o timeframe.period. Estos valores se toman normalmente del gráfico en el que se está ejecutando el script. Sin embargo, si la función A request.*() se llama desde dentro de la expresión de la función B request.*(), entonces la función A puede heredar los valores de la función B. Para obtener más información, consulte aquí.
Ver también
syminfo.ticker
syminfo.tickerid
timeframe.period
ticker.new
ticker.modify
request.security_lower_tf
request.dividends
request.earnings
request.splits
request.financial
request.quandl
request.security_lower_tf()

Solicita los resultados de una expresión de un símbolo especificado en un timeframe inferior o igual al intervalo de tiempo del gráfico. Devuelve una array que contiene un elemento por cada barra del intervalo de tiempo inferior dentro de la barra del gráfico. En un gráfico de 5 minutos, la solicitud de datos utilizando un argumento timeframe de «1» suele devolver una matriz con cinco elementos que representan el valor del expression en cada barra de 1 minuto, ordenados por tiempo con el valor más antiguo en primer lugar.
Sintaxis
request.security_lower_tf(symbol, timeframe, expression, ignore_invalid_symbol, currency, ignore_invalid_timeframe, calc_bars_count) → array<type>
Argumentos
symbol (series string) Símbolo o identificador de ticker de los datos solicitados. Utilice una cadena vacía o syminfo.tickerid para solicitar datos utilizando el símbolo del gráfico. Para recuperar datos con modificadores adicionales (sesiones con horario ampliado, ajustes de dividendos, tipos de gráficos no estándar como Heikin Ashi y Renko, etc.), cree un identificador de ticker personalizado en la solicitud utilizando las funciones del espacio de nombres ticker.*.
timeframe (series string) Intervalo de tiempo de los datos solicitados. Utilice una cadena vacía o timeframe.period para solicitar los del gráfico o del timeframe especificado en la función indicator. Para solicitar datos con un intervalo diferente, proporcione una cadena de intervalo de tiempo válida. Para obtener más información consulte aquí sobre cómo especificar cadenas de intervalos de tiempo.
expression (variable, object or function of series int/float/bool/string/color/enum, or a tuple of these) Expresión a calcular y devolver del contexto solicitado. Puede aceptar una variable integrada como close, una definida por el usuario, una expresión como ta.change(close) / (high - low), una llamada a una función que no utilice dibujos de Pine Script®, un objeto o una tupla de expresiones. Las colecciones no están permitidas a menos que se encuentren dentro de los campos de un objeto
ignore_invalid_symbol (series bool) Determina el comportamiento de la función si no se encuentra el símbolo especificado: si es false, el script se detendrá y lanzará un error de ejecución; si es true, la función devolverá na y la ejecución continuará. Opcional. Por defecto es false.
currency (series string) Divisa en la que se van a convertir los valores expresados en unidades monetarias (open, high, low, close, etc.) o expresiones que utilicen dichos valores. Los tipos de conversión utilizados se basan en los tipos diarios de los pares FX_IDC del día anterior (respecto a la barra en la que se realiza el cálculo). Valores posibles: una cadena de tres letras con el código de la moneda en formato ISO 4217 (por ejemplo, 'USD') o una de las constantes de currency.* namespace, por ejemplo currency.USD. Tenga en cuenta que los valores literales como 200 no se convierten. Es opcional. El valor por defecto es syminfo.currency.
ignore_invalid_timeframe (series bool) Determina el comportamiento de la función cuando el intervalo de tiempo del gráfico es menor que el timeframe utilizado en la llamada a la función. Si es false, el script se detendrá y lanzará un error de ejecución. En el caso de que sea true, la función devolverá na y la ejecución continuará. Opcional. Por defecto es false.
calc_bars_count (simple int) Si se especifica, la función solo solicitará la cantidad de valores a partir del final del histórico del símbolo y calculará expression como si estos valores fueran los únicos datos disponibles, lo que podría mejorar la velocidad de cálculo en algunos casos. Opcional. El valor por defecto es de 100 000, que es el límite para todos los planes no profesionales de TradingView.
Ejemplo

//@version=5
indicator("`request.security_lower_tf()` Example", overlay = true)

// If the current chart timeframe is set to 120 minutes, then the `arrayClose` array will contain two 'close' values from the 60 minute timeframe for each bar.
arrClose = request.security_lower_tf(syminfo.tickerid, "60", close)

if bar_index == last_bar_index - 1
    label.new(bar_index, high, str.tostring(arrClose))
Devoluciones
Array de un tipo determinado por expression o una tupla del mismo.
Observaciones
Los scripts que utilizan esta función pueden calcularse de forma diferente en las barras históricas y en tiempo real, lo que puede resultar en un repintado.
Tenga en cuenta que, con esta función, los diferenciales (por ejemplo, «AAPL+MSFT*TSLA») no siempre devuelven datos fiables.
Un único script no puede contener más de 40 llamadas únicas a la función request.*(). Una llamada es única solo en el caso de que no llame a la misma función con los mismos argumentos.
Cuando se realizan dos llamadas a una función request.*() para evaluar la misma expresión desde el mismo contexto con diferentes valores calc_bars_count, la segunda llamada solicita el mismo número de barras históricas que la primera. Por ejemplo, si un script llama a request.security("AAPL", "", close, calc_bars_count = 3) after it calls request.security("AAPL", "", close, calc_bars_count = 5), la segunda llamada también utilizará cinco barras de datos históricos, y, no tres.
El símbolo de una llamada request.() puede ser heredado si no se especifica con precisión, es decir, si el argumento symbol es una cadena vacía o syminfo.tickerid. Del mismo modo, el intervalo de tiempo de una llamada request.() puede heredarse si el argumento timeframe es una cadena vacía o timeframe.period. Estos valores se toman normalmente del gráfico en el que se está ejecutando el script. Sin embargo, si la función A request.*() se llama desde dentro de la expresión de la función B request.*(), entonces la función A puede heredar los valores de la función B. Para obtener más información, consulte aquí.
Ver también
request.security
syminfo.ticker
syminfo.tickerid
timeframe.period
ticker.new
request.dividends
request.earnings
request.splits
request.financial
request.quandl
request.seed()

Solicita datos de un repositorio GitHub mantenido por el usuario y los devuelve como una serie. Se puede encontrar un tutorial detallado sobre cómo añadir nuevos datos here.
Sintaxis
request.seed(source, symbol, expression, ignore_invalid_symbol, calc_bars_count) → series <type>
Argumentos
source (series string) Nombre del repositorio GitHub.
symbol (series string) Nombre del archivo del repositorio de GitHub que contiene los datos. No se debe incluir la extensión de archivo ".csv".
expression (<arg_expr_type>) Expresión que se calcula y devuelve en función del contexto del símbolo solicitado. Puede ser una variable integrada como close, una expresión como ta.sma(close, 100), una variable no mutable calculada previamente en el script, una llamada a una función que no utilice dibujos de Pine Script®, un array, una matriz o una tupla. No permite variables mutables, a menos que estén contenidas en el cuerpo de una función utilizada en la expresión.
ignore_invalid_symbol (input bool) Determina el comportamiento de la función si no se encuentra el símbolo especificado: si es false, el script se detendrá y lanzará un error de ejecución; si es true, la función devolverá na y la ejecución continuará. Opcional. Por defecto es false.
calc_bars_count (simple int) Si se especifica, la función solo solicitará la cantidad de valores a partir del final del histórico del símbolo y calculará expression como si estos valores fueran los únicos datos disponibles, lo que podría mejorar la velocidad de cálculo en algunos casos. Opcional. El valor por defecto es de 100 000, que es el límite para todos los planes no profesionales de TradingView.
Ejemplo

//@version=5
indicator("BTC Development Activity")

[devAct, devActSMA] = request.seed("seed_crypto_santiment", "BTC_DEV_ACTIVITY", [close, ta.sma(close, 10)])

plot(devAct, "BTC Development Activity")
plot(devActSMA, "BTC Development Activity SMA10", color = color.yellow)
Devoluciones
Serie o tupla de series solicitada, que puede incluir los ID de arrays/matrices.
request.splits()

Solicita los datos sobre los splits para el símbolo especificado.
Sintaxis
request.splits(ticker, field, gaps, lookahead, ignore_invalid_symbol) → series float
Argumentos
ticker (series string) Símbolo. Tenga en cuenta que el símbolo debe incluir un prefijo. Por ejemplo: "NASDAQ:AAPL" en lugar de "AAPL". Usar syminfo.ticker generará un error. En su lugar, utilice syminfo.tickerid.
field (series string) Cadena de entrada. Los valores posibles incluyen: splits.denominator, splits.numerator.
gaps (simple barmerge_gaps) Estrategia de combinación de los datos solicitados (los datos solicitados se combinan automáticamente con los datos OHLC de la serie principal). Valores posibles: barmerge.gaps_on, barmerge.gaps_off. barmerge.gaps_on; los datos solicitados se combinan con posibles gaps (valores de na). barmerge.gaps_off: los datos solicitados se combinan de forma continua sin gaps; todos los gaps se rellenan con los valores existentes más próximos. El valor por defecto es barmerge.gaps_off.
lookahead (simple barmerge_lookahead) Estrategia de combinación para la posición de datos solicitada. Valores posibles: barmerge.lookahead_on, barmerge.lookahead_off. A partir de la versión 3, el valor por defecto es barmerge.lookahead_off. Tenga en cuenta que el comportamiento es el mismo en tiempo real y solo difiere respecto al histórico.
ignore_invalid_symbol (input bool) Parámetro opcional. Determina el comportamiento de la función si no se encuentra el símbolo especificado: si es false, el script se detendrá y devolverá un error de tiempo de ejecución; si es true, la función devolverá na y seguirá ejecutándose. El valor por defecto es false.
Ejemplo

//@version=5
indicator("request.splits")
s1 = request.splits("NASDAQ:BELFA", splits.denominator)
plot(s1)
s2 = request.splits("NASDAQ:BELFA", splits.denominator, gaps=barmerge.gaps_on, lookahead=barmerge.lookahead_on)
plot(s2)
Devoluciones
Serie solicitada o n/a si no existen datos sobre los splits para el símbolo especificado
Ver también
request.earnings
request.dividends
request.security
syminfo.tickerid
runtime.error()

Cuando se ejecuta, se produce un error en el tiempo de ejecución y genera un mensaje de error que se especifica en el argumento message.
Sintaxis
runtime.error(message) → void
Argumentos
message (series string) Mensaje de error.
second()2 sobrecargas

Sintaxis y sobrecargas
second(time) → series int
second(time, timezone) → series int
Argumentos
time (series int) Tiempo UNIX en milisegundos.
Devoluciones
Segundo (en zona horaria de mercado) para el tiempo UNIX proporcionado.
Observaciones
Tiempo UNIX es el número de milisegundos que han transcurrido desde las 00:00:00 UTC, del 1 de enero de 1970.
Ver también
second
time
year
month
dayofmonth
dayofweek
hour
minute
str.contains()3 sobrecargas

Devuelve true si la cadena source contiene la subcadena str; de lo contrario, false.
Sintaxis y sobrecargas
str.contains(source, str) → const bool
str.contains(source, str) → simple bool
str.contains(source, str) → series bool
Argumentos
source (const string) Cadena de origen.
str (const string) Subcadena a buscar.
Ejemplo

//@version=5
indicator("str.contains")
// If the current chart is a continuous futures chart, e.g “BTC1!”, then the function will return true, false otherwise.
var isFutures = str.contains(syminfo.tickerid, "!")
plot(isFutures ? 1 : 0)
Devoluciones
True si en la la cadena str se encuentra el source; de lo contrario, false.
Ver también
str.pos
str.match
str.endswith()3 sobrecargas

Devuelve true si la cadena source termina con la subcadena especificada en str; de lo contrario, false.
Sintaxis y sobrecargas
str.endswith(source, str) → const bool
str.endswith(source, str) → simple bool
str.endswith(source, str) → series bool
Argumentos
source (const string) Cadena de origen.
str (const string) Subcadena a buscar.
Devoluciones
True si la cadena source termina con la subcadena especificada en str; de lo contrario, false.
Ver también
str.startswith
str.format()2 sobrecargas

Convierte la cadena de formato y el valor o los valores en una cadena formateada. Puede contener texto literal y un marcador de posición entre corchetes {} para cada valor a formatear. Cada marcador de posición consta del índice del argumento requerido (comenzando por 0) que lo reemplazará y un especificador de formato opcional. El índice representa la posición de ese argumento en la lista de argumentos str.format.
Sintaxis y sobrecargas
str.format(formatString, arg0, arg1, ...) → simple string
str.format(formatString, arg0, arg1, ...) → series string
Argumentos
formatString (simple string) Formatear cadena.
arg0, arg1, ... (simple int/float/bool/string) Valores a formatear.
Ejemplo

//@version=5
indicator("str.format", overlay=true)
// The format specifier inside the curly braces accepts certain modifiers:
// - Specify the number of decimals to display:
s1 = str.format("{0,number,#.#}", 1.34) // returns: 1.3
label.new(bar_index, close, text=s1)
// - Round a float value to an integer:
s2 = str.format("{0,number,integer}", 1.34) // returns: 1
label.new(bar_index - 1, close, text=s2)
// - Display a number in currency:
s3 = str.format("{0,number,currency}", 1.34) // returns: $1.34
label.new(bar_index - 2, close, text=s3)
// - Display a number as a percentage:
s4 = str.format("{0,number,percent}", 0.5) // returns: 50%
label.new(bar_index - 3, close, text=s4)
// EXAMPLES WITH SEVERAL ARGUMENTS
// returns: Number 1 is not equal to 4
s5 = str.format("Number {0} is not {1} to {2}", 1, "equal", 4)
label.new(bar_index - 4, close, text=s5)
// returns: 1.34 != 1.3
s6 = str.format("{0} != {0, number, #.#}", 1.34)
label.new(bar_index - 5, close, text=s6)
// returns: 1 is equal to 1, but 2 is equal to 2
s7 = str.format("{0, number, integer} is equal to 1, but {1, number, integer} is equal to 2", 1.34, 1.52)
label.new(bar_index - 6, close, text=s7)
// returns: The cash turnover amounted to $1,340,000.00
s8 = str.format("The cash turnover amounted to {0, number, currency}", 1340000)
label.new(bar_index - 7, close, text=s8)
// returns: Expected return is 10% - 20%
s9 = str.format("Expected return is {0, number, percent} - {1, number, percent}", 0.1, 0.2)
label.new(bar_index - 8, close, text=s9)
Devoluciones
La cadena formateada.
Observaciones
Por defecto, los números formateados mostrarán hasta tres decimales sin ceros al final.
La cadena utilizada como argumento formatString puede contener caracteres de comillas simples ('). No obstante, todas las comillas simples de esa cadena deben ir emparejadas para evitar resultados de formato inesperados.
Los corchetes dentro de un patrón sin comillas deben estar equilibrados. Por ejemplo, "ab {0} de" y "ab '}' de" son patrones válidos, pero "ab {0'}' de", "ab } de" y "''{''" no lo son.
str.format_time()

Convierte la timestamp time en una cadena formateada conforme a format y timezone.
Sintaxis
str.format_time(time, format, timezone) → series string
Argumentos
time (series int) Tiempo UNIX, en milisegundos.
format (series string) Una cadena de formato que especifica la representación fecha/hora del time en la cadena devuelta. Todas las letras utilizadas en la cadena, excepto las escapadas por comillas simples ', se consideran tokens de formato y se utilizan como una instrucción de formato. Consulte la sección Observaciones para obtener una lista de los tokens más útiles. Opcional. Por defecto es "aaaa-MM-dd'T'HH:mm:ssZ", que representa la norma estándar ISO 8601.
timezone (series string) Permite adaptar el valor de la hora devuelto a una zona horaria especificada, ya sea en notación UTC/GMT (por ejemplo, "UTC-5", "GMT+0530") o como el nombre de la base de datos de la zona horaria IANA (por ejemplo, "America/New_York"). Es opcional. El valor por defecto es syminfo.timezone.
Ejemplo

//@version=5
indicator("str.format_time")
if timeframe.change("1D")
    formattedTime = str.format_time(time, "yyyy-MM-dd HH:mm", syminfo.timezone)
    label.new(bar_index, high, formattedTime)
Devoluciones
La cadena formateada.
Observaciones
Los tokens M, d, h, H, m y s se pueden duplicar para generar ceros a la izquierda. Por ejemplo, el mes de enero se mostrará como 1 con M, o 01 con MM.
Los tokens de formato más utilizados son:
y: Año. Utilice yy para mostrar los dos últimos dígitos del año o yyyy para mostrar los cuatro. El año 2000 será 00 con yy o 2000 con yyyy.
M: mes. No debe confundirse con la m minúscula, que significa minuto.
d - día del mes.
a - AM/PM postfix.
h: hora en formato de 12 horas. La última hora del día será 11 en este formato.
H: hora en formato de 24 horas. La última hora del día será 23 en este formato.
m - minuto.
s - segundo.
S - fracciones de segundo.
Z - zona horaria, el desfase de HHmm respecto a UTC, precedida por + o -.
str.length()3 sobrecargas

Devuelve un número entero correspondiente a la cantidad de caracteres de la cadena.
Sintaxis y sobrecargas
str.length(string) → const int
str.length(string) → simple int
str.length(string) → series int
Argumentos
string (const string) Cadena de origen.
Devoluciones
Número de caracteres en la cadena de origen.
str.lower()3 sobrecargas

Devuelve una nueva cadena con todas las letras convertidas a minúsculas.
Sintaxis y sobrecargas
str.lower(source) → const string
str.lower(source) → simple string
str.lower(source) → series string
Argumentos
source (const string) Cadena a convertir.
Devoluciones
Nueva cadena con todas las letras convertidas a minúsculas.
Ver también
str.upper
str.match()2 sobrecargas

Devuelve la nueva subcadena de la cadena source si coincide con una expresión regular regex y, en caso contrario, una cadena vacía.
Sintaxis y sobrecargas
str.match(source, regex) → simple string
str.match(source, regex) → series string
Argumentos
source (simple string) Cadena de origen.
regex (simple string) Expresión regular con la que debe coincidir esta cadena.
Ejemplo

//@version=5
indicator("str.match")

s = input.string("It's time to sell some NASDAQ:AAPL!")

// finding first substring that matches regular expression "[\w]+:[\w]+"
var string tickerid = str.match(s, "[\\w]+:[\\w]+")

if barstate.islastconfirmedhistory
    label.new(bar_index, high, text = tickerid) // "NASDAQ:AAPL"
Devoluciones
Nueva subcadena de la cadena source si coincide con una expresión regular regex; de lo contrario, una cadena vacía.
Observaciones
Función que devuelve el primer evento de la expresión regular en la cadena source.
El símbolo de barra invertida "\" en la cadenaregex debe ir seguido de una barra inveertida, por ejemplo "\\d" representa la expresión regular "\d".
Ver también
str.contains
str.substring
str.pos()3 sobrecargas

Devuelve la posición del primer evento de la cadena str en la cadena source; de lo contrario `na'.
Sintaxis y sobrecargas
str.pos(source, str) → const int
str.pos(source, str) → simple int
str.pos(source, str) → series int
Argumentos
source (const string) Cadena de origen.
str (const string) Subcadena a buscar.
Devoluciones
La posición de la cadena str en la cadena source.
Observaciones
La indexación de las cadenas empieza en 0.
Ver también
str.contains
str.match
str.substring
str.repeat()4 sobrecargas

Construye una nueva cadena que contiene la cadena source repetida repeat veces con la separator inyectada entre cada instancia repetida.
Sintaxis y sobrecargas
str.repeat(source, repeat, separator) → const string
str.repeat(source, repeat, separator) → input string
str.repeat(source, repeat, separator) → simple string
str.repeat(source, repeat, separator) → series string
Argumentos
source (const string) Cadena a repetir.
repeat (const int) Número de veces que se repite la cadena source. Debe ser mayor o igual que 0.
separator (const string) Cadena a inyectar entre valores repetidos. Opcional. Por defecto es una cadena vacía.
Ejemplo

//@version=5
indicator("str.repeat")
repeat = str.repeat("?", 3, ",") // Returns "?,?,?"
label.new(bar_index,close,repeat)
Observaciones
Devuelve na si source es na.
str.replace()3 sobrecargas

Devuelve una nueva cadena con la ocurrencia Nth de la cadena target reemplazada por la cadena replacement, donde N se especifica en occurrence.
Sintaxis y sobrecargas
str.replace(source, target, replacement, occurrence) → const string
str.replace(source, target, replacement, occurrence) → simple string
str.replace(source, target, replacement, occurrence) → series string
Argumentos
source (const string) Cadena de origen.
target (const string) Cadena a sustituir.
replacement (const string) Cadena que se añadirá en lugar de la cadena de destino.
occurrence (const int) La enésima (n-th) ocurrencia de la cadena de destino que se va a sustituir. La indexación comienza en 0 para la primera coincidencia. Opcional. El valor por defecto es 0.
Ejemplo

//@version=5
indicator("str.replace")
var source = "FTX:BTCUSD / FTX:BTCEUR"

// Replace first occurrence of "FTX" with "BINANCE" replacement string
var newSource = str.replace(source, "FTX",  "BINANCE", 0)

if barstate.islastconfirmedhistory
    // Display "BINANCE:BTCUSD / FTX:BTCEUR"
    label.new(bar_index, high, text = newSource)
Devoluciones
Cadena procesada.
Ver también
str.replace_all
str.match
str.replace_all()2 sobrecargas

Sustituye cada aparición de la cadena de destino en la cadena de origen por la cadena de sustitución.
Sintaxis y sobrecargas
str.replace_all(source, target, replacement) → simple string
str.replace_all(source, target, replacement) → series string
Argumentos
source (simple string) Cadena de origen.
target (simple string) Cadena a sustituir.
replacement (simple string) Cadena que se sustituirá por cada aparición de la cadena de destino.
Devoluciones
Cadena procesada.
str.split()

Divide una cadena en un array de subcadenas y devuelve el id del array.
Sintaxis
str.split(string, separator) → array<string>
Argumentos
string (series string) Cadena de origen.
separator (series string) Cadena que separa cada subcadena.
Devoluciones
Id de un array de cadenas.
str.startswith()3 sobrecargas

Devuelve true si la cadena source comienza con la subcadena especificada en str; de lo contrario, false.
Sintaxis y sobrecargas
str.startswith(source, str) → const bool
str.startswith(source, str) → simple bool
str.startswith(source, str) → series bool
Argumentos
source (const string) Cadena de origen.
str (const string) Subcadena a buscar.
Devoluciones
True si la cadena source comienza con la subcadena especificada en str; de lo contrario, false.
Ver también
str.endswith
str.substring()6 sobrecargas

Devuelve una nueva cadena que es una subcadena de la cadena source. La subcadena comienza con el carácter en el índice especificado por begin_pos y se extiende hasta 'end_pos - 1' de la cadena source.
Sintaxis y sobrecargas
str.substring(source, begin_pos) → const string
str.substring(source, begin_pos) → simple string
str.substring(source, begin_pos) → series string
str.substring(source, begin_pos, end_pos) → const string
str.substring(source, begin_pos, end_pos) → simple string
str.substring(source, begin_pos, end_pos) → series string
Argumentos
source (const string) Cadena de origen donde extraer la subcadena.
begin_pos (const int) La posición inicial de la subcadena extraída. Es inclusivo (la subcadena extraída incluye el carácter en esa posición).
Ejemplo

//@version=5
indicator("str.substring", overlay = true)
sym= input.symbol("NASDAQ:AAPL")
pos = str.pos(sym, ":")  // Get position of ":" character
tkr= str.substring(sym, pos+1) // "AAPL"
if barstate.islastconfirmedhistory
    label.new(bar_index, high, text = tkr)
Devoluciones
Subcadena extraída de la cadena de origen.
Observaciones
La indexación de cadenas comienza a partir de 0. Si begin_pos es igual aend_pos, la función devuelve una cadena vacía.
Ver también
str.contains
str.pos
str.match
str.tonumber()4 sobrecargas

Convierte un valor representado en string a su equivalente “float”.
Sintaxis y sobrecargas
str.tonumber(string) → const float
str.tonumber(string) → input float
str.tonumber(string) → simple float
str.tonumber(string) → series float
Argumentos
string (const string) Cadena que contiene la representación de un valor numérico entero o de coma flotante.
Devoluciones
Equivalente "float" del valor en string. Si el valor numérico entero o de coma flotante no está debidamente formado, la función devuelve na.
str.tostring()4 sobrecargas

Sintaxis y sobrecargas
str.tostring(value, format) → simple string
str.tostring(value, format) → series string
str.tostring(value) → simple string
str.tostring(value) → series string
Argumentos
value (simple int/float) Valor o ID de la matriz cuyos elementos se han convertido en una cadena.
format (simple string) Cadena de formato (format string). Acepta las constantes de format.*: format.mintick, format.percent, format.volume. Es opcional. El valor por defecto es '#.##########'.
Devoluciones
Representación de cadena del argumento value.
Si el argumento value es una cadena, se devuelve tal cual.
Cuando el value es na, la función devuelve la cadena "NaN".
Observaciones
Cuando sea necesario, el formato de los valores float también redondeará los valores, por ejemplo, str.tostring(3.99, '#') devolverá "4".
Para mostrar los ceros finales, ponga '0' en lugar de '#'. Por ejemplo, '#.000'.
Si se utiliza format.mintick, el valor se redondeará al número más próximo que pueda dividirse por syminfo.mintick sin el resto. La cadena se devuelve con ceros al final.
Si el argumento x es una cadena, se devolverá el mismo valor de la cadena.
Los argumentos de tipo bool devuelven "true" o "false".
Cuando x es igual a na, la función devuelve "NaN".
str.trim()4 sobrecargas

Construye una nueva cadena con todos los espacios en blanco consecutivos y otros caracteres de control (por ejemplo, "\n", "\t", etc.) a la izquierda y a la derecha de source.
Sintaxis y sobrecargas
str.trim(source) → const string
str.trim(source) → input string
str.trim(source) → simple string
str.trim(source) → series string
Argumentos
source (const string) Cadena para recortar.
Ejemplo

//@version=5
indicator("str.trim")
trim = str.trim("    abc    ") // Returns "abc"
label.new(bar_index,close,trim)
Observaciones
Devuelve una cadena vacía ("") si el resultado está vacío tras el recorte o si el source es na.
str.upper()3 sobrecargas

Devuelve una nueva cadena con todas las letras convertidas a mayúsculas.
Sintaxis y sobrecargas
str.upper(source) → const string
str.upper(source) → simple string
str.upper(source) → series string
Argumentos
source (const string) Cadena a convertir.
Devoluciones
Nueva cadena con todas las letras convertidas a mayúsculas.
Ver también
str.lower
strategy()

Esta sentencia de declaración designa el script como una estrategia y establece una serie de propiedades relacionadas con la estrategia.
Sintaxis
strategy(title, shorttitle, overlay, format, precision, scale, pyramiding, calc_on_order_fills, calc_on_every_tick, max_bars_back, backtest_fill_limits_assumption, default_qty_type, default_qty_value, initial_capital, currency, slippage, commission_type, commission_value, process_orders_on_close, close_entries_rule, margin_long, margin_short, explicit_plot_zorder, max_lines_count, max_labels_count, max_boxes_count, calc_bars_count, risk_free_rate, use_bar_magnifier, fill_orders_on_standard_ohlc, max_polylines_count, dynamic_requests, behind_chart) → void
Argumentos
title (const string) Título del script. Se muestra en el gráfico cuando no se utiliza el argumento shorttitle, y se convierte en el título por defecto de la publicación al publicar el script.
shorttitle (const string) Nombre de visualización del script en los gráficos. Si se especifica, sustituirá al argumento title en la mayoría de las ventanas relacionadas con los gráficos. Opcional. Por defecto es el argumento utilizado para title.
overlay (const bool) Si es true, la estrategia se mostrará sobre el gráfico. Si es false, se añadirá en un panel distinto. Las etiquetas específicas de la estrategia que indican entradas y salidas se mostrarán sobre el gráfico principal con independencia de esta configuración. Opcional. El valor por defecto es false.
format (const string) Especifica el formato de los valores mostrados del script. Valores posibles: format.inherit, format.price, format.volume, format.percent. Opcional. El valor por defecto es format.inherit.
precision (const int) Especifica el número de dígitos después del punto flotante de los valores mostrados del script. Debe ser un entero no negativo no mayor que 16. Si format se establece en format.inherit y se especifica precision, el formato se establecerá en format.price. Cuando el parámetro format de la función utiliza format.volume, el parámetro precision no afectará al resultado, ya que las reglas de precisión decimal definidas por format.volume prevalecen sobre otros ajustes de precisión. Es opcional. El valor por defecto se hereda de la precisión del símbolo del gráfico.
scale (const scale_type) Escala de precios utilizada. Valores posibles: scale.right, scale.left, scale.none. El valor scale.none solo puede aplicarse en combinación con overlay = true. Opcional. Por defecto, el script utiliza la misma escala que el gráfico.
pyramiding (const int) Cantidad máxima de entradas permitidas en la misma dirección. Si el valor es 0, solo se puede abrir una orden de entrada en la misma dirección y se rechazan las órdenes de entrada adicionales. Este ajuste también puede modificarse en la pestaña "Configuración/Propiedades" de la estrategia. Es opcional. El valor por defecto es 0.
calc_on_order_fills (const bool) Especifica si la estrategia debe volver a calcularse una vez ejecutada la orden. Si es true, la estrategia vuelva a calcularse una vez ejecutada la orden, en lugar de hacerlo solo cuando se cierra la barra. Este ajuste también se puede modificar en la pestaña "Configuración/Propiedades" de la estrategia. Es opcional. Por defecto es false.
calc_on_every_tick (const bool) Especifica si la estrategia debe recalcularse en cada tick en tiempo real. Si es true, cuando la estrategia se ejecuta en una barra de tiempo real, se recalcula en cada actualización del gráfico. Si es false, la estrategia solo realiza el cálculo cuando se cierra la barra en tiempo real. El argumento utilizado no afecta al cálculo de la estrategia en datos históricos. Este ajuste también puede modificarse en la pestaña "Configuración/Propiedades" de la estrategia. Opcional. El valor por defecto es false.
max_bars_back (const int) Longitud del búfer histórico que mantiene el script para cada variable y función y que determina cuántos valores anteriores se pueden referenciar utilizando el operador de referencia histórica []. El tiempo de ejecución runtime de Pine Script® detecta, de forma automática, el tamaño del búfer necesario. Solo se precisa este parámetro cuando se produce un error en el tiempo de ejecución debido a algún fallo en la detección automática. Puede obtener más información sobre la mecánica subyacente del búfer histórico en nuestro Centro de ayuda. Es opcional. El valor por defecto es 0.
backtest_fill_limits_assumption (const int) Umbral de ejecución de órdenes limitadas en clics. Este tipo de órdenes solo se ejecutan cuando el precio de mercado supera el nivel establecido para la orden limitada en el número de ticks especificado. Es opcional. El valor por defecto es 0.
default_qty_type (const string) Especifica las unidades utilizadas para default_qty_value. Valores posibles: strategy.fixed para contratos/acciones/lotes, strategy.cash para importes en divisas o strategy.percent_of_equity para el porcentaje del capital disponible. Esta configuración también puede modificarse en la pestaña "Configuración/Propiedades" de la estrategia. Opcional. El valor por defecto es strategy.fixed.
default_qty_value (const int/float) Cantidad por defecto a negociar, en unidades determinadas por el argumento utilizado con el parámetro default_qty_type. Este ajuste también puede modificarse en la pestaña "Configuración/Propiedades" de la estrategia. Es opcional. El valor por defecto es 1.
initial_capital (const int/float) Cantidad de fondos inicialmente disponibles para la estrategia a negociar, en unidades de currency. Es opcional. El valor por defecto es 1000000.
currency (const string) Divisa que utiliza la estrategia en los cálculos relacionados con las divisas. Las posiciones de mercado se siguen abriendo mediante la conversión de currency en la divisa del símbolo del gráfico. Los tipos de conversión utilizados se basan en los tipos diarios de los pares FX_IDC del día anterior (en relación con la barra en la que se realiza el cálculo). Este ajuste también puede modificarse en la pestaña "Configuración/Propiedades" de la estrategia. Es opcional. El valor por defecto es currency.NONE, en cuyo caso se utiliza la divisa del gráfico. Valores posibles: una de las constantes del espacio de nombres currency.*, por ejemplo currency.USD.
slippage (const int) Deslizamiento (slippage) expresado en ticks. Este valor se suma o se resta al precio de ejecución de las órdenes de mercado/stop para que dicho precio sea menos favorable para la estrategia. Por ejemplo, si syminfo.mintick es 0,01 y el slippage se establece en 5, una orden de mercado a largo entrará a 5 * 0,01 = 0,05 puntos por encima del precio real. Este ajuste también se puede modificar en la pestaña "Configuración/Propiedades" de la estrategia. Es opcional. El valor por defecto es 0.
commission_type (const string) Determina lo que expresa el número pasado al commission_value: strategy.commission.percent es para un porcentaje del volumen efectivo de la orden, strategy.commission.cash_per_contract es para divisa por contrato, strategy.commission.cash_per_order es para divisa por orden. Este ajuste también puede modificarse en la pestaña "Ajustes/Propiedades" de la estrategia. Opcional. El valor por defecto es strategy.commission.percent.
commission_value (const int/float) Comisión aplicada a las órdenes de la estrategia en unidades determinadas por el argumento pasado al parámetro commission_type. Este ajuste también se puede modificar en la pestaña "Configuración/Propiedades" de la estrategia. Es opcional. El valor por defecto es 0.
process_orders_on_close (const bool) Cuando se establece en true, genera un intento adicional de ejecución de órdenes una vez que se cierra la barra y se completan los cálculos de la estrategia. Si las órdenes son de mercado, el emulador del broker las ejecuta antes de que se abra la siguiente barra. Si las órdenes dependen del precio, solo se ejecutan si se cumplen las condiciones de dicho precio. Esta opción resulta útil si desea cerrar posiciones en la barra actual. La configuración también se puede modificar en la pestaña "Configuración/Propiedades" de la estrategia. Es opcional. Por defecto es false.
close_entries_rule (const string) Determina el orden en que se cierran las negociaciones. Los valores posibles son: "FIFO" (primero en entrar, primero en salir) cuando la orden de salida más antigua cierra la orden de entrada más reciente o "ANY" si las órdenes se cierran en función del parámetro from_entry de la función strategy.exit. El sistema "FIFO" solo se puede utilizar con acciones, futuros y forex estadounidenses (Norma de cumplimiento 2-43b de la NFA), mientras que "ANY" permite el forex en países no estadounidenses. Es opcional. El valor por defecto es "FIFO".
margin_long (const int/float) El margen para posiciones a largo es el porcentaje del precio de compra de un valor que debe cubrirse con una garantía o depósito en efectivo para las posiciones a largo. Tiene que ser un número no negativo. La lógica utilizada para simular las llamadas de margen se explica en el Centro de ayuda. Este ajuste también se puede modificar en la pestaña "Configuración/Propiedades" de la estrategia. Es opcional. El valor por defecto es 0, en cuyo caso la estrategia no impone límite alguno respecto al tamaño de la posición.
margin_short (const int/float) El margen para posiciones a corto es el porcentaje del precio de compra de un valor que debe cubrirse con una garantía o depósito en efectivo para las posiciones a corto. Tiene que ser un número no negativo. La lógica utilizada para simular las llamadas de margen se explica en el Centro de ayuda. Este ajuste también se puede modificar en la pestaña "Configuración/Propiedades" de la estrategia. Es opcional. El valor por defecto es 0, en cuyo caso la estrategia no impone límite alguno respecto al tamaño de la posición.
explicit_plot_zorder (const bool) Especifica el orden en el que se dibujan los plots, fills y hlines del script. Si es true, los trazados se dibujan por orden de aparición en el código del script, es decir, los más recientes por encima de los anteriores. Esto solo es aplicable a las funciones plot*(), fill y hline. Es opcional. El valor por defecto es false.
max_lines_count (const int) Cantidad de dibujos que se muestran en el último line. Valores posibles: de 1 a 500. Es opcional. El valor por defecto es 50.
max_labels_count (const int) Cantidad de dibujos que se muestran en el último label. Valores posibles: de 1 a 500. Es opcional. El valor por defecto es 50.
max_boxes_count (const int) Cantidad de dibujos que se muestran en el último box. Valores posibles: de 1 a 500. Es opcional. El valor por defecto es 50.
calc_bars_count (const int) Limita el cálculo inicial de un script al último número de barras especificado. Cuando se especifique, se incluirá un campo de "Barras calculadas" en la sección "Cálculo" de la pestaña "Configuración/Entradas" del script. Es opcional. El valor por defecto es 0, en cuyo caso el script se ejecuta en todas las barras disponibles.
risk_free_rate (const int/float) La tasa de rendimiento sin riesgo es la variación porcentual anual del valor de una inversión con riesgo mínimo o nulo. Se utiliza para calcular los ratios Sharpe y Sortino. Opcional. El valor predeterminado es 2.
use_bar_magnifier (const bool) Cuando es true, el Emulador del broker utiliza datos de menor intervalo de tiempo durante el backtesting del histórico para conseguir resultados más realistas. Es opcional. El valor por defecto es false. Solo las cuentas Premium tienen acceso a esta funcionalidad.
fill_orders_on_standard_ohlc (const bool) Cuando true, fuerza a las estrategias que se ejecutan en gráficos Heikin Ashi a completar órdenes utilizando precios OHLC reales, para obtener resultados más realistas. Opcional. Por defecto es false.
max_polylines_count (const int) Cantidad de dibujos que se muestran en el último polyline. Valores posibles: de 1 a 100. El recuento es aproximado; es posible que se muestren más dibujos que los especificados. Opcional. El valor por defecto es 50.
dynamic_requests (const bool) Especifica si el script puede llamar dinámicamente a funciones del espacio de nombres request.*(). Las llamadas dinámicas request.*() están permitidas dentro de los ámbitos locales de estructuras condicionales (por ejemplo, if), bucles (por ejemplo, for) y funciones exportadas. Además, estas llamadas permiten argumentos «en serie» para muchos de sus parámetros. Opcional. Por defecto es false. Para obtener más información consulte la sección Solicitudes dinámicas del Manual de usuario.
behind_chart (const bool) Controla si los trazados y dibujos del script en el panel del gráfico principal aparecen detrás de la pantalla del gráfico (si es true), o delante de ella (si es false). Opcional. Por defecto es true.
Ejemplo

//@version=5
strategy("My strategy", overlay = true, margin_long = 100, margin_short = 100)

// Enter long by market if current open is greater than previous high.
if open > high[1]
    strategy.entry("Long", strategy.long, 1)
// Generate a full exit bracket (profit 10 points, loss 5 points per contract) from the entry named "Long".
strategy.exit("Exit", "Long", profit = 10, loss = 5)
Observaciones
Puede obtener más información sobre las estrategias en nuestro Manual del usuario.
Cada script de estrategia debe tener una llamada a strategy.
Las estrategias que utilizan el parámetro calc_on_every_tick = true se pueden calcular de forma diferente en las barras históricas y en las de tiempo real, lo que provoca un repintado.
Las estrategias siempre utilizan los precios del gráfico para entrar y salir de las posiciones. Utilizarlas en tipos de gráficos atípicos (Heikin Ashi, Renko, etc.) puede dar lugar a unos resultados confusos, ya que sus precios son engañosos. Por lo tanto, no se recomienda hacer backtesting en los gráficos atípicos.
Ver también
indicator
library
strategy.cancel()

Cancela una orden pendiente o no ejecutada con un identificador específico. Si varias órdenes pendientes comparten el mismo identificador, al llamar a este comando con ese identificador como argumento id se cancelan todas. Si un script llama a este comando con un id que representa el identificador de una orden ejecutada, no tiene ningún efecto.
Este comando es más útil cuando se trabaja con órdenes basadas en precios (por ejemplo, órdenes con límite). Las llamadas a este comando también pueden cancelar órdenes de mercado, pero solo si se ejecutan en los mismos ticks que los comandos de emisión de órdenes.
Sintaxis
strategy.cancel(id) → void
Argumentos
id (series string) Identificador de la orden pendiente de cancelar.
Ejemplo

//@version=5
strategy(title = "Order cancellation demo")

conditionForBuy = open > high[1]
if conditionForBuy
    strategy.entry("Long", strategy.long, 1, limit = low) // Enter long using limit order at low price of current bar if `conditionForBuy` is `true`.
if not conditionForBuy
    strategy.cancel("Long") // Cancel the entry order with name "Long" if `conditionForBuy` is `false`.
strategy.cancel_all()

Cancela todas las órdenes pendientes o no ejecutadas, con independencia de sus identificadores.
Este comando es más útil cuando se trabaja con órdenes basadas en precios (por ejemplo, órdenes con límite). Las llamadas a este comando también pueden cancelar órdenes de mercado, pero solo si se ejecutan en los mismos ticks que los comandos de emisión de órdenes.
Sintaxis
strategy.cancel_all() → void
Ejemplo

//@version=5
strategy(title = "Cancel all orders demo")
conditionForBuy1 = open > high[1]
if conditionForBuy1
    strategy.entry("Long entry 1", strategy.long, 1, limit = low) // Enter long using a limit order if `conditionForBuy1` is `true`.
conditionForBuy2 = conditionForBuy1 and open[1] > high[2]
float lowest2 = ta.lowest(low, 2)
if conditionForBuy2
    strategy.entry("Long entry 2", strategy.long, 1, limit = lowest2) // Enter long using a limit order if `conditionForBuy2` is `true`.
conditionForStopTrading = open < lowest2
if conditionForStopTrading
    strategy.cancel_all() // Cancel both limit orders if `conditionForStopTrading` is `true`.
strategy.close()

Crea una orden para salir de la parte de una posición abierta por órdenes de entrada con un identificador específico. Si varias entradas en la posición comparten el mismo identificador, las órdenes de este comando se aplica a todas ellas, empezando por la primera operación abierta en la que se haya llamado dicho identificador como argumento id.
Este comando siempre genera órdenes de mercado. Para salir de una posición utilizando órdenes en función del precio (por ejemplo, órdenes stop-loss), utilice el comando strategy.exit.
Sintaxis
strategy.close(id, comment, qty, qty_percent, alert_message, immediately, disable_alert) → void
Argumentos
id (series string) Identificador de entrada de las operaciones abiertas a cerrar.
comment (series string) Opcional. Notas adicionales sobre la orden ejecutada. Si el valor no es una cadena vacía, el Simulador de estrategias y el gráfico muestran este texto para la orden en lugar del identificador de salida generado de forma automática. El valor por defecto es una cadena vacía.
qty (series int/float) Opcional. El número de contratos/lotes/acciones/unidades a cerrar cuando se emite una orden de salida. Si se especifica, el comando utiliza este valor en lugar de qty_percent para determinar el tamaño de la orden. El valor por defecto es na, lo que significa que el tamaño de la orden depende del valor de qty_percent.
qty_percent (series int/float) Opcional. Valor entre 0 y 100 que representa el porcentaje de la cantidad de la operación abierta a cerrar cuando se ejecuta una orden de salida. El cálculo del porcentaje depende del tamaño total de las operaciones abiertas con el identificador de entrada id. El comando ignora este parámetro si el valor de qty no es na. El valor por defecto es 100.
alert_message (series string) Opcional. Texto personalizado para la alerta que se activa cuando se completa una orden. Si el campo «Mensaje» del cuadro de diálogo «Crear alerta» contiene el marcador de posición {{strategy.order.alert_message}}, el mensaje de alerta sustituye al marcador de posición por este texto. Por defecto es una cadena vacía.
immediately (series bool) Opcional. Si es true, la orden de cierre se ejecuta en el mismo tick en el que la estrategia la coloca, ignorando las propiedades de la estrategia que restringen la ejecución al tick de apertura de la barra siguiente. Por defecto es false.
disable_alert (series bool) Opcional. Si true cuando el comando crea una orden, la estrategia no activa una alerta cuando esa orden se ejecuta. Este parámetro acepta un valor de «serie», lo que significa que los usuarios pueden controlar qué órdenes activan alertas cuando se ejecutan. El valor por defecto es false.
Ejemplo

//@version=5
strategy("Partial close strategy",  margin_long = 100, margin_short = 100)

// Calculate a 14-bar and 28-bar moving average of `close` prices.
float sma14 = ta.sma(close, 14)
float sma28 = ta.sma(close, 28)

// Place a market order to enter a long position when `sma14` crosses over `sma28`.
if ta.crossover(sma14, sma28)
    strategy.entry("My Long Entry ID", strategy.long)

// Place a market order to close the long trade when `sma14` crosses under `sma28`. 
if ta.crossunder(sma14, sma28)
    strategy.close("My Long Entry ID", "50% market close", qty_percent = 50)

// Plot the position size.
plot(strategy.position_size)
Observaciones
Cuando una posición consta de varias operaciones abiertas y el close_entries_rule en la sentencia de declaración strategy es «FIFO» (por defecto), una llamada strategy.close sale de la posición empezando por la primera operación abierta. Este comportamiento se aplica incluso si el valor id es el identificador de entrada de diferentes operaciones abiertas. Sin embargo, en ese caso, el tamaño máximo de la orden de salida sigue dependiendo de las operaciones abiertas por órdenes con el identificador id. Para obtener más información, consulte aquí la sección correspondiente de nuestro Manual de usuario.
strategy.close_all()2 sobrecargas

Crea una orden para cerrar completamente una posición abierta, con independencia de los identificadores de las órdenes de entrada que la abrieron o añadieron.
Este comando siempre genera órdenes de mercado. Para salir de una posición utilizando órdenes en función del precio (por ejemplo, órdenes stop-loss), utilice el comando strategy.exit.
Sintaxis y sobrecargas
strategy.close_all(comment, alert_message) → void
strategy.close_all(comment, alert_message, immediately, disable_alert) → void
Argumentos
comment (series string) Opcional. Notas adicionales sobre la orden ejecutada. Si el valor no es una cadena vacía, el Simulador de estrategias y el gráfico muestran este texto para la orden en lugar del identificador de salida generado de forma automática. El valor por defecto es una cadena vacía.
alert_message (series string) Opcional. Texto personalizado para la alerta que se activa cuando se completa una orden. Si el campo «Mensaje» del cuadro de diálogo «Crear alerta» contiene el marcador de posición {{strategy.order.alert_message}}, el mensaje de alerta sustituye al marcador de posición por este texto. Por defecto es una cadena vacía.
Ejemplo

//@version=5
strategy("Multi-entry close strategy",  margin_long = 100, margin_short = 100)

// Calculate a 14-bar and 28-bar moving average of `close` prices.
float sma14 = ta.sma(close, 14)
float sma28 = ta.sma(close, 28)

// Place a market order to enter a long trade every time `sma14` crosses over `sma28`.
if ta.crossover(sma14, sma28)
    strategy.order("My Long Entry ID " + str.tostring(strategy.opentrades), strategy.long)

// Place a market order to close the entire position every 500 bars. 
if bar_index % 500 == 0
    strategy.close_all()

// Plot the position size.
plot(strategy.position_size)
strategy.closedtrades.commission()

Devuelve la suma de las comisiones de entrada y salida pagadas en la operación cerrada, expresada en strategy.account_currency.
Sintaxis
strategy.closedtrades.commission(trade_num) → series float
Argumentos
trade_num (series int) El número de la operación cerrada. El número de la primera operación es cero.
Ejemplo

//@version=5
strategy("`strategy.closedtrades.commission` Example", commission_type = strategy.commission.percent, commission_value = 0.1)

// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars.
if bar_index % 15 == 0
    strategy.entry("Long", strategy.long)
if bar_index % 20 == 0
    strategy.close("Long")

// Plot total fees for the latest closed trade.
plot(strategy.closedtrades.commission(strategy.closedtrades - 1))
Ver también
strategy
strategy.opentrades.commission
strategy.closedtrades.entry_bar_index()

Devuelve el bar_index de la entrada de la operación cerrada.
Sintaxis
strategy.closedtrades.entry_bar_index(trade_num) → series int
Argumentos
trade_num (series int) El número de la operación cerrada. El número de la primera operación es cero.
Ejemplo

//@version=5
strategy("strategy.closedtrades.entry_bar_index Example")
// Enter long trades on three rising bars; exit on two falling bars.
if ta.rising(close, 3)
    strategy.entry("Long", strategy.long)
if ta.falling(close, 2)
    strategy.close("Long")
// Function that calculates the average amount of bars in a trade.
avgBarsPerTrade() =>
    sumBarsPerTrade = 0
    for tradeNo = 0 to strategy.closedtrades - 1
        // Loop through all closed trades, starting with the oldest.
        sumBarsPerTrade += strategy.closedtrades.exit_bar_index(tradeNo) - strategy.closedtrades.entry_bar_index(tradeNo) + 1
    result = nz(sumBarsPerTrade / strategy.closedtrades)
plot(avgBarsPerTrade())
Ver también
strategy.closedtrades.exit_bar_index
strategy.opentrades.entry_bar_index
strategy.closedtrades.entry_comment()

Devuelve el mensaje de comentarios respecto a la entrada de la operación cerrada, o na si no hay ninguna entrada con este trade_num.
Sintaxis
strategy.closedtrades.entry_comment(trade_num) → series string
Argumentos
trade_num (series int) El número de la operación cerrada. El número de la primera operación es cero.
Ejemplo

//@version=5
strategy("`strategy.closedtrades.entry_comment()` Example", overlay = true)

stopPrice = open * 1.01

longCondition = ta.crossover(ta.sma(close, 14), ta.sma(close, 28))

if (longCondition)
    strategy.entry("Long", strategy.long, stop = stopPrice, comment = str.tostring(stopPrice, "#.####"))
    strategy.exit("EXIT", trail_points = 1000, trail_offset = 0)

var testTable = table.new(position.top_right, 1, 3, color.orange, border_width = 1)

if barstate.islastconfirmedhistory or barstate.isrealtime
    table.cell(testTable, 0, 0, 'Last closed trade:')
    table.cell(testTable, 0, 1, "Order stop price value: " + strategy.closedtrades.entry_comment(strategy.closedtrades - 1))
    table.cell(testTable, 0, 2, "Actual Entry Price: " + str.tostring(strategy.closedtrades.entry_price(strategy.closedtrades - 1)))
Ver también
strategy
strategy.entry
strategy.closedtrades
strategy.closedtrades.entry_id()

Devuelve el id de la entrada de la operación cerrada.
Sintaxis
strategy.closedtrades.entry_id(trade_num) → series string
Argumentos
trade_num (series int) El número de la operación cerrada. El número de la primera operación es cero.
Ejemplo

//@version=5
strategy("strategy.closedtrades.entry_id Example", overlay = true)

// Enter a short position and close at the previous to last bar.
if bar_index == 1
    strategy.entry("Short at bar #" + str.tostring(bar_index), strategy.short)
if bar_index == last_bar_index - 2
    strategy.close_all()
    
// Display ID of the last entry position.
if barstate.islastconfirmedhistory
    label.new(last_bar_index, high, "Last Entry ID is: " + strategy.closedtrades.entry_id(strategy.closedtrades - 1))
Devoluciones
Devuelve el id de la entrada de la operación cerrada.
Observaciones
Función que devuelve na si trade_num no está en el rango de 0 a strategy.closedtrades-1.
Ver también
strategy.closedtrades.entry_bar_index
strategy.closedtrades.entry_price
strategy.closedtrades.entry_time
strategy.closedtrades.entry_price()

Devuelve el precio de la entrada de la operación cerrada.
Sintaxis
strategy.closedtrades.entry_price(trade_num) → series float
Argumentos
trade_num (series int) El número de la operación cerrada. El número de la primera operación es cero.
Ejemplo

//@version=5
strategy("strategy.closedtrades.entry_price Example 1")

// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars.
if bar_index % 15 == 0
    strategy.entry("Long", strategy.long)
if bar_index % 20 == 0
    strategy.close("Long")

// Return the entry price for the latest  entry.
entryPrice = strategy.closedtrades.entry_price(strategy.closedtrades - 1)

plot(entryPrice, "Long entry price")
Ejemplo

// Calculates the average profit percentage for all closed trades.
//@version=5
strategy("strategy.closedtrades.entry_price Example 2")

// Strategy calls to create single short and long trades
if bar_index == last_bar_index - 15
    strategy.entry("Long Entry",  strategy.long)
else if bar_index == last_bar_index - 10
    strategy.close("Long Entry")
    strategy.entry("Short", strategy.short)
else if bar_index == last_bar_index - 5
    strategy.close("Short")

// Calculate profit for both closed trades.
profitPct = 0.0
for tradeNo = 0 to strategy.closedtrades - 1
    entryP = strategy.closedtrades.entry_price(tradeNo)
    exitP = strategy.closedtrades.exit_price(tradeNo)
    profitPct += (exitP - entryP) / entryP * strategy.closedtrades.size(tradeNo) * 100
    
// Calculate average profit percent for both closed trades.
avgProfitPct = nz(profitPct / strategy.closedtrades)

plot(avgProfitPct)
Ver también
strategy.closedtrades.entry_price
strategy.closedtrades.exit_price
strategy.closedtrades.size
strategy.closedtrades
strategy.closedtrades.entry_time()

Devuelve el tiempo UNIX de entrada de la operación cerrada, expresada en milisegundos.
Sintaxis
strategy.closedtrades.entry_time(trade_num) → series int
Argumentos
trade_num (series int) El número de la operación cerrada. El número de la primera operación es cero.
Ejemplo

//@version=5
strategy("strategy.closedtrades.entry_time Example", overlay = true)

// Enter long trades on three rising bars; exit on two falling bars.
if ta.rising(close, 3)
    strategy.entry("Long", strategy.long)
if ta.falling(close, 2)
    strategy.close("Long")

// Calculate the average trade duration 
avgTradeDuration() =>
    sumTradeDuration = 0
    for i = 0 to strategy.closedtrades - 1
        sumTradeDuration += strategy.closedtrades.exit_time(i) - strategy.closedtrades.entry_time(i)
    result = nz(sumTradeDuration / strategy.closedtrades)

// Display average duration converted to seconds and formatted using 2 decimal points
if barstate.islastconfirmedhistory
    label.new(bar_index, high, str.tostring(avgTradeDuration() / 1000, "#.##") + " seconds")
Ver también
strategy.opentrades.entry_time
strategy.closedtrades.exit_time
time
strategy.closedtrades.exit_bar_index()

Devuelve el bar_index de la salida de una operación cerrada.
Sintaxis
strategy.closedtrades.exit_bar_index(trade_num) → series int
Argumentos
trade_num (series int) El número de la operación cerrada. El número de la primera operación es cero.
Ejemplo

//@version=5
strategy("strategy.closedtrades.exit_bar_index Example 1")

// Strategy calls to place a single short trade. We enter the trade at the first bar and exit the trade at 10 bars before the last chart bar.
if bar_index == 0
    strategy.entry("Short",  strategy.short)
if bar_index == last_bar_index - 10
    strategy.close("Short")

// Calculate the amount of bars since the last closed trade.
barsSinceClosed = strategy.closedtrades > 0 ? bar_index - strategy.closedtrades.exit_bar_index(strategy.closedtrades - 1) : na

plot(barsSinceClosed, "Bars since last closed trade")
Ejemplo

// Calculates the average amount of bars per trade.
//@version=5
strategy("strategy.closedtrades.exit_bar_index Example 2")

// Enter long trades on three rising bars; exit on two falling bars.
if ta.rising(close, 3)
    strategy.entry("Long", strategy.long)
if ta.falling(close, 2)
    strategy.close("Long")

// Function that calculates the average amount of bars per trade.
avgBarsPerTrade() =>
    sumBarsPerTrade = 0
    for tradeNo = 0 to strategy.closedtrades - 1
        // Loop through all closed trades, starting with the oldest.
        sumBarsPerTrade += strategy.closedtrades.exit_bar_index(tradeNo) - strategy.closedtrades.entry_bar_index(tradeNo) + 1
    result = nz(sumBarsPerTrade / strategy.closedtrades)

plot(avgBarsPerTrade())
Ver también
bar_index
last_bar_index
strategy.closedtrades.exit_comment()

Devuelve el mensaje de comentario de la salida de la operación cerrada, o na si no hay ninguna entrada con este trade_num.
Sintaxis
strategy.closedtrades.exit_comment(trade_num) → series string
Argumentos
trade_num (series int) El número de la operación cerrada. El número de la primera operación es cero.
Ejemplo

//@version=5
strategy("`strategy.closedtrades.exit_comment()` Example", overlay = true)

longCondition = ta.crossover(ta.sma(close, 14), ta.sma(close, 28))
if (longCondition)
    strategy.entry("Long", strategy.long)
    strategy.exit("Exit", stop = open * 0.95, limit = close * 1.05, trail_points = 100, trail_offset = 0, comment_profit = "TP", comment_loss = "SL", comment_trailing = "TRAIL")

exitStats() =>
    int slCount = 0
    int tpCount = 0
    int trailCount = 0
    
    if strategy.closedtrades > 0
        for i = 0 to strategy.closedtrades - 1
            switch strategy.closedtrades.exit_comment(i)
                "TP"    => tpCount    += 1
                "SL"    => slCount    += 1
                "TRAIL" => trailCount += 1
    [slCount, tpCount, trailCount]

var testTable = table.new(position.top_right, 1, 4, color.orange, border_width = 1)

if barstate.islastconfirmedhistory
    [slCount, tpCount, trailCount] = exitStats()
    table.cell(testTable, 0, 0, "Closed trades (" + str.tostring(strategy.closedtrades) +") stats:")
    table.cell(testTable, 0, 1, "Stop Loss: " + str.tostring(slCount))
    table.cell(testTable, 0, 2, "Take Profit: " + str.tostring(tpCount))
    table.cell(testTable, 0, 3, "Trailing Stop: " + str.tostring(trailCount))
Ver también
strategy
strategy.exit
strategy.close
strategy.closedtrades
strategy.closedtrades.exit_id()

Devuelve el id de la salida de la operación cerrada.
Sintaxis
strategy.closedtrades.exit_id(trade_num) → series string
Argumentos
trade_num (series int) El número de la operación cerrada. El número de la primera operación es cero.
Ejemplo

//@version=5
strategy("strategy.closedtrades.exit_id Example", overlay = true)

// Strategy calls to create single short and long trades
if bar_index == last_bar_index - 15
    strategy.entry("Long Entry",  strategy.long)
else if bar_index == last_bar_index - 10
    strategy.entry("Short Entry", strategy.short)
    
// When a new open trade is detected then we create the exit strategy corresponding with the matching entry id
// We detect the correct entry id by determining if a position is long or short based on the position quantity
if ta.change(strategy.opentrades) != 0
    posSign = strategy.opentrades.size(strategy.opentrades - 1)
    strategy.exit(posSign > 0 ? "SL Long Exit" : "SL Short Exit", strategy.opentrades.entry_id(strategy.opentrades - 1), stop = posSign > 0 ? high - ta.tr : low + ta.tr)

// When a new closed trade is detected then we place a label above the bar with the exit info
if ta.change(strategy.closedtrades) != 0
    msg = "Trade closed by: " + strategy.closedtrades.exit_id(strategy.closedtrades - 1)
    label.new(bar_index, high + (3 * ta.tr), msg)
Devoluciones
Devuelve el id de la salida de la operación cerrada.
Observaciones
Función que devuelve na si trade_num no está en el rango de 0 a strategy.closedtrades-1.
Ver también
strategy.closedtrades.exit_bar_index
strategy.closedtrades.exit_price
strategy.closedtrades.exit_time
strategy.closedtrades.exit_price()

Devuelve el precio de salida de la operación cerrada.
Sintaxis
strategy.closedtrades.exit_price(trade_num) → series float
Argumentos
trade_num (series int) El número de la operación cerrada. El número de la primera operación es cero.
Ejemplo

//@version=5
strategy("strategy.closedtrades.exit_price Example 1")

// We are creating a long trade every 5 bars
if bar_index % 5 == 0
    strategy.entry("Long",  strategy.long)
strategy.close("Long")

// Return the exit price from the latest closed trade.
exitPrice = strategy.closedtrades.exit_price(strategy.closedtrades - 1)

plot(exitPrice, "Long exit price")
Ejemplo

// Calculates the average profit percentage for all closed trades.
//@version=5
strategy("strategy.closedtrades.exit_price Example 2")

// Strategy calls to create single short and long trades.
if bar_index == last_bar_index - 15
    strategy.entry("Long Entry",  strategy.long)
else if bar_index == last_bar_index - 10
    strategy.close("Long Entry")
    strategy.entry("Short", strategy.short)
else if bar_index == last_bar_index - 5
    strategy.close("Short")

// Calculate profit for both closed trades.
profitPct = 0.0
for tradeNo = 0 to strategy.closedtrades - 1
    entryP = strategy.closedtrades.entry_price(tradeNo)
    exitP = strategy.closedtrades.exit_price(tradeNo)
    profitPct += (exitP - entryP) / entryP * strategy.closedtrades.size(tradeNo) * 100
    
// Calculate average profit percent for both closed trades.
avgProfitPct = nz(profitPct / strategy.closedtrades)

plot(avgProfitPct)
Ver también
strategy.closedtrades.entry_price
strategy.closedtrades.exit_time()

Devuelve el tiempo UNIX de salida de la operación cerrada, expresada en milisegundos.
Sintaxis
strategy.closedtrades.exit_time(trade_num) → series int
Argumentos
trade_num (series int) El número de la operación cerrada. El número de la primera operación es cero.
Ejemplo

//@version=5
strategy("strategy.closedtrades.exit_time Example 1")

// Enter long trades on three rising bars; exit on two falling bars.
if ta.rising(close, 3)
    strategy.entry("Long", strategy.long)
if ta.falling(close, 2)
    strategy.close("Long")

// Calculate the average trade duration. 
avgTradeDuration() =>
    sumTradeDuration = 0
    for i = 0 to strategy.closedtrades - 1
        sumTradeDuration += strategy.closedtrades.exit_time(i) - strategy.closedtrades.entry_time(i)
    result = nz(sumTradeDuration / strategy.closedtrades)

// Display average duration converted to seconds and formatted using 2 decimal points.
if barstate.islastconfirmedhistory
    label.new(bar_index, high, str.tostring(avgTradeDuration() / 1000, "#.##") + " seconds")
Ejemplo

// Reopens a closed trade after X seconds.
//@version=5
strategy("strategy.closedtrades.exit_time Example 2")

// Strategy calls to emulate a single long trade at the first bar.
if bar_index == 0
    strategy.entry("Long", strategy.long)

reopenPositionAfter(timeSec) =>
    if strategy.closedtrades > 0
        if time - strategy.closedtrades.exit_time(strategy.closedtrades - 1) >= timeSec * 1000
            strategy.entry("Long", strategy.long)

// Reopen last closed position after 120 sec.                
reopenPositionAfter(120)

if ta.change(strategy.opentrades) != 0
    strategy.exit("Long", stop = low * 0.9, profit = high * 2.5)
Ver también
strategy.closedtrades.entry_time
strategy.closedtrades.max_drawdown()

Devuelve la bajada máxima de la negociación cerrada, es decir, la pérdida máxima posible durante la operación, expresada en strategy.account_currency.
Sintaxis
strategy.closedtrades.max_drawdown(trade_num) → series float
Argumentos
trade_num (series int) El número de la operación cerrada. El número de la primera operación es cero.
Ejemplo

//@version=5
strategy("`strategy.closedtrades.max_drawdown` Example")

// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars.
if bar_index % 15 == 0
    strategy.entry("Long", strategy.long)
if bar_index % 20 == 0
    strategy.close("Long")

// Get the biggest max trade drawdown value from all of the closed trades.
maxTradeDrawDown() =>
    maxDrawdown = 0.0
    for tradeNo = 0 to strategy.closedtrades - 1
        maxDrawdown := math.max(maxDrawdown, strategy.closedtrades.max_drawdown(tradeNo))
    result = maxDrawdown

plot(maxTradeDrawDown(), "Biggest max drawdown")
Observaciones
Función que devuelve na si trade_num no se encuentra en el rango 0 a strategy.closedtrades - 1.
Ver también
strategy.opentrades.max_drawdown
strategy.max_drawdown
strategy.closedtrades.max_drawdown_percent()

Devuelve la pérdida máxima de la negociación cerrada, es decir, la mayor pérdida posible durante la negociación, expresada en porcentaje y calculada mediante la fórmula: Lowest Value During Trade / (Entry Price x Quantity) * 100.
Sintaxis
strategy.closedtrades.max_drawdown_percent(trade_num) → series float
Argumentos
trade_num (series int) El número de la operación cerrada. El número de la primera operación es cero.
Ver también
strategy.closedtrades.max_drawdown
strategy.max_drawdown
strategy.closedtrades.max_runup()

Devuelve la subida máxima de la negociación cerrada, es decir, el beneficio máximo posible durante la operación, expresado en strategy.account_currency.
Sintaxis
strategy.closedtrades.max_runup(trade_num) → series float
Argumentos
trade_num (series int) El número de la operación cerrada. El número de la primera operación es cero.
Ejemplo

//@version=5
strategy("`strategy.closedtrades.max_runup` Example")

// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars.
if bar_index % 15 == 0
    strategy.entry("Long", strategy.long)
if bar_index % 20 == 0
    strategy.close("Long")

// Get the biggest max trade runup value from all of the closed trades.
maxTradeRunUp() =>
    maxRunup = 0.0
    for tradeNo = 0 to strategy.closedtrades - 1
        maxRunup := math.max(maxRunup, strategy.closedtrades.max_runup(tradeNo))
    result = maxRunup

plot(maxTradeRunUp(), "Max trade runup")
Ver también
strategy.opentrades.max_runup
strategy.max_runup
strategy.closedtrades.max_runup_percent()

Devuelve el máximo aumento de capital de la negociación cerrada, es decir, el beneficio máximo posible durante la operación, expresado en porcentaje y calculado mediante la fórmula: Highest Value During Trade / (Entry Price x Quantity) * 100.
Sintaxis
strategy.closedtrades.max_runup_percent(trade_num) → series float
Argumentos
trade_num (series int) El número de la operación cerrada. El número de la primera operación es cero.
Ver también
strategy.closedtrades.max_runup
strategy.max_runup
strategy.closedtrades.profit()

Devuelve el beneficio/pérdida de la operación cerrada, expresado en strategy.account_currency. Las pérdidas se expresan como valores negativos.
Sintaxis
strategy.closedtrades.profit(trade_num) → series float
Argumentos
trade_num (series int) El número de la operación cerrada. El número de la primera operación es cero.
Ejemplo

//@version=5
strategy("`strategy.closedtrades.profit` Example")

// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars.
if bar_index % 15 == 0
    strategy.entry("Long", strategy.long)
if bar_index % 20 == 0
    strategy.close("Long")

// Calculate average gross profit by adding the difference between gross profit and commission.
avgGrossProfit() =>
    sumGrossProfit = 0.0
    for tradeNo = 0 to strategy.closedtrades - 1
        sumGrossProfit += strategy.closedtrades.profit(tradeNo) - strategy.closedtrades.commission(tradeNo)
    result = nz(sumGrossProfit / strategy.closedtrades)
    
plot(avgGrossProfit(), "Average gross profit")
Ver también
strategy.opentrades.profit
strategy.closedtrades.commission
strategy.closedtrades.profit_percent()

Devuelve el valor de beneficios/pérdidas de la negociación cerrada, expresado en porcentaje. Las pérdidas se expresan como valores negativos.
Sintaxis
strategy.closedtrades.profit_percent(trade_num) → series float
Argumentos
trade_num (series int) El número de la operación cerrada. El número de la primera operación es cero.
Ver también
strategy.closedtrades.profit
strategy.closedtrades.size()

Devuelve la dirección y el número de contratos negociados en una operación cerrada. Si el valor es > 0, la posición de mercado era larga. Por el contrario, si el valor es < 0, la posición era corta.
Sintaxis
strategy.closedtrades.size(trade_num) → series float
Argumentos
trade_num (series int) El número de la operación cerrada. El número de la primera operación es cero.
Ejemplo

//@version=5
strategy("`strategy.closedtrades.size` Example 1")

// We calculate the max amt of shares we can buy.
amtShares = math.floor(strategy.equity / close)
// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars
if bar_index % 15 == 0
    strategy.entry("Long", strategy.long, qty = amtShares)
if bar_index % 20 == 0
    strategy.close("Long")

// Plot the number of contracts traded in the last closed trade.     
plot(strategy.closedtrades.size(strategy.closedtrades - 1), "Number of contracts traded")
Ejemplo

// Calculates the average profit percentage for all closed trades.
//@version=5
strategy("`strategy.closedtrades.size` Example 2")

// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars.
if bar_index % 15 == 0
    strategy.entry("Long", strategy.long)
if bar_index % 20 == 0
    strategy.close("Long")


// Calculate profit for both closed trades.
profitPct = 0.0
for tradeNo = 0 to strategy.closedtrades - 1
    entryP = strategy.closedtrades.entry_price(tradeNo)
    exitP = strategy.closedtrades.exit_price(tradeNo)
    profitPct += (exitP - entryP) / entryP * strategy.closedtrades.size(tradeNo) * 100
    
// Calculate average profit percent for both closed trades.
avgProfitPct = nz(profitPct / strategy.closedtrades)

plot(avgProfitPct)
Ver también
strategy.opentrades.size
strategy.position_size
strategy.closedtrades
strategy.opentrades
strategy.convert_to_account()

Convierte el valor de la divisa del símbolo negociado en el gráfico (syminfo.currency) a la divisa utilizada por la estrategia (strategy.account_currency).
Sintaxis
strategy.convert_to_account(value) → series float
Argumentos
value (series int/float) Valor a convertir.
Ejemplo

//@version=5
strategy("`strategy.convert_to_account` Example 1", currency = currency.EUR)

plot(close, "Close price using default currency")
plot(strategy.convert_to_account(close), "Close price converted to strategy currency")
Ejemplo

// Calculates the "Buy and hold return" using your account's currency.
//@version=5
strategy("`strategy.convert_to_account` Example 2", currency = currency.EUR)

dateInput = input.time(timestamp("20 Jul 2021 00:00 +0300"), "From Date", confirm = true)

buyAndHoldReturnPct(fromDate) =>
    if time >= fromDate
        money = close * syminfo.pointvalue
        var initialBal = strategy.convert_to_account(money)
        (strategy.convert_to_account(money) - initialBal) / initialBal * 100
        
plot(buyAndHoldReturnPct(dateInput))
Ver también
strategy
strategy.convert_to_symbol
strategy.convert_to_symbol()

Convierte el valor de la divisa utilizada por la estrategia (strategy.account_currency) a la divisa del símbolo negociado en el gráfico (syminfo.currency).
Sintaxis
strategy.convert_to_symbol(value) → series float
Argumentos
value (series int/float) Valor a convertir.
Ejemplo

//@version=5
strategy("`strategy.convert_to_symbol` Example", currency = currency.EUR)

// Calculate the max qty we can buy using current chart's currency.
calcContracts(accountMoney) =>
    math.floor(strategy.convert_to_symbol(accountMoney) / syminfo.pointvalue / close)

// Return max qty we can buy using 300 euros
qt = calcContracts(300)

// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars using our custom qty.
if bar_index % 15 == 0
    strategy.entry("Long", strategy.long, qty = qt)
if bar_index % 20 == 0
    strategy.close("Long")
Ver también
strategy
strategy.convert_to_account
strategy.default_entry_qty()

Calcula la cantidad por defecto, en unidades, de una orden de entrada de strategy.entry o strategy.order si se llenara en el valor fill_price especificado. El cálculo depende de varias propiedades de la estrategia, incluyendo default_qty_type, default_qty_value, currency y otros parámetros de la función strategy y su representación en la pestaña "Propiedades" de la configuración de la estrategia.
Sintaxis
strategy.default_entry_qty(fill_price) → series float
Argumentos
fill_price (series int/float) El precio de ejecución para calcular la cantidad por defecto de la orden.
Ejemplo

//@version=5
strategy("Supertrend Strategy", overlay = true, default_qty_type = strategy.percent_of_equity, default_qty_value = 15)

//@variable The length of the ATR calculation.
atrPeriod = input(10, "ATR Length")
//@variable The ATR multiplier.
factor = input.float(3.0, "Factor", step = 0.01)
//@variable The tick offset of the stop order.
stopOffsetInput = input.int(100, "Tick offset for entry stop")

// Get the direction of the SuperTrend.
[_, direction] = ta.supertrend(factor, atrPeriod)

if ta.change(direction) < 0
    //@variable The stop price of the entry order.
    stopPrice = close + syminfo.mintick * stopOffsetInput
    //@variable The expected default fill quantity at the `stopPrice`. This value may not reflect actual qty of the filled order, because fill price may be different.
    calculatedQty = strategy.default_entry_qty(stopPrice)
    strategy.entry("My Long Entry Id", strategy.long, stop = stopPrice)
    label.new(bar_index, stopPrice, str.format("Stop set at {0}\nExpected qty at {0}: {1}", math.round_to_mintick(stopPrice), calculatedQty))

if ta.change(direction) > 0
    strategy.close_all()
Observaciones
Esta función no tiene en cuenta las posiciones abiertas simuladas por una estrategia. Por ejemplo, si un script de estrategia tiene una posición abierta de una orden a largo con un qty de 10 unidades, usar la función strategy.entry para simular una orden a corto con un qty de 5 hará que el script venda 15 unidades para invertir la posición. Esta función seguirá devolviendo 5 en tal caso, ya que no considera una operación abierta.
Este valor representa la cantidad calculada por defecto de una orden.
Los comandos para cursar órdenes pueden anular el valor por defecto pasando explícitamente un nuevo valor qty en la llamada a la función.
strategy.entry()

Crea una nueva orden para abrir o añadir a una posición. Si existe una orden sin cumplimentar con el mismo id, una llamada a este comando modifica esa orden.
El tipo de orden resultante depende de los parámetros limit y stop. Si la llamada no contiene argumentos limit o stop, crea una orden de mercado que se ejecuta en el siguiente tick. Si la llamada especifica un valor limit pero no un valor stop, coloca una orden límite que se ejecuta después de que el precio de mercado alcance el valor limit o un precio mejor (más bajo para órdenes de compra y más alto para órdenes de venta). Si la llamada especifica un valor stop pero no un valor limit, emite una orden stop que se ejecuta después de que el precio de mercado alcance el valor stop o un precio peor (más alto para órdenes de compra y más bajo para órdenes de venta). Si la llamada contiene argumentos limit y stop, crea una orden stop-límite, que genera una orden limitada al precio limit solo después de que el precio de mercado alcance el valor stop o un precio peor.
Las órdenes de este comando, a diferencia de las de strategy.order, se ven afectadas por el parámetro pyramiding de la sentencia de declaración strategy. La piramidación especifica el número de entradas abiertas simultáneas permitidas por posición. Por ejemplo, con pyramiding = 3, la estrategia puede tener hasta tres operaciones abiertas, y el comando no puede crear órdenes para abrir más operaciones hasta que al menos una existente se cierre.
Por defecto, cuando una estrategia ejecuta una orden desde este comando en la dirección opuesta a la posición de mercado actual, invierte esa posición. Por ejemplo, si hay una posición larga abierta de cinco acciones, una orden de este comando con un qty de 5 y un direction de strategy.short activa la venta de 10 acciones para cerrar la posición larga y abrir una nueva posición corta de cinco acciones. Los usuarios pueden cambiar este comportamiento especificando una dirección permitida con la función strategy.risk_allow_entry_in.
Sintaxis
strategy.entry(id, direction, qty, limit, stop, oca_name, oca_type, comment, alert_message, disable_alert) → void
Argumentos
id (series string) Identificador de la orden, que corresponde a un identificador de entrada en las operaciones de la estrategia después de que la orden se complete. Si la estrategia abre una nueva posición después de ejecutar la orden, el identificador de la orden se convierte en el valor strategy.position_entry_name. Los comandos de la estrategia pueden hacer referencia al identificador de la orden para cancelar o modificar órdenes pendientes y generar órdenes de salida para operaciones abiertas específicas. El Simulador de estrategias y el gráfico muestran el identificador de la orden a menos que el comando especifique un valor comment.
direction (series strategy_direction) La dirección de la operación. Valores posibles: strategy.long para una operación larga, strategy.short para una corta.
qty (series int/float) Opcional. Número de contratos/acciones/lotes/unidades en la operación abierta resultante cuando la orden se ejecuta. El valor por defecto es na, lo que significa que la orden utiliza los parámetros default_qty_type y default_qty_value de la sentencia de declaración strategy para determinar la cantidad.
limit (series int/float) Opcional. Precio límite de la orden. Si se especifica, la orden crea una orden limit o stop-limit, dependiendo de si también se indica el valor stop. El valor por defecto es na, lo que significa que la orden resultante no es del tipo limit o stop-limit.
stop (series int/float) Opcional. Precio stop de la orden. Si se especifica, la orden crea una orden stop o stop-limit, dependiendo de si también se indica el valor limit. El valor por defecto es na, lo que significa que la orden resultante no es del tipo stop o stop-limit.
oca_name (series string) Opcional. Nombre del grupo de órdenes Una-cancela-todas (OCA). Cuando se ejecuta una orden pendiente con los mismos parámetros oca_name y oca_type, esa orden afecta a todas las no ejecutadas del grupo. El valor por defecto es una cadena vacía, lo que significa que la orden no pertenece a un grupo OCA.
oca_type (input string) Opcional. Especifica cómo se comporta una orden no ejecutada cuando se ejecuta otra pendiente con los mismos valores oca_name y oca_type. Valores posibles: strategy.oca.cancel, strategy.oca.reduce, strategy.oca.none. El valor por defecto es strategy.oca.none.
comment (series string) Opcional. Notas adicionales sobre la orden ejecutada. Si el valor no es una cadena vacía, el Simulador de estrategias y el gráfico muestran este texto para la orden en lugar del id especificado. El valor por defecto es una cadena vacía.
alert_message (series string) Opcional. Texto personalizado para la alerta que se activa cuando se completa una orden. Si el campo «Mensaje» del cuadro de diálogo «Crear alerta» contiene el marcador de posición {{strategy.order.alert_message}}, el mensaje de alerta sustituye al marcador de posición por este texto. Por defecto es una cadena vacía.
disable_alert (series bool) Opcional. Si true cuando el comando crea una orden, la estrategia no activa una alerta cuando esa orden se ejecuta. Este parámetro acepta un valor de «serie», lo que significa que los usuarios pueden controlar qué órdenes activan alertas cuando se ejecutan. El valor por defecto es false.
Ejemplo

//@version=5
strategy("Market order strategy", overlay = true, margin_long = 100, margin_short = 100)

// Calculate a 14-bar and 28-bar moving average of `close` prices.
float sma14 = ta.sma(close, 14)
float sma28 = ta.sma(close, 28)

// Place a market order to close the short trade and enter a long position when `sma14` crosses over `sma28`.
if ta.crossover(sma14, sma28)
    strategy.entry("My Long Entry ID", strategy.long)

// Place a market order to close the long trade and enter a short position when `sma14` crosses under `sma28`.
if ta.crossunder(sma14, sma28)
    strategy.entry("My Short Entry ID", strategy.short)
Ejemplo

//@version=5
strategy("Limit order strategy", overlay=true, margin_long=100, margin_short=100)

//@variable The distance from the `close` price for each limit order.
float limitOffsetInput = input.int(100, "Limit offset, in ticks", 1) * syminfo.mintick

//@function Draws a label and line at the specified `price` to visualize a limit order's level. 
drawLimit(float price, bool isLong) =>
    color col = isLong ? color.blue : color.red
    label.new(
         bar_index, price, (isLong ? "Long" : "Short") + " limit order created", 
         style = label.style_label_right, color = col, textcolor = color.white
     )
    line.new(bar_index, price, bar_index + 1, price, extend = extend.right, style = line.style_dashed, color = col)

//@function Stops the `l` line from extending further.
method stopExtend(line l) =>
    l.set_x2(bar_index)
    l.set_extend(extend.none)

// Initialize two `line` variables to reference limit line IDs.
var line longLimit  = na
var line shortLimit = na

// Calculate a 14-bar and 28-bar moving average of `close` prices.
float sma14 = ta.sma(close, 14)
float sma28 = ta.sma(close, 28)

if ta.crossover(sma14, sma28)
    // Cancel any unfilled sell orders with the specified ID.
    strategy.cancel("My Short Entry ID")
    //@variable The limit price level. Its value is `limitOffsetInput` ticks below the current `close`.
    float limitLevel = close - limitOffsetInput
    // Place a long limit order to close the short trade and enter a long position at the `limitLevel`.
    strategy.entry("My Long Entry ID", strategy.long, limit = limitLevel)
    // Make new drawings for the long limit and stop extending the `shortLimit` line.
    longLimit := drawLimit(limitLevel, isLong = true)
    shortLimit.stopExtend()
    
if ta.crossunder(sma14, sma28)
    // Cancel any unfilled buy orders with the specified ID.
    strategy.cancel("My Long Entry ID")
    //@variable The limit price level. Its value is `limitOffsetInput` ticks above the current `close`.
    float limitLevel = close + limitOffsetInput
    // Place a short limit order to close the long trade and enter a short position at the `limitLevel`.
    strategy.entry("My Short Entry ID", strategy.short, limit = limitLevel)
    // Make new drawings for the short limit and stop extending the `shortLimit` line.
    shortLimit := drawLimit(limitLevel, isLong = false)
    longLimit.stopExtend()
strategy.exit()

Crea órdenes en función del precio para salir de una posición abierta. Si existen órdenes de salida no ejecutadas con el mismo id, las llamadas a este comando modifican esas órdenes. Este comando puede generar más de un tipo de orden de salida, dependiendo de los parámetros especificados. Sin embargo, no crea órdenes de mercado. Para salir de una posición con una orden de mercado, utilice strategy.close o strategy.close_all.
Si una llamada a este comando contiene un argumento profit o limit, crea órdenes take-profit para salir de las operaciones aplicables en los niveles de precios determinados o valores mejores (más altos para operaciones largas y más bajos para las cortas). Si la llamada contiene argumentos loss o stop, crea órdenes stop-loss para salir de las operaciones aplicables en los niveles determinados o valores peores (más bajos para operaciones largas y más altos para las cortas). Al llamar a este comando con argumentos profit o limit y loss o stop se crea un corchete de orden con ambos tipos de orden.
Este comando puede crear órdenes trailing stop cuando su llamada especifica un argumento trail_price o trail_points y un argumento trail_offset. Una orden trailing stop se activa cuando el precio se mueve trail_points ticks más allá del precio de entrada o alcanza el nivel trail_price. Una vez activado, el stop sigue trail_offset ticks por detrás del precio de mercado cada vez que el beneficio de la operación alcanza un nuevo máximo. El stop no se mueve cuando la operación no alcanza un nuevo mejor valor.
Cada llamada a este comando reserva una parte de la posición a cerrar hasta que la estrategia complete o cancele sus órdenes. Por ejemplo, si hay una posición abierta de 50 contratos y una llamada a strategy.exit especifica un qty de 20, las órdenes de esa llamada reservan 20 contratos de la posición. Una segunda llamada puede cerrar un máximo de 30 contratos, incluso si su qty es 50 y una de sus órdenes se ejecuta primero. Este comportamiento no afecta a las órdenes de otros comandos, como strategy.close o strategy.order.
Si una llamada a este comando se produce antes de la ejecución de una orden de entrada creada, la estrategia espera y no crea las órdenes de salida hasta después de que se ejecute la orden de entrada.
Sintaxis
strategy.exit(id, from_entry, qty, qty_percent, profit, limit, loss, stop, trail_price, trail_points, trail_offset, oca_name, comment, comment_profit, comment_loss, comment_trailing, alert_message, alert_profit, alert_loss, alert_trailing, disable_alert) → void
Argumentos
id (series string) Identificador de las órdenes, que corresponde a uno de salida en las operaciones de la estrategia tras emitirse una orden. Los comandos de estrategia pueden hacer referencia al identificador de orden para cancelar o modificar órdenes de salida pendientes. El Probador de estrategias y el gráfico muestran el identificador de la orden a menos que el comando incluya un argumento comment* que se aplique a la orden ejecutada.
from_entry (series string) Opcional. Identificador de la orden de entrada de la operación de la que se desea salir. Si hay más de una operación abierta con el identificador de entrada especificado, el comando genera órdenes de salida para todas las entradas anteriores o durante la llamada. El valor por defecto es una cadena vacía, lo que significa que el comando genera órdenes de salida para todas las operaciones abiertas hasta el cierre de la posición.
qty (series int/float) Opcional. Número de contratos/lotes/acciones/unidades a cerrar cuando se emite una orden de salida. Si se especifica, el comando utiliza este valor en lugar de qty_percent para determinar el tamaño de la orden. Las órdenes de salida reservan esta cantidad de la posición, lo que significa que otras llamadas a este comando no pueden cerrar esta porción hasta que la estrategia se ejecute o cancele esas órdenes. El valor por defecto es na, lo que significa que el tamaño de la orden depende del valor de qty_percent.
qty_percent (series int/float) Opcional. Valor entre 0 y 100 representando el porcentaje de la cantidad de la operación abierta a cerrar cuando una orden de salida se ejecuta. Las órdenes de salida reservan este porcentaje de las operaciones abiertas aplicables, lo que significa que otras llamadas a este comando no pueden cerrar esta porción hasta que la estrategia ejecute o cancele esas órdenes. El cálculo del porcentaje depende del tamaño total de las operaciones abiertas aplicables sin considerar la cantidad reservada de otras llamadas a strategy.exit. El comando ignora este parámetro si el valor de qty no es na. El valor por defecto es 100.
profit (series int/float) Opcional. Distancia take-profit, expresada en ticks. Si se especifica, el comando crea una orden limitada para salir de la operación a profit ticks del precio de entrada en la dirección favorable. La orden se ejecuta al precio calculado o a un valor mejor. El comando ignora este parámetro si el valor de limit no es na. El valor por defecto es na.
limit (series int/float) Opcional. Precio de take-profit. Si se especifica, el comando ignora el parámetro profit y crea una orden limitada para salir de la operación a este precio o a un valor mejor. El valor por defecto es na.
loss (series int/float) Opcional. Distancia de stop-loss, expresada en ticks. Si se especifica, el comando crea una orden stop para salir de la operación a loss ticks del precio de entrada en la dirección desfavorable. La orden se ejecuta al precio calculado o a un valor peor. El comando ignora este parámetro si el valor de stop no es na. El valor por defecto es na.
stop (series int/float) Opcional. Precio de stop-loss. Si se especifica, el comando ignora el parámetro loss y crea una orden stop para salir de la operación a este precio o a un valor peor. El valor por defecto es na.
trail_price (series int/float) Opcional. Precio del nivel de activación del trailing stop. Si se especifica, el comando ignora el parámetro trail_points y activa una orden trailing stop cuando el precio de mercado alcanza este valor. El valor inicial del trailing stop está a trail_offset ticks del nivel de activación en la dirección desfavorable. El valor por defecto es na.
trail_points (series int/float) Opcional. Distancia de activación del trailing stop, expresada en ticks. Si este valor es positivo, el comando crea una orden trailing stop cuando el precio de mercado se aleja de trail_points ticks del precio de entrada de la operación en la dirección favorable. Si este valor es negativo, el comando crea una orden trailing stop cuando el precio de mercado se mueve a trail_points ticks absolutos del precio de entrada en la dirección opuesta de la operación. El comando ignora este parámetro si el valor de trail_price no es na. El valor por defecto es na.
trail_offset (series int/float) Opcional. Desplazamiento del trailing stop. Cuando el precio de mercado alcanza el nivel de activación determinado por el parámetro trail_price o trail_points, el comando crea un trailing stop con un valor inicial de trail_offset ticks de distancia de ese nivel en la dirección desfavorable. Tras su activación, el trailing stop se mueve hacia el precio de mercado cada vez que el beneficio de la operación alcanza un nuevo máximo, manteniendo la distancia especificada detrás del mejor precio. El valor por defecto es na.
oca_name (series string) Opcional. Nombre del grupo Una-cancela-todas (OCA) al que pertenecen las órdenes take-profit, stop-loss y trailing stop del comando. Todas las órdenes de este comando son del tipo strategy.oca.reduce OCA. Cuando se ejecuta una orden de este tipo OCA con el mismo oca_name, la estrategia reduce los tamaños de otras órdenes no emitidas en el grupo OCA por la cantidad ejecutada. El valor por defecto es una cadena vacía, lo que significa que la estrategia asigna el nombre OCA de forma automática, y las órdenes resultantes no pueden reducir o ser reducidas por las órdenes de otros comandos.
comment (series string) Opcional. Notas adicionales sobre la orden ejecutada. Si el valor no es una cadena vacía, el Probador de estrategias y el gráfico muestran este texto para la orden en lugar del id especificado. El comando ignora este valor si la llamada incluye un argumento para un parámetro comment_* que se aplica a la orden. El valor por defecto es una cadena vacía.
comment_profit (series string) Opcional. Notas adicionales sobre la orden ejecutada. Si el valor no es una cadena vacía, el Probador de estrategias y el gráfico muestran este texto para la orden en lugar del id o comment especificado. Este comentario solo se aplica a las órdenes take-profit del comando creadas utilizando el parámetro profit o limit. El valor por defecto es una cadena vacía.
comment_loss (series string) Opcional. Notas adicionales sobre la orden ejecutada. Si el valor no es una cadena vacía, el Probador de estrategias y el gráfico muestran este texto para la orden en lugar del id o comment especificado. Este comentario solo se aplica a las órdenes stop-loss del comando creadas utilizando el parámetro loss o stop. El valor por defecto es una cadena vacía.
comment_trailing (series string) Opcional. Notas adicionales sobre la orden ejecutada. Si el valor no es una cadena vacía, el Probador de estrategias y el gráfico muestran este texto para la orden en lugar del id o comment especificado. Este comentario solo se aplica a las órdenes trailing stop del comando creadas utilizando los parámetros trail_price o trail_points y trail_offset. El valor por defecto es una cadena vacía.
alert_message (series string) Opcional. Texto personalizado para la alerta que se activa cuando se ejecuta una orden. Si el campo «Mensaje» del cuadro de diálogo «Crear alerta» contiene el marcador de posición {{strategy.order.alert_message}}, el mensaje de alerta sustituye al marcador de posición por este texto. La orden ignora este valor si la llamada incluye un argumento para el otro parámetro alert_* que se aplica a la orden. El valor por defecto es una cadena vacía.
alert_profit (series string) Opcional. Texto personalizado para la alerta que se activa cuando se completa una orden. Si el campo «Mensaje» del cuadro de diálogo «Crear alerta» contiene el marcador de posición {{strategy.order.alert_message}}, el mensaje de alerta sustituye al marcador de posición por este texto. Este mensaje solo se aplica a las órdenes take-profit creadas con el parámetro profit o limit. El valor por defecto es una cadena vacía.
alert_loss (series string) Opcional. Texto personalizado para la alerta que se activa cuando se ejecuta una orden. Si el campo «Mensaje» del cuadro de diálogo «Crear Alerta» contiene el marcador de posición {{strategy.order.alert_message}}, el mensaje de alerta sustituye al marcador de posición por este texto. Este mensaje solo se aplica a las órdenes stop-loss del comando creadas utilizando el parámetro loss o stop. El valor por defecto es una cadena vacía.
alert_trailing (series string) Opcional. Texto personalizado para la alerta que se activa cuando se completa una orden. Si el campo «Mensaje» del cuadro de diálogo «Crear Alerta» contiene el marcador de posición {{strategy.order.alert_message}}, el mensaje de alerta sustituye al marcador de posición por este texto. Este mensaje solo se aplica a las órdenes trailing stop creadas utilizando los parámetros trail_price o trail_points y trail_offset. El valor por defecto es una cadena vacía.
disable_alert (series bool) Opcional. Si true cuando el comando crea una orden, la estrategia no activa una alerta cuando esa orden se ejecuta. Este parámetro acepta un valor de «serie», lo que significa que los usuarios pueden controlar qué órdenes activan alertas cuando se ejecutan. El valor por defecto es false.
Ejemplo

//@version=5
strategy("Exit bracket strategy", overlay = true, margin_long = 100, margin_short = 100)

// Inputs that define the profit and loss amount of each trade as a tick distance from the entry price.
int profitDistanceInput = input.int(100, "Profit distance, in ticks", 1)
int lossDistanceInput   = input.int(100, "Loss distance, in ticks", 1)

// Variables to track the take-profit and stop-loss price. 
var float takeProfit = na
var float stopLoss   = na

// Calculate a 14-bar and 28-bar moving average of `close` prices.
float sma14 = ta.sma(close, 14)
float sma28 = ta.sma(close, 28)

if ta.crossover(sma14, sma28) and strategy.opentrades == 0
    // Place a market order to enter a long position.
    strategy.entry("My Long Entry ID", strategy.long)
    // Place a take-profit and stop-loss order when the entry order fills. 
    strategy.exit("My Long Exit ID", "My Long Entry ID", profit = profitDistanceInput, loss = lossDistanceInput)

if ta.change(strategy.opentrades) == 1
    //@variable The long entry price.
    float entryPrice = strategy.opentrades.entry_price(0)
    // Update the `takeProfit` and `stopLoss` values.
    takeProfit := entryPrice + profitDistanceInput * syminfo.mintick
    stopLoss   := entryPrice - lossDistanceInput * syminfo.mintick

if ta.change(strategy.closedtrades) == 1
    // Reset the `takeProfit` and `stopLoss`.
    takeProfit := na
    stopLoss   := na

// Plot the `takeProfit` and `stopLoss`.
plot(takeProfit, "Take-profit level", color.green, 2, plot.style_linebr)
plot(stopLoss, "Stop-loss level", color.red, 2, plot.style_linebr)
Ejemplo

//@version=5
strategy("Trailing stop strategy", overlay = true, margin_long = 100, margin_short = 100)

//@variable The distance required to activate the trailing stop.
float activationDistanceInput = input.int(100, "Trail activation distance, in ticks") * syminfo.mintick 
//@variable The number of ticks the trailing stop follows behind the price as it reaches new peaks. 
int trailDistanceInput = input.int(100, "Trail distance, in ticks")

//@function Draws a label and line at the specified `price` to visualize a trailing stop order's activation level. 
drawActivation(float price) =>
    label.new(
         bar_index, price, "Activation level", style = label.style_label_right, 
         color = color.gray, textcolor = color.white
     )
    line.new(
         bar_index, price, bar_index + 1, price, extend = extend.right, style = line.style_dashed, color = color.gray
     )

//@function Stops the `l` line from extending further.
method stopExtend(line l) =>
    l.set_x2(bar_index)
    l.set_extend(extend.none)

// The activation line, active trailing stop price, and active trailing stop flag. 
var line activationLine     = na
var float trailingStopPrice = na
var bool isActive           = false

if bar_index % 100 == 0 and strategy.opentrades == 0
    trailingStopPrice := na
    isActive          := false
    // Place a market order to enter a long position.
    strategy.entry("My Long Entry ID", strategy.long)
    //@variable The activation level's price. 
    float activationPrice = close + activationDistanceInput
    // Create a trailing stop order that activates the defined number of ticks above the entry price.
    strategy.exit(
         "My Long Exit ID", "My Long Entry ID", trail_price = activationPrice, trail_offset = trailDistanceInput,
         oca_name = "Exit"
     )
    // Create new drawings at the `activationPrice`.
    activationLine := drawActivation(activationPrice)

// Logic for trailing stop visualization. 
if strategy.opentrades == 1
    // Stop extending the `activationLine` when the stop activates.
    if not isActive and high > activationLine.get_price(bar_index)
        isActive := true
        activationLine.stopExtend()
    // Update the `trailingStopPrice` while the trailing stop is active. 
    if isActive
        float offsetPrice = high - trailDistanceInput * syminfo.mintick
        trailingStopPrice := math.max(nz(trailingStopPrice, offsetPrice), offsetPrice)

// Close the trade with a market order if the trailing stop does not activate before the next 300th bar. 
if not isActive and bar_index % 300 == 0
    strategy.close_all("Market close")

// Reset the `trailingStopPrice` and `isActive` flags when the trade closes, and stop extending the `activationLine`.
if ta.change(strategy.closedtrades) > 0
    if not isActive
        activationLine.stopExtend()
    trailingStopPrice := na
    isActive          := false

// Plot the `trailingStopPrice`.
plot(trailingStopPrice, "Trailing stop", color.red, 3, plot.style_linebr)
Observaciones
Una sola llamada al comando strategy.exit puede generar órdenes de salida para varias entradas en una posición abierta, dependiendo del valor from_entry de la llamada. Si la llamada no incluye un argumento from_entry, crea órdenes de salida para todas las operaciones abiertas, incluso las abiertas después de la llamada, hasta que se cierre la posición. Consulte esta sección de nuestro Manual del usuario para obtener más información.
Cuando una posición consta de varias operaciones abiertas, y el close_entries_rule en la sentencia de declaración strategy es «FIFO» (por defecto), las órdenes de una llamada strategy.exit salen de la posición empezando por la primera operación abierta. Este comportamiento se aplica incluso si el valor from_entry es el identificador de entrada de diferentes operaciones abiertas. Sin embargo, en ese caso, el tamaño máximo de las órdenes de salida sigue dependiendo de las operaciones abiertas por órdenes con el identificador from_entry. Para más información, consulte aquí el apartado correspondiente del Manual de usuario.
Si una llamada a strategy.exit incluye argumentos para crear órdenes stop-loss y trailing stop, el comando emite solo la orden que se supone que debe ejecutarse primero, porque ambas órdenes son de tipo «stop».
strategy.opentrades.commission()

Devuelve la suma de las comisiones de entrada y salida pagadas en la operación abierta, expresada en strategy.account_currency.
Sintaxis
strategy.opentrades.commission(trade_num) → series float
Argumentos
trade_num (series int) Número de la operación abierta. El número de la primera operación es cero.
Ejemplo

// Calculates the gross profit or loss for the current open position.
//@version=5
strategy("`strategy.opentrades.commission` Example", commission_type = strategy.commission.percent, commission_value = 0.1)

// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars.
if bar_index % 15 == 0
    strategy.entry("Long", strategy.long)
if bar_index % 20 == 0
    strategy.close("Long")

// Calculate gross profit or loss for open positions only.
tradeOpenGrossPL() =>
    sumOpenGrossPL = 0.0
    for tradeNo = 0 to strategy.opentrades - 1
        sumOpenGrossPL += strategy.opentrades.profit(tradeNo) - strategy.opentrades.commission(tradeNo)
    result = sumOpenGrossPL
    
plot(tradeOpenGrossPL())
Ver también
strategy
strategy.closedtrades.commission
strategy.opentrades.entry_bar_index()

Devuelve el bar_index de la entrada de la operación abierta.
Sintaxis
strategy.opentrades.entry_bar_index(trade_num) → series int
Argumentos
trade_num (series int) Número de la operación abierta. El número de la primera operación es cero.
Ejemplo

// Wait 10 bars and then close the position.
//@version=5
strategy("`strategy.opentrades.entry_bar_index` Example")

barsSinceLastEntry() =>
    strategy.opentrades > 0 ? bar_index - strategy.opentrades.entry_bar_index(strategy.opentrades - 1) : na

// Enter a long position if there are no open positions.
if strategy.opentrades == 0
    strategy.entry("Long",  strategy.long)

// Close the long position after 10 bars. 
if barsSinceLastEntry() >= 10
    strategy.close("Long")
Ver también
strategy.closedtrades.entry_bar_index
strategy.closedtrades.exit_bar_index
strategy.opentrades.entry_comment()

Devuelve el mensaje de comentarios respecto a la entrada de la operación abierta, o na si no hay ninguna entrada con este trade_num.
Sintaxis
strategy.opentrades.entry_comment(trade_num) → series string
Argumentos
trade_num (series int) Número de la operación abierta. El número de la primera operación es cero.
Ejemplo

//@version=5
strategy("`strategy.opentrades.entry_comment()` Example", overlay = true)

stopPrice = open * 1.01

longCondition = ta.crossover(ta.sma(close, 14), ta.sma(close, 28))

if (longCondition)
    strategy.entry("Long", strategy.long, stop = stopPrice, comment = str.tostring(stopPrice, "#.####"))

var testTable = table.new(position.top_right, 1, 3, color.orange, border_width = 1)

if barstate.islastconfirmedhistory or barstate.isrealtime
    table.cell(testTable, 0, 0, 'Last entry stats')
    table.cell(testTable, 0, 1, "Order stop price value: " + strategy.opentrades.entry_comment(strategy.opentrades - 1))
    table.cell(testTable, 0, 2, "Actual Entry Price: " + str.tostring(strategy.opentrades.entry_price(strategy.opentrades - 1)))
Ver también
strategy
strategy.entry
strategy.opentrades
strategy.opentrades.entry_id()

Devuelve el id de la entrada de la operación abierta.
Sintaxis
strategy.opentrades.entry_id(trade_num) → series string
Argumentos
trade_num (series int) Número de la operación abierta. El número de la primera operación es cero.
Ejemplo

//@version=5
strategy("`strategy.opentrades.entry_id` Example", overlay = true)

// We enter a long position when 14 period sma crosses over 28 period sma.
// We enter a short position when 14 period sma crosses under 28 period sma.
longCondition = ta.crossover(ta.sma(close, 14), ta.sma(close, 28))
shortCondition = ta.crossunder(ta.sma(close, 14), ta.sma(close, 28))

// Strategy calls to enter a long or short position when the corresponding condition is met.
if longCondition
    strategy.entry("Long entry at bar #" + str.tostring(bar_index), strategy.long)
if shortCondition
    strategy.entry("Short entry at bar #" + str.tostring(bar_index), strategy.short)

// Display ID of the latest open position.
if barstate.islastconfirmedhistory
    label.new(bar_index, high + (2 * ta.tr),  "Last opened position is \n " + strategy.opentrades.entry_id(strategy.opentrades - 1))
Devoluciones
Devuelve el id de la entrada de la operación abierta.
Observaciones
Función que devuelve na si trade_num no está en el rango: 0 a strategy.opentrades-1.
Ver también
strategy.opentrades.entry_bar_index
strategy.opentrades.entry_price
strategy.opentrades.entry_time
strategy.opentrades.entry_price()

Devuelve el precio de la entrada de la operación abierta.
Sintaxis
strategy.opentrades.entry_price(trade_num) → series float
Argumentos
trade_num (series int) Número de la operación abierta. El número de la primera operación es cero.
Ejemplo

//@version=5
strategy("strategy.opentrades.entry_price Example 1", overlay = true)

// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars.
if ta.crossover(close, ta.sma(close, 14))
    strategy.entry("Long", strategy.long)

// Return the entry price for the latest closed trade.
currEntryPrice = strategy.opentrades.entry_price(strategy.opentrades - 1)
currExitPrice = currEntryPrice * 1.05

if high >= currExitPrice
    strategy.close("Long")

plot(currEntryPrice, "Long entry price", style = plot.style_linebr)
plot(currExitPrice, "Long exit price", color.green, style = plot.style_linebr)
Ejemplo

// Calculates the average price for the open position.
//@version=5
strategy("strategy.opentrades.entry_price Example 2", pyramiding = 2)

// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars.
if bar_index % 15 == 0
    strategy.entry("Long", strategy.long)
if bar_index % 20 == 0
    strategy.close("Long")

// Calculates the average price for the open position.
avgOpenPositionPrice() =>
    sumOpenPositionPrice = 0.0
    for tradeNo = 0 to strategy.opentrades - 1
        sumOpenPositionPrice += strategy.opentrades.entry_price(tradeNo) * strategy.opentrades.size(tradeNo) / strategy.position_size
    result = nz(sumOpenPositionPrice / strategy.opentrades)

plot(avgOpenPositionPrice())
Ver también
strategy.closedtrades.exit_price
strategy.opentrades.entry_time()

Devuelve el tiempo UNIX de entrada de la operación abierta, expresada en milisegundos.
Sintaxis
strategy.opentrades.entry_time(trade_num) → series int
Argumentos
trade_num (series int) Número de la operación abierta. El número de la primera operación es cero.
Ejemplo

//@version=5
strategy("strategy.opentrades.entry_time Example")

// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars.
if bar_index % 15 == 0
    strategy.entry("Long", strategy.long)
if bar_index % 20 == 0
    strategy.close("Long")

// Calculates duration in milliseconds since the last position was opened.
timeSinceLastEntry()=>
    strategy.opentrades > 0 ? (time - strategy.opentrades.entry_time(strategy.opentrades - 1)) : na

plot(timeSinceLastEntry() / 1000 * 60 * 60 * 24, "Days since last entry")
Ver también
strategy.closedtrades.entry_time
strategy.closedtrades.exit_time
strategy.opentrades.max_drawdown()

Devuelve la bajada máxima de la operación abierta, es decir, la pérdida máxima posible durante la negociación, expresada en strategy.account_currency.
Sintaxis
strategy.opentrades.max_drawdown(trade_num) → series float
Argumentos
trade_num (series int) Número de la operación abierta. El número de la primera operación es cero.
Ejemplo

//@version=5
strategy("strategy.opentrades.max_drawdown Example 1")

// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars.
if bar_index % 15 == 0
    strategy.entry("Long", strategy.long)
if bar_index % 20 == 0
    strategy.close("Long")

// Plot the max drawdown of the latest open trade.
plot(strategy.opentrades.max_drawdown(strategy.opentrades - 1), "Max drawdown of the latest open trade")
Ejemplo

// Calculates the max trade drawdown value for all open trades.
//@version=5
strategy("`strategy.opentrades.max_drawdown` Example 2", pyramiding = 100)

// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars.
if bar_index % 15 == 0
    strategy.entry("Long", strategy.long)
if bar_index % 20 == 0
    strategy.close("Long")

// Get the biggest max trade drawdown value from all of the open trades.
maxTradeDrawDown() =>
    maxDrawdown = 0.0
    for tradeNo = 0 to strategy.opentrades - 1
        maxDrawdown := math.max(maxDrawdown, strategy.opentrades.max_drawdown(tradeNo))
    result = maxDrawdown

plot(maxTradeDrawDown(), "Biggest max drawdown")
Observaciones
Función que devuelve na si trade_num no se encuentra en el rango 0 a strategy.closedtrades - 1.
Ver también
strategy.closedtrades.max_drawdown
strategy.max_drawdown
strategy.opentrades.max_drawdown_percent()

Devuelve la pérdida máxima de la negociación abierta, es decir, la máxima pérdida posible durante la operación, expresada en porcentaje y calculada mediante la fórmula: Lowest Value During Trade / (Entry Price x Quantity) * 100.
Sintaxis
strategy.opentrades.max_drawdown_percent(trade_num) → series float
Argumentos
trade_num (series int) El número de la operación cerrada. El número de la primera operación es cero.
Ver también
strategy.opentrades.max_drawdown
strategy.max_drawdown
strategy.opentrades.max_runup()

Devuelve la subida máxima de la operación abierta, es decir, el beneficio máximo posible durante la negociación, expresado en strategy.account_currency.
Sintaxis
strategy.opentrades.max_runup(trade_num) → series float
Argumentos
trade_num (series int) Número de la operación abierta. El número de la primera operación es cero.
Ejemplo

//@version=5
strategy("strategy.opentrades.max_runup Example 1")

// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars.
if bar_index % 15 == 0
    strategy.entry("Long", strategy.long)
if bar_index % 20 == 0
    strategy.close("Long")

// Plot the max runup of the latest open trade.
plot(strategy.opentrades.max_runup(strategy.opentrades - 1), "Max runup of the latest open trade")
Ejemplo

// Calculates the max trade runup value for all open trades.
//@version=5
strategy("strategy.opentrades.max_runup Example 2", pyramiding = 100)

// Enter a long position every 30 bars.
if bar_index % 30 == 0
    strategy.entry("Long", strategy.long)

// Calculate biggest max trade runup value from all of the open trades.
maxOpenTradeRunUp() =>
    maxRunup = 0.0
    for tradeNo = 0 to strategy.opentrades - 1
        maxRunup := math.max(maxRunup, strategy.opentrades.max_runup(tradeNo))
    result = maxRunup

plot(maxOpenTradeRunUp(), "Biggest max runup of all open trades")
Ver también
strategy.closedtrades.max_runup
strategy.max_drawdown
strategy.opentrades.max_runup_percent()

Devuelve el aumento máximo de la negociación abierta, es decir, el beneficio máximo posible durante la operación, expresado en porcentaje y calculado mediante la fórmula: Highest Value During Trade / (Entry Price x Quantity) * 100.
Sintaxis
strategy.opentrades.max_runup_percent(trade_num) → series float
Argumentos
trade_num (series int) El número de la operación cerrada. El número de la primera operación es cero.
Ver también
strategy.opentrades.max_runup
strategy.max_runup
strategy.opentrades.profit()

Devuelve el beneficio/pérdida de la operación abierta, expresado en strategy.account_currency. Las pérdidas se expresan como valores negativos.
Sintaxis
strategy.opentrades.profit(trade_num) → series float
Argumentos
trade_num (series int) Número de la operación abierta. El número de la primera operación es cero.
Ejemplo

// Returns the profit of the last open trade.
//@version=5
strategy("`strategy.opentrades.profit` Example 1", commission_type = strategy.commission.percent, commission_value = 0.1)

// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars.
if bar_index % 15 == 0
    strategy.entry("Long", strategy.long)
if bar_index % 20 == 0
    strategy.close("Long")

plot(strategy.opentrades.profit(strategy.opentrades - 1), "Profit of the latest open trade")
Ejemplo

// Calculates the profit for all open trades.
//@version=5
strategy("`strategy.opentrades.profit` Example 2", pyramiding = 5)

// Strategy calls to enter 5 long positions every 2 bars.
if bar_index % 2 == 0
    strategy.entry("Long", strategy.long, qty = 5)

// Calculate open profit or loss for the open positions.
tradeOpenPL() =>
    sumProfit = 0.0
    for tradeNo = 0 to strategy.opentrades - 1
        sumProfit += strategy.opentrades.profit(tradeNo)
    result = sumProfit
    
plot(tradeOpenPL(), "Profit of all open trades")
Ver también
strategy.closedtrades.profit
strategy.openprofit
strategy.netprofit
strategy.grossprofit
strategy.opentrades.profit_percent()

Devuelve el beneficio/pérdida de la negociación abierta, expresado en porcentaje. Las pérdidas se expresan como valores negativos.
Sintaxis
strategy.opentrades.profit_percent(trade_num) → series float
Argumentos
trade_num (series int) El número de la operación cerrada. El número de la primera operación es cero.
Ver también
strategy.opentrades.profit
strategy.opentrades.size()

Devuelve la dirección y el número de contratos negociados en la operación abierta. Si el valor es > 0, la posición de mercado fue larga. Si el valor es < 0, la posición de mercado fue corta.
Sintaxis
strategy.opentrades.size(trade_num) → series float
Argumentos
trade_num (series int) Número de la operación abierta. El número de la primera operación es cero.
Ejemplo

//@version=5
strategy("`strategy.opentrades.size` Example 1")

// We calculate the max amt of shares we can buy.
amtShares = math.floor(strategy.equity / close)
// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars
if bar_index % 15 == 0
    strategy.entry("Long", strategy.long, qty = amtShares)
if bar_index % 20 == 0
    strategy.close("Long")

// Plot the number of contracts in the latest open trade.
plot(strategy.opentrades.size(strategy.opentrades - 1), "Amount of contracts in latest open trade")
Ejemplo

// Calculates the average profit percentage for all open trades.
//@version=5
strategy("`strategy.opentrades.size` Example 2")

// Strategy calls to enter long trades every 15 bars and exit long trades every 20 bars.
if bar_index % 15 == 0
    strategy.entry("Long", strategy.long)
if bar_index % 20 == 0
    strategy.close("Long")

// Calculate profit for all open trades.
profitPct = 0.0
for tradeNo = 0 to strategy.opentrades - 1
    entryP = strategy.opentrades.entry_price(tradeNo)
    exitP = close
    profitPct += (exitP - entryP) / entryP * strategy.opentrades.size(tradeNo) * 100
    
// Calculate average profit percent for all open trades.
avgProfitPct = nz(profitPct / strategy.opentrades)
plot(avgProfitPct)
Ver también
strategy.closedtrades.size
strategy.position_size
strategy.opentrades
strategy.closedtrades
strategy.order()

Crea una nueva orden para abrir, añadir o salir de una posición. Si existe una orden no ejecutada con el mismo id, una llamada a este comando modifica esa orden.
El tipo de orden resultante depende de los parámetros limit y stop. Si la llamada no contiene argumentos limit o stop, crea una orden de mercado que se ejecuta en el siguiente tick. Si la llamada especifica un valor limit pero no un valor stop, coloca una orden límite que se ejecuta después de que el precio de mercado alcance el valor limit o un precio mejor (más bajo para órdenes de compra y más alto para órdenes de venta). Si la llamada especifica un valor stop pero no un valor limit, emite una orden stop que se ejecuta después de que el precio de mercado alcance el valor stop o un precio peor (más alto para órdenes de compra y más bajo para órdenes de venta). Si la llamada contiene argumentos limit y stop, crea una orden stop-límite, que genera una orden limitada al precio limit solo después de que el precio de mercado alcance el valor stop o un precio peor.
Las órdenes de este comando, a diferencia de las de strategy.entry, no se ven afectadas por el parámetro pyramiding de la sentencia de declaración strategy. Las estrategias pueden abrir cualquier número de operaciones en la misma dirección con llamadas a esta función.
Este comando no invierte de manera automática las posiciones abiertas porque no crea exclusivamente órdenes de entrada como hace strategy.entry. Por ejemplo, si hay una posición larga abierta de cinco acciones, una orden de este comando con un qty de 5 y un direction de strategy.short activa la venta de cinco acciones, cerrando la posición.
Sintaxis
strategy.order(id, direction, qty, limit, stop, oca_name, oca_type, comment, alert_message, disable_alert) → void
Argumentos
id (series string) Identificador de la orden, que corresponde a uno de entrada o salida en las operaciones de la estrategia tras completarse la orden. Si la estrategia abre una nueva posición después de cumplimentar la orden, el identificador de la orden se convierte en el valor strategy.position_entry_name. Los comandos de la estrategia pueden hacer referencia al ID de la orden para cancelar o modificar órdenes pendientes y generar órdenes de salida para operaciones concretas abiertas. El Probador de estrategias y el gráfico muestran el identificador de la orden salvo que el comando especifique un valor comment.
direction (series strategy_direction) La dirección de la operación. Valores posibles: strategy.long para una operación larga, strategy.short para una corta.
qty (series int/float) Opcional. Número de contratos/acciones/lotes/unidades a negociar cuando la orden se ejecute. El valor por defecto es na, lo que significa que la orden utiliza los parámetros default_qty_type y default_qty_value de la sentencia de declaración strategy para determinar la cantidad.
limit (series int/float) Opcional. Precio límite de la orden. Si se especifica, la orden crea una orden limit o stop-limit, dependiendo de si también se indica el valor stop. El valor por defecto es na, lo que significa que la orden resultante no es del tipo limit o stop-limit.
stop (series int/float) Opcional. Precio stop de la orden. Si se especifica, la orden crea una orden stop o stop-limit, dependiendo de si también se indica el valor limit. El valor por defecto es na, lo que significa que la orden resultante no es del tipo stop o stop-limit.
oca_name (series string) Opcional. Nombre del grupo de órdenes Una-cancela-todas (OCA). Cuando se ejecuta una orden pendiente con los mismos parámetros oca_name y oca_type, esa orden afecta a todas las no ejecutadas del grupo. El valor por defecto es una cadena vacía, lo que significa que la orden no pertenece a un grupo OCA.
oca_type (input string) Opcional. Especifica cómo se comporta una orden no ejecutada cuando se ejecuta otra pendiente con los mismos valores oca_name y oca_type. Valores posibles: strategy.oca.cancel, strategy.oca.reduce, strategy.oca.none. El valor por defecto es strategy.oca.none.
comment (series string) Opcional. Notas adicionales sobre la orden ejecutada. Si el valor no es una cadena vacía, el Simulador de estrategias y el gráfico muestran este texto para la orden en lugar del id especificado. El valor por defecto es una cadena vacía.
alert_message (series string) Opcional. Texto personalizado para la alerta que se activa cuando se completa una orden. Si el campo «Mensaje» del cuadro de diálogo «Crear alerta» contiene el marcador de posición {{strategy.order.alert_message}}, el mensaje de alerta sustituye al marcador de posición por este texto. Por defecto es una cadena vacía.
disable_alert (series bool) Opcional. Si true cuando el comando crea una orden, la estrategia no activa una alerta cuando esa orden se ejecuta. Este parámetro acepta un valor de «serie», lo que significa que los usuarios pueden controlar qué órdenes activan alertas cuando se ejecutan. El valor por defecto es false.
Ejemplo

//@version=5
strategy("Market order strategy", overlay = true, margin_long = 100, margin_short = 100)

// Calculate a 14-bar and 28-bar moving average of `close` prices.
float sma14 = ta.sma(close, 14)
float sma28 = ta.sma(close, 28)

// Place a market order to enter a long position when `sma14` crosses over `sma28`.
if ta.crossover(sma14, sma28) and strategy.position_size == 0
    strategy.order("My Long Entry ID", strategy.long)

// Place a market order to sell the same quantity as the long trade when `sma14` crosses under `sma28`, 
// effectively closing the long position.
if ta.crossunder(sma14, sma28) and strategy.position_size > 0
    strategy.order("My Long Exit ID", strategy.short)
Ejemplo

//@version=5
strategy("Limit and stop exit strategy", overlay = true, margin_long = 100, margin_short = 100)

//@variable The distance from the long entry price for each short limit order.
float shortOffsetInput = input.int(200, "Sell limit/stop offset, in ticks", 1) * syminfo.mintick

//@function Draws a label and line at the specified `price` to visualize a limit order's level. 
drawLimit(float price, bool isLong, bool isStop = false) =>
    color col = isLong ? color.blue : color.red
    label.new(
         bar_index, price, (isLong ? "Long " : "Short ") + (isStop ? "stop" : "limit") + " order created", 
         style = label.style_label_right, color = col, textcolor = color.white
     )
    line.new(bar_index, price, bar_index + 1, price, extend = extend.right, style = line.style_dashed, color = col)

//@function Stops the `l` line from extending further.
method stopExtend(line l) =>
    l.set_x2(bar_index)
    l.set_extend(extend.none)

// Initialize two `line` variables to reference limit and stop line IDs.
var line profitLimit = na
var line lossStop    = na

// Calculate a 14-bar and 28-bar moving average of `close` prices.
float sma14 = ta.sma(close, 14)
float sma28 = ta.sma(close, 28)

if ta.crossover(sma14, sma28) and strategy.position_size == 0
    // Place a market order to enter a long position.
    strategy.order("My Long Entry ID", strategy.long)
    
if strategy.position_size > 0 and strategy.position_size[1] == 0
    //@variable The entry price of the long trade. 
    float entryPrice = strategy.opentrades.entry_price(0)
    // Calculate short limit and stop levels above and below the `entryPrice`.
    float profitLevel = entryPrice + shortOffsetInput
    float lossLevel   = entryPrice - shortOffsetInput
    // Place short limit and stop orders at the `profitLevel` and `lossLevel`. 
    strategy.order("Profit", strategy.short, limit = profitLevel, oca_name = "Bracket", oca_type = strategy.oca.cancel)
    strategy.order("Loss", strategy.short, stop = lossLevel, oca_name = "Bracket", oca_type = strategy.oca.cancel)
    // Make new drawings for the `profitLimit` and `lossStop` lines.
    profitLimit := drawLimit(profitLevel, isLong = false)
    lossStop    := drawLimit(lossLevel, isLong = false, isStop = true)

if ta.change(strategy.closedtrades) > 0
    // Stop extending the `profitLimit` and `lossStop` lines.
    profitLimit.stopExtend()
    lossStop.stopExtend()
strategy.risk.allow_entry_in()

Esta función se puede utilizar para especificar la tendencia de mercado en la que se permite a la función strategy.entry abrir posiciones.
Sintaxis
strategy.risk.allow_entry_in(value) → void
Argumentos
value (simple string) Dirección permitida. Valores posibles: strategy.direction.all, strategy.direction.long, strategy.direction.short
Ejemplo

//@version=5
strategy("strategy.risk.allow_entry_in")

strategy.risk.allow_entry_in(strategy.direction.long)
if open > close
    strategy.entry("Long", strategy.long)
// Instead of opening a short position with 10 contracts, this command will close long entries.
if open < close
    strategy.entry("Short", strategy.short, qty = 10)
strategy.risk.max_cons_loss_days()

El objeto de esta norma es cancelar todas las órdenes pendientes, cerrar todas la posiciones abiertas y no volver a cursar órdenes después de un número concreto de dias consecutivos con pérdidas. La norma afecta a toda la estrategia.
Sintaxis
strategy.risk.max_cons_loss_days(count, alert_message) → void
Argumentos
count (simple int) Parámetro obligatorio. Número permitido de días consecutivos con pérdidas.
alert_message (simple string) Parámetro opcional que sustituye al marcador de posición {{strategy.order.alert_message}} cuando se usa en el campo "Mensaje" del cuadro de diálogo "Crear alerta"
Ejemplo

//@version=5
strategy("risk.max_cons_loss_days Demo 1")
strategy.risk.max_cons_loss_days(3) // No orders will be placed after 3 days, if each day is with loss.
plot(strategy.position_size)
strategy.risk.max_drawdown()

El objeto de esta norma es determinar la serie de pérdidas máxima. La norma afecta a toda la estrategia. Una vez se haya alcanzado el valor de pérdida máxima, todas las órdenes pendientes se cancelan, todas las posiciones abiertas se cierran y ya no se pueden cursar más órdenes.
Sintaxis
strategy.risk.max_drawdown(value, type, alert_message) → void
Argumentos
value (simple int/float) Parámetro obligatorio. Valor máximo de la serie de pérdidas. Se especifica en dinero (divisa de referencia) o en porcentaje del patrimonio máximo. Para el % del patrimonio, el rango de valores permitidos es de 0 a 100.
type (simple string) Parámetro obligatorio. Tipo del valor. Especifique uno de los siguientes valores: strategy.percent_of_equity or strategy.cash. Nota: si el patrimonio baja a cero o a un valor negativo, y se ha especificado 'strategy.percent_of_equity'', se cancelan todas las órdenes pendientes, se cierran todas las posiciones abiertas y ya no se podrán cursar nuevas órdenes.
alert_message (simple string) Parámetro opcional que sustituye al marcador de posición {{strategy.order.alert_message}} cuando se usa en el campo "Mensaje" del cuadro de diálogo "Crear alerta"
Ejemplo

//@version=5
strategy("risk.max_drawdown Demo 1")
strategy.risk.max_drawdown(50, strategy.percent_of_equity) // set maximum drawdown to 50% of maximum equity
plot(strategy.position_size)
Ejemplo

//@version=5
strategy("risk.max_drawdown Demo 2", currency = "EUR")
strategy.risk.max_drawdown(2000, strategy.cash)  // set maximum drawdown to 2000 EUR from maximum equity
plot(strategy.position_size)
strategy.risk.max_intraday_filled_orders()

El objeto de esta norma es determinar el número máximo de órdenes ejecutadas en un día (por 1 barra, si la resolución del gráfico es superior a un día). La norma afecta a toda la estrategia. Una vez se haya alcanzado el máximo número de órdenes ejecutadas, todas las órdenes pendientes se cancelan, todas las posiciones abiertas se cierran y ya no se pueden cursar más órdenes hasta el final de la actual sesión de negociación.
Sintaxis
strategy.risk.max_intraday_filled_orders(count, alert_message) → void
Argumentos
count (simple int) Parámetro obligatorio. El número máximo de órdenes ejecutadas por 1 día.
alert_message (simple string) Parámetro opcional que sustituye al marcador de posición {{strategy.order.alert_message}} cuando se usa en el campo "Mensaje" del cuadro de diálogo "Crear alerta"
Ejemplo

//@version=5
strategy("risk.max_intraday_filled_orders Demo")
strategy.risk.max_intraday_filled_orders(10) // After 10 orders are filled, no more strategy orders will be placed (except for a market order to exit current open market position, if there is any).
if open > close
    strategy.entry("buy", strategy.long)
if open < close
    strategy.entry("sell", strategy.short)
strategy.risk.max_intraday_loss()

Valor máximo de pérdida permitido durante un día. Se especifica en dinero (divisa de referencia) o en un porcentaje del capital máximo intradía (de 0 a 100).
Sintaxis
strategy.risk.max_intraday_loss(value, type, alert_message) → void
Argumentos
value (simple int/float) Parámetro obligatorio. Valor máximo de pérdida. Se especifica en dinero (divisa de referencia) o en porcentaje del capital máximo intradía. El rango de valores permitidos para el % del capital es de 0 a 100.
type (simple string) Parámetro obligatorio. Tipo del valor. Especifique uno de los siguientes valores: strategy.percent_of_equity o strategy.cash. Nota: si el capital baja a cero o a un valor negativo y se ha especificado strategy.percent_of_equity, se cancelan todas las órdenes pendientes, se cierran todas las posiciones abiertas y no se pueden volver a emitir nuevas órdenes de forma definitiva.
alert_message (simple string) Parámetro opcional que sustituye al marcador de posición {{strategy.order.alert_message}} cuando se usa en el campo "Mensaje" del cuadro de diálogo "Crear alerta"
Ejemplo

// Sets the maximum intraday loss using the strategy's equity value.
//@version=5
strategy("strategy.risk.max_intraday_loss Example 1", overlay = false, default_qty_type = strategy.percent_of_equity, default_qty_value = 100)

// Input for maximum intraday loss %. 
lossPct = input.float(10)

// Set maximum intraday loss to our lossPct input
strategy.risk.max_intraday_loss(lossPct, strategy.percent_of_equity)

// Enter Short at bar_index zero.
if bar_index == 0
    strategy.entry("Short", strategy.short)

// Store equity value from the beginning of the day
eqFromDayStart = ta.valuewhen(ta.change(dayofweek) > 0, strategy.equity, 0)

// Calculate change of the current equity from the beginning of the current day.
eqChgPct = 100 * ((strategy.equity - eqFromDayStart) / strategy.equity)

// Plot it
plot(eqChgPct) 
hline(-lossPct)
Ejemplo

// Sets the maximum intraday loss using the strategy's cash value.
//@version=5
strategy("strategy.risk.max_intraday_loss Example 2", overlay = false)

// Input for maximum intraday loss in absolute cash value of the symbol. 
absCashLoss = input.float(5)

// Set maximum intraday loss to `absCashLoss` in account currency.
strategy.risk.max_intraday_loss(absCashLoss, strategy.cash)

// Enter Short at bar_index zero.
if bar_index == 0
    strategy.entry("Short", strategy.short)

// Store the open price value from the beginning of the day.
beginPrice = ta.valuewhen(ta.change(dayofweek) > 0, open, 0)

// Calculate the absolute price change for the current period.
priceChg = (close - beginPrice)

hline(absCashLoss)
plot(priceChg)
Ver también
strategy
strategy.percent_of_equity
strategy.cash
strategy.risk.max_position_size()

El objeto de esta norma es determinar el tamaño máximo de una posición de mercado. La norma afecta a la siguiente función: strategy.entry. La cantidad de la 'entrada' se puede reducir (si fuera necesario) al número correspondiente de contratos/acciones/lotes/unidades, para que el tamaño total de la posición no exceda el valor indicado en 'strategy.risk.max_position_size'. Si la cantidad mínima posible sigue incumpliendo la norma, no se podrá cursar la orden.
Sintaxis
strategy.risk.max_position_size(contracts) → void
Argumentos
contracts (simple int/float) Parámetro obligatorio. Cantidad máxima de contratos/acciones/lotes/unidades en una posición.
Ejemplo

//@version=5
strategy("risk.max_position_size Demo", default_qty_value = 100)
strategy.risk.max_position_size(10)
if open > close
    strategy.entry("buy", strategy.long)
plot(strategy.position_size)  // max plot value will be 10
string()4 sobrecargas

Convierte na en string
Sintaxis y sobrecargas
string(x) → const string
string(x) → input string
string(x) → simple string
string(x) → series string
Argumentos
x (const string) Valor a convertir al tipo especificado, normalmente na.
Devoluciones
Valor del argumento después de convertirse en string.
Ver también
float
int
bool
color
line
label
syminfo.prefix()2 sobrecargas

Devuelve el prefijo bursátil del symbol, por ejemplo, "NASDAQ".
Sintaxis y sobrecargas
syminfo.prefix(symbol) → simple string
syminfo.prefix(symbol) → series string
Argumentos
symbol (simple string) Símbolo. Tenga en cuenta que el símbolo debe contener un prefijo. Por ejemplo: "NASDAQ:AAPL" en lugar de "AAPL".
Ejemplo

//@version=5
indicator("syminfo.prefix fun", overlay=true)
i_sym = input.symbol("NASDAQ:AAPL")
pref = syminfo.prefix(i_sym)
tick = syminfo.ticker(i_sym)
t = ticker.new(pref, tick, session.extended)
s = request.security(t, "1D", close)
plot(s)
Devoluciones
Devuelve el prefijo bursátil del symbol, por ejemplo, "NASDAQ".
Observaciones
El resultado de la función se utiliza en las funciones ticker.new/ticker.modify y request.security.
Ver también
syminfo.tickerid
syminfo.ticker
syminfo.prefix
syminfo.ticker
ticker.new
syminfo.ticker()2 sobrecargas

Devuelve el nombre del symbol sin el prefijo del mercado de valores, por ejemplo, "AAPL".
Sintaxis y sobrecargas
syminfo.ticker(symbol) → simple string
syminfo.ticker(symbol) → series string
Argumentos
symbol (simple string) Símbolo. Tenga en cuenta que el símbolo debe contener un prefijo. Por ejemplo: "NASDAQ:AAPL" en lugar de "AAPL".
Ejemplo

//@version=5
indicator("syminfo.ticker fun", overlay=true) 
i_sym = input.symbol("NASDAQ:AAPL")
pref = syminfo.prefix(i_sym)
tick = syminfo.ticker(i_sym)
t = ticker.new(pref, tick, session.extended)
s = request.security(t, "1D", close)
plot(s)
Devoluciones
Devuelve el nombre del symbol sin el prefijo del mercado de valores, por ejemplo, "AAPL".
Observaciones
El resultado de la función se utiliza en las funciones ticker.new/ticker.modify y request.security.
Ver también
syminfo.tickerid
syminfo.ticker
syminfo.prefix
syminfo.prefix
ticker.new
ta.alma()2 sobrecargas

Media móvil de Arnaud Legoux. Utiliza la distribución gaussiana como pesos para hallar la media móvil.
Sintaxis y sobrecargas
ta.alma(series, length, offset, sigma) → series float
ta.alma(series, length, offset, sigma, floor) → series float
Argumentos
series (series int/float) Serie de valores a procesar.
length (series int) Número de barras (longitud)
offset (simple int/float) Controla el intercambio entre suavidad (más cercano a 1) y capacidad de respuesta (más cercano a 0).
sigma (simple int/float) Cambia la suavidad de ALMA. El sigma más grande es el ALMA más suave.
Ejemplo

//@version=5
indicator("ta.alma", overlay=true) 
plot(ta.alma(close, 9, 0.85, 6))

// same on pine, but much less efficient
pine_alma(series, windowsize, offset, sigma) =>
    m = offset * (windowsize - 1)
    //m = math.floor(offset * (windowsize - 1)) // Used as m when math.floor=true
    s = windowsize / sigma
    norm = 0.0
    sum = 0.0
    for i = 0 to windowsize - 1
        weight = math.exp(-1 * math.pow(i - m, 2) / (2 * math.pow(s, 2)))
        norm := norm + weight
        sum := sum + series[windowsize - i - 1] * weight
    sum / norm
plot(pine_alma(close, 9, 0.85, 6))
Devoluciones
Media móvil de Arnaud Legoux.
Observaciones
Los valores na de la serie source se incluyen en los cálculos y generarán un resultado na.
Ver también
ta.sma
ta.ema
ta.rma
ta.wma
ta.vwma
ta.swma
ta.atr()

La función atr (rango verdadero medio) devuelve el RMA del rango verdadero. El rango verdadero es max(máximo - mínimo, abs(máximo - cierre[1]), abs(mínimo - cierre[1])).
Sintaxis
ta.atr(length) → series float
Argumentos
length (simple int) Longitud (número de barras históricas).
Ejemplo

//@version=5
indicator("ta.atr")
plot(ta.atr(14))

//the same on pine
pine_atr(length) =>
    trueRange = na(high[1])? high-low : math.max(math.max(high - low, math.abs(high - close[1])), math.abs(low - close[1]))
    //true range can be also calculated with ta.tr(true)
    ta.rma(trueRange, length)

plot(pine_atr(14))
Devoluciones
Rango verdadero medio.
Observaciones
Se ignoran los valores na de la serie source; la función calcula sobre la cantidad length de valores no na.
Ver también
ta.tr
ta.rma
ta.barssince()

Cuenta el número de barras desde la última vez que la condición fue true.
Sintaxis
ta.barssince(condition) → series int
Argumentos
condition (series bool) La condición a comprobar.
Ejemplo

//@version=5
indicator("ta.barssince")
// get number of bars since last color.green bar
plot(ta.barssince(close >= open))
Devoluciones
Número de barras desde que la condición era true.
Observaciones
Si no se cumple la condición con anterioridad a la barra actual, la función devuelve na.
Tenga en cuenta que usar esta variable o función puede provocar un repintado del indicador.
Ver también
ta.lowestbars
ta.highestbars
ta.valuewhen
ta.highest
ta.lowest
ta.bb()

Bandas de Bollinger. Una banda de Bollinger es una herramienta de análisis técnico definida por un conjunto de líneas trazadas por dos desviaciones estándar (tanto positivas como negativas) que se encuentran lejos de la media móvil simple (SMA) del precio del valor, pero que se puede ajustar según las preferencias del usuario.
Sintaxis
ta.bb(series, length, mult) → [series float, series float, series float]
Argumentos
series (series int/float) Serie de valores a procesar.
length (series int) Número de barras (longitud)
mult (simple int/float) Factor de desviación típica.
Ejemplo

//@version=5
indicator("ta.bb")

[middle, upper, lower] = ta.bb(close, 5, 4)
plot(middle, color=color.yellow)
plot(upper, color=color.yellow)
plot(lower, color=color.yellow)

// the same on pine
f_bb(src, length, mult) =>
    float basis = ta.sma(src, length)
    float dev = mult * ta.stdev(src, length)
    [basis, basis + dev, basis - dev]

[pineMiddle, pineUpper, pineLower] = f_bb(close, 5, 4)

plot(pineMiddle)
plot(pineUpper)
plot(pineLower)
Devoluciones
Bandas de Bollinger.
Observaciones
Se ignoran los valores na de la serie source; la función calcula sobre la cantidad length de valores no na.
Ver también
ta.sma
ta.stdev
ta.kc
ta.bbw()

Ancho de las Bandas de Bollinger. Es la diferencia entre las Bandas Bollinger superior e inferior dividida por la banda media.
Sintaxis
ta.bbw(series, length, mult) → series float
Argumentos
series (series int/float) Serie de valores a procesar.
length (series int) Número de barras (longitud)
mult (simple int/float) Factor de desviación típica.
Ejemplo

//@version=5
indicator("ta.bbw")

plot(ta.bbw(close, 5, 4), color=color.yellow)

// the same on pine
f_bbw(src, length, mult) =>
    float basis = ta.sma(src, length)
    float dev = mult * ta.stdev(src, length)
    ((basis + dev) - (basis - dev)) / basis

plot(f_bbw(close, 5, 4))
Devoluciones
Ancho de las Bandas de Bollinger.
Observaciones
Se ignoran los valores na de la serie source; la función calcula sobre la cantidad length de valores no na.
Ver también
ta.bb
ta.sma
ta.stdev
ta.cci()

El CCI (índice de canal de materias primas) se calcula en función de la diferencia entre el precio típico de una materia prima y su media móvil simple, dividido por la desviación absoluta media del precio típico. El índice se calcula en escala por un factor inverso de 0,015 para proporcionar números más legibles.
Sintaxis
ta.cci(source, length) → series float
Argumentos
source (series int/float) Serie de valores a procesar.
length (series int) Número de barras (longitud)
Devoluciones
Índice del canal de materias primas de la fuente para la longitud de las barras históricas.
Observaciones
Se ignoran los valores na de la serie source.
ta.change()6 sobrecargas

Compara el valor actual de source respecto a la length de sus barras históricas y devuelve la diferencia.
Sintaxis y sobrecargas
ta.change(source) → series int
ta.change(source) → series float
ta.change(source, length) → series int
ta.change(source, length) → series float
ta.change(source) → series bool
ta.change(source, length) → series bool
Argumentos
source (series int) Serie de fuentes.
Ejemplo

//@version=5
indicator('Day and Direction Change', overlay = true)
dailyBarTime = time('1D')
isNewDay = ta.change(dailyBarTime) != 0
bgcolor(isNewDay ? color.new(color.green, 80) : na)

isGreenBar = close >= open
colorChange = ta.change(isGreenBar)
plotshape(colorChange, 'Direction Change')
Devoluciones
Diferencia entre valores cuando estos son numéricos. Si se utiliza una fuente 'bool', devuelve true cuando la fuente actual es diferente a la anterior.
Observaciones
Los valores na de la serie source se incluyen en los cálculos y generarán un resultado na.
Ver también
ta.mom
ta.cross
ta.cmo()

Oscilador momentum de Chande. Se calcula la diferencia entre la suma de las ganancias y la de las pérdidas recientes y, a continuación, se divide el resultado por la suma de todas las fluctuaciones de precios durante el mismo período.
Sintaxis
ta.cmo(series, length) → series float
Argumentos
series (series int/float) Serie de valores a procesar.
length (series int) Número de barras (longitud)
Ejemplo

//@version=5
indicator("ta.cmo")
plot(ta.cmo(close, 5), color=color.yellow)

// the same on pine
f_cmo(src, length) =>
    float mom = ta.change(src)
    float sm1 = math.sum((mom >= 0) ? mom : 0.0, length)
    float sm2 = math.sum((mom >= 0) ? 0.0 : -mom, length)
    100 * (sm1 - sm2) / (sm1 + sm2)

plot(f_cmo(close, 5))
Devoluciones
Oscilador momentum de Chande.
Observaciones
Se ignoran los valores na de la serie source.
Ver también
ta.rsi
ta.stoch
math.sum
ta.cog()

El cog (centro de gravedad) es un indicador que se basa en las estadísticas y la relación áurea de Fibonacci.
Sintaxis
ta.cog(source, length) → series float
Argumentos
source (series int/float) Serie de valores a procesar.
length (series int) Número de barras (longitud)
Ejemplo

//@version=5
indicator("ta.cog", overlay=true) 
plot(ta.cog(close, 10))

// the same on pine
pine_cog(source, length) =>
    sum = math.sum(source, length)
    num = 0.0
    for i = 0 to length - 1
        price = source[i]
        num := num + price * (i + 1)
    -num / sum

plot(pine_cog(close, 10))
Devoluciones
Centro de gravedad.
Observaciones
Se ignoran los valores na de la serie source.
Ver también
ta.stoch
ta.correlation()

Coeficiente de correlación. Describe el grado en que dos series tienden a desviarse de sus valores ta.sma.
Sintaxis
ta.correlation(source1, source2, length) → series float
Argumentos
source1 (series int/float) Serie de fuentes.
source2 (series int/float) Serie de objetivos.
length (series int) Longitud (número de barras históricas).
Devoluciones
Coeficiente de correlación.
Observaciones
Se ignoran los valores na de la serie source; la función calcula sobre la cantidad length de valores no na.
Ver también
request.security
ta.cross()

Sintaxis
ta.cross(source1, source2) → series bool
Argumentos
source1 (series int/float) Primera serie de datos.
source2 (series int/float) Segunda serie de datos.
Devoluciones
true si dos series se cruzan, de lo contrario false.
Ver también
ta.change
ta.crossover()

Se considera que la serie source1 ha cruzado por encima de la serie source2 si, en la barra actual, el valor de source1 es mayor que el valor de source2, y cuando en la barra anterior, el valor de source1 era menor o igual al valor de source2.
Sintaxis
ta.crossover(source1, source2) → series bool
Argumentos
source1 (series int/float) Primera serie de datos.
source2 (series int/float) Segunda serie de datos.
Devoluciones
true si source1 se cruza por encima de source2, de lo contrario false.
ta.crossunder()

Se considera que la serie source1 ha cruzado por debajo de la serie source2 si, en la barra actual, el valor de source1 es menor que el valor de source2, y cuando en la barra anterior, el valor de source1 era mayor o igual al valor de source2.
Sintaxis
ta.crossunder(source1, source2) → series bool
Argumentos
source1 (series int/float) Primera serie de datos.
source2 (series int/float) Segunda serie de datos.
Devoluciones
true si source1 se cruza por debajo de source2, de lo contrario false.
ta.cum()

Suma acumulativa (total) de source. Es decir, la suma de todos los elementos de source.
Sintaxis
ta.cum(source) → series float
Argumentos
source (series int/float) Fuente utilizada para el cálculo.
Devoluciones
Series de suma total.
Ver también
math.sum
ta.dev()

Medida de la diferencia entre la serie y su ta.sma
Sintaxis
ta.dev(source, length) → series float
Argumentos
source (series int/float) Serie de valores a procesar.
length (series int) Número de barras (longitud)
Ejemplo

//@version=5
indicator("ta.dev")
plot(ta.dev(close, 10))

// the same on pine
pine_dev(source, length) =>
    mean = ta.sma(source, length)
    sum = 0.0
    for i = 0 to length - 1
        val = source[i]
        sum := sum + math.abs(val - mean)
    dev = sum/length
plot(pine_dev(close, 10))
Devoluciones
Desviación de source respecto a length barras históricas.
Observaciones
Se ignoran los valores na de la serie source.
Ver también
ta.variance
ta.stdev
ta.dmi()

La función dmi devuelve el índice del movimiento direccional.
Sintaxis
ta.dmi(diLength, adxSmoothing) → [series float, series float, series float]
Argumentos
diLength (simple int) Periodo DI
adxSmoothing (simple int) Período ADX suavizado.
Ejemplo

//@version=5
indicator(title="Directional Movement Index", shorttitle="DMI", format=format.price, precision=4)
len = input.int(17, minval=1, title="DI Length")
lensig = input.int(14, title="ADX Smoothing", minval=1)
[diplus, diminus, adx] = ta.dmi(len, lensig)
plot(adx, color=color.red, title="ADX")
plot(diplus, color=color.blue, title="+DI")
plot(diminus, color=color.orange, title="-DI")
Devoluciones
Tuple de tres series DMI: movimiento direccional positivo (+DI), movimiento direccional negativo (-DI) e índice de movimiento direccional medio (ADX).
Ver también
ta.rsi
ta.tsi
ta.mfi
ta.ema()

La función ema devuelve una media móvil ponderada exponencialmente. En ema las ponderaciones se reducen de forma exponencial. Se calcula mediante la fórmula: EMA = alpha * source + (1 - alpha) * EMA[1], donde alpha = 2 / (length + 1).
Sintaxis
ta.ema(source, length) → series float
Argumentos
source (series int/float) Serie de valores a procesar.
length (simple int) Número de barras (longitud)
Ejemplo

//@version=5
indicator("ta.ema")
plot(ta.ema(close, 15))

//the same on pine
pine_ema(src, length) =>
    alpha = 2 / (length + 1)
    sum = 0.0
    sum := na(sum[1]) ? src : alpha * src + (1 - alpha) * nz(sum[1])
plot(pine_ema(close,15))
Devoluciones
Media móvil exponencial de source siendo alpha = 2 / (length + 1).
Observaciones
Tenga en cuenta que usar esta variable o función puede provocar un repintado del indicador.
Se ignoran los valores na de la serie source; la función calcula sobre la cantidad length de valores no na.
Ver también
ta.sma
ta.rma
ta.wma
ta.vwma
ta.swma
ta.alma
ta.falling()

Compruebe si la serie source está disminuyendo respecto a la longitud length de las barras.
Sintaxis
ta.falling(source, length) → series bool
Argumentos
source (series int/float) Serie de valores a procesar.
length (series int) Número de barras (longitud)
Devoluciones
true si el valor actual de source es menor que cualquier valor anterior source para length barras históricas; de lo contrario, false.
Observaciones
Se ignoran los valores na de la serie source; la función calcula sobre la cantidad length de valores no na.
Ver también
ta.rising
ta.highest()2 sobrecargas

Valor más alto para un número dado de barras históricas.
Sintaxis y sobrecargas
ta.highest(length) → series float
ta.highest(source, length) → series float
Argumentos
length (series int) Número de barras (longitud)
Devoluciones
Valor máximo de la serie.
Observaciones
Versión con dos argumentos: source es la serie y length es el número de barras históricas.
Versión con un argumento: length es el número de barras históricas. El algoritmo utiliza high (alto) como una serie source.
Se ignoran los valores na de la serie source.
Ver también
ta.lowest
ta.lowestbars
ta.highestbars
ta.valuewhen
ta.barssince
ta.highestbars()2 sobrecargas

Compensación del valor más alto para un número determinado de barras históricas.
Sintaxis y sobrecargas
ta.highestbars(length) → series int
ta.highestbars(source, length) → series int
Argumentos
length (series int) Número de barras (longitud)
Devoluciones
Compensación a la barra más alta
Observaciones
Versión con dos argumentos: source es la serie y length es el número de barras históricas.
Versión con un argumento: length es el número de barras históricas. El algoritmo utiliza high (alto) como una serie source.
Se ignoran los valores na de la serie source.
Ver también
ta.lowest
ta.highest
ta.lowestbars
ta.barssince
ta.valuewhen
ta.hma()

La función hma devuelve la media móvil de Hull.
Sintaxis
ta.hma(source, length) → series float
Argumentos
source (series int/float) Serie de valores a procesar.
length (simple int) Número de barras.
Ejemplo

//@version=5
indicator("Hull Moving Average")
src = input(defval=close, title="Source")
length = input(defval=9, title="Length")
hmaBuildIn = ta.hma(src, length)
plot(hmaBuildIn, title="Hull MA", color=#674EA7)
Devoluciones
Media móvil de Hull de 'source' para las barras históricas de 'length'.
Observaciones
Se ignoran los valores na de la serie source.
Ver también
ta.ema
ta.rma
ta.wma
ta.vwma
ta.sma
ta.kc()2 sobrecargas

Canales Keltner. El canal Keltner es un indicador de análisis técnico que muestra una línea en la media móvil central, además de las líneas de canal a una distancia superior e inferior.
Sintaxis y sobrecargas
ta.kc(series, length, mult) → [series float, series float, series float]
ta.kc(series, length, mult, useTrueRange) → [series float, series float, series float]
Argumentos
series (series int/float) Serie de valores a procesar.
length (simple int) Número de barras (longitud)
mult (simple int/float) Factor de desviación típica.
Ejemplo

//@version=5
indicator("ta.kc")

[middle, upper, lower] = ta.kc(close, 5, 4)
plot(middle, color=color.yellow)
plot(upper, color=color.yellow)
plot(lower, color=color.yellow)


// the same on pine
f_kc(src, length, mult, useTrueRange) =>
    float basis = ta.ema(src, length)
    float span = (useTrueRange) ? ta.tr : (high - low)
    float rangeEma = ta.ema(span, length)
    [basis, basis + rangeEma * mult, basis - rangeEma * mult]
    
[pineMiddle, pineUpper, pineLower] = f_kc(close, 5, 4, true)

plot(pineMiddle)
plot(pineUpper)
plot(pineLower)
Devoluciones
Canales Keltner.
Observaciones
Se ignoran los valores na de la serie source; la función calcula sobre la cantidad length de valores no na.
Ver también
ta.ema
ta.atr
ta.bb
ta.kcw()2 sobrecargas

Ancho de los Canales de Keltner. Es la diferencia entre el ancho de los Canales Keltner superiores e inferiores dividida por el canal medio.
Sintaxis y sobrecargas
ta.kcw(series, length, mult) → series float
ta.kcw(series, length, mult, useTrueRange) → series float
Argumentos
series (series int/float) Serie de valores a procesar.
length (simple int) Número de barras (longitud)
mult (simple int/float) Factor de desviación típica.
Ejemplo

//@version=5
indicator("ta.kcw")

plot(ta.kcw(close, 5, 4), color=color.yellow)

// the same on pine
f_kcw(src, length, mult, useTrueRange) =>
    float basis = ta.ema(src, length)
    float span = (useTrueRange) ? ta.tr : (high - low)
    float rangeEma = ta.ema(span, length)
    
    ((basis + rangeEma * mult) - (basis - rangeEma * mult)) / basis

plot(f_kcw(close, 5, 4, true))
Devoluciones
Ancho de los Canales de Keltner.
Observaciones
Se ignoran los valores na de la serie source; la función calcula sobre la cantidad length de valores no na.
Ver también
ta.kc
ta.ema
ta.atr
ta.bb
ta.linreg()

Curva de regresión lineal. Línea que mejor se ajusta a los precios especificados durante un periodo de tiempo definido por el usuario. Se calcula utilizando el método de mínimos cuadrados. El resultado de esta función se calcula mediante la fórmula: linreg = intercept + slope * (length - 1 - offset), donde intercept y slope son los valores calculados con el método de mínimos cuadrados sobre la serie source.
Sintaxis
ta.linreg(source, length, offset) → series float
Argumentos
source (series int/float) Serie de fuentes.
length (series int) Número de barras (longitud)
offset (simple int) Compensación.
Devoluciones
Curva de regresión lineal.
Observaciones
Los valores na de la serie source se incluyen en los cálculos y generarán un resultado na.
ta.lowest()2 sobrecargas

Valor más bajo para un número dado de barras históricas.
Sintaxis y sobrecargas
ta.lowest(length) → series float
ta.lowest(source, length) → series float
Argumentos
length (series int) Número de barras (longitud)
Devoluciones
Valor mínimo de la serie.
Observaciones
Versión con dos argumentos: source es la serie y length es el número de barras históricas.
Versión con un argumento: length es el número de barras históricas. El algoritmo utiliza low (bajo) como una serie source.
Se ignoran los valores na de la serie source.
Ver también
ta.highest
ta.lowestbars
ta.highestbars
ta.valuewhen
ta.barssince
ta.lowestbars()2 sobrecargas

Compensación del valor más bajo para un número dado de barras históricas.
Sintaxis y sobrecargas
ta.lowestbars(length) → series int
ta.lowestbars(source, length) → series int
Argumentos
length (series int) Número de barras históricas.
Devoluciones
Compensación a la barra más baja.
Observaciones
Versión con dos argumentos: source es la serie y length es el número de barras históricas.
Versión con un argumento: length es el número de barras históricas. El algoritmo utiliza low (bajo) como una serie source.
Se ignoran los valores na de la serie source.
Ver también
ta.lowest
ta.highest
ta.highestbars
ta.barssince
ta.valuewhen
ta.macd()

MACD (convergencia/divergencia de la media móvil). El objetivo es que muestre cambios en la fuerza, la dirección, el momentum y la duración de una tendencia en el precio de una acción.
Sintaxis
ta.macd(source, fastlen, slowlen, siglen) → [series float, series float, series float]
Argumentos
source (series int/float) Serie de valores a procesar.
fastlen (simple int) Parámetro de longitud rápida.
slowlen (simple int) Parametro longitud lenta.
siglen (simple int) Parametro de longitud de señal.
Ejemplo

//@version=5
indicator("MACD")
[macdLine, signalLine, histLine] = ta.macd(close, 12, 26, 9)
plot(macdLine, color=color.blue)
plot(signalLine, color=color.orange)
plot(histLine, color=color.red, style=plot.style_histogram)
Si necesita un solo valor, utilice los marcadores de posición '_' de esta manera:
Ejemplo

//@version=5
indicator("MACD")
[_, signalLine, _] = ta.macd(close, 12, 26, 9)
plot(signalLine, color=color.orange)
Devoluciones
Tuple de tres series MACD: línea MACD, línea de señal y línea de histograma.
Observaciones
Se ignoran los valores na de la serie source; la función calcula sobre la cantidad length de valores no na.
Ver también
ta.sma
ta.ema
ta.max()

Devuelve el valor máximo histórico de source desde el inicio del gráfico hasta la barra actual.
Sintaxis
ta.max(source) → series float
Argumentos
source (series int/float) Fuente utilizada para el cálculo.
Observaciones
Se ignoran las apariciones na de source.
ta.median()2 sobrecargas

Devuelve la mediana de la serie.
Sintaxis y sobrecargas
ta.median(source, length) → series int
ta.median(source, length) → series float
Argumentos
source (series int) Serie de valores a procesar.
length (series int) Número de barras (longitud)
Devoluciones
La mediana de la serie.
Observaciones
Se ignoran los valores na de la serie source; la función calcula sobre la cantidad length de valores no na.
ta.mfi()

Índice de flujo monetario (MFI). Este índice es un oscilador técnico que utiliza el precio y el volumen para identificar las condiciones de sobrecompra o sobreventa de un activo.
Sintaxis
ta.mfi(series, length) → series float
Argumentos
series (series int/float) Serie de valores a procesar.
length (series int) Número de barras (longitud)
Ejemplo

//@version=5
indicator("Money Flow Index")

plot(ta.mfi(hlc3, 14), color=color.yellow)

// the same on pine
pine_mfi(src, length) =>
    float upper = math.sum(volume * (ta.change(src) <= 0.0 ? 0.0 : src), length)
    float lower = math.sum(volume * (ta.change(src) >= 0.0 ? 0.0 : src), length)
    mfi = 100.0 - (100.0 / (1.0 + upper / lower))
    mfi

plot(pine_mfi(hlc3, 14))
Devoluciones
Índice de flujo monetario.
Observaciones
Se ignoran los valores na de la serie source; la función calcula sobre la cantidad length de valores no na.
Ver también
ta.rsi
math.sum
ta.min()

Devuelve el valor mínimo histórico de source desde el inicio del gráfico hasta la barra actual.
Sintaxis
ta.min(source) → series float
Argumentos
source (series int/float) Fuente utilizada para el cálculo.
Observaciones
Se ignoran las apariciones na de source.
ta.mode()2 sobrecargas

Devuelve el mode de una serie. En el caso de que existan varios valores con la misma frecuencia, devuelve el valor más pequeño.
Sintaxis y sobrecargas
ta.mode(source, length) → series int
ta.mode(source, length) → series float
Argumentos
source (series int) Serie de valores a procesar.
length (series int) Número de barras (longitud)
Devoluciones
Valor más frecuente de source. Si no existe ninguno, devuelve en su lugar el valor más pequeño.
Observaciones
Se ignoran los valores na de la serie source; la función calcula sobre la cantidad length de valores no na.
ta.mom()

Momentum del precio source y del precio source de length barras históricas. Se trata de una simple diferencia: source - source[length].
Sintaxis
ta.mom(source, length) → series float
Argumentos
source (series int/float) Serie de valores a procesar.
length (series int) Compensación de la barra actual respecto a la barra anterior.
Devoluciones
Momentum del precio source y del precio source de length barras históricas.
Observaciones
Los valores na de la serie source se incluyen en los cálculos y generarán un resultado na.
Ver también
ta.change
ta.percentile_linear_interpolation()

Calcula el percentil utilizando un método de interpolación lineal entre los dos rangos más cercanos.
Sintaxis
ta.percentile_linear_interpolation(source, length, percentage) → series float
Argumentos
source (series int/float) Serie de valores a procesar (fuente).
length (series int) Número de barras históricas (longitud).
percentage (simple int/float) Porcentaje, un número entre 0 y 100.
Devoluciones
El percentil P-ésimo de la serie source para length barras históricas.
Observaciones
Observe que un percentil que se calcule utilizando este método NO siempre será un miembro de un conjunto de datos de entrada.
Los valores na de la serie source se incluyen en los cálculos y generarán un resultado na.
Ver también
ta.percentile_nearest_rank
ta.percentile_nearest_rank()

Calcula el percentil utilizando el método del Rango más cercano.
Sintaxis
ta.percentile_nearest_rank(source, length, percentage) → series float
Argumentos
source (series int/float) Serie de valores a procesar (fuente).
length (series int) Número de barras históricas (longitud).
percentage (simple int/float) Porcentaje, un número entre 0 y 100.
Devoluciones
El percentil P-ésimo de la serie source para length barras históricas.
Observaciones
Utilizar el método de rango más cercano en longitudes inferiores a 100 barras históricas puede dar lugar a que se utilice el mismo número para más de un percentil.
Un percentil calculado utilizando el método de rango más cercano siempre será un miembro del conjunto de datos de entrada.
El percentil 100 se define como el valor más grande en el conjunto de datos de entrada.
Se ignoran los valores na de la serie source.
Ver también
ta.percentile_linear_interpolation
ta.percentrank()

El rango de porcentaje son los porcentajes de la cantidad de valores anteriores que fueron menores o iguales al valor actual de una serie dada.
Sintaxis
ta.percentrank(source, length) → series float
Argumentos
source (series int/float) Serie de valores a procesar.
length (series int) Número de barras (longitud)
Devoluciones
Calificación porcentual de source respecto a length barras históricas.
Observaciones
Los valores na de la serie source se incluyen en los cálculos y generarán un resultado na.
ta.pivot_point_levels()

Calcula los niveles de los puntos pivote utilizando los type y anchor especificados.
Sintaxis
ta.pivot_point_levels(type, anchor, developing) → array<float>
Argumentos
type (series string) Tipo de niveles de los puntos pivote. Valores posibles: "Traditional", "Fibonacci", "Woodie", "Clásic", "DM", "Camarilla".
anchor (series bool) Condición que activa el reinicio de los cálculos del punto pivote. Cuando es true, los cálculos se reinician; cuando es false, persisten los resultados que se han calculado en el último reinicio.
developing (series bool) Si la condición es false, los valores son los que se calcularon la última vez que la condición de anclaje fuera true. Permanecen constantes hasta que la condición de anclaje vuelve a ser true. Cuando la condición es true, se desarrollan los pivotes, es decir, se vuelven a calcular de forma constante en función de los datos desarrollados entre el punto del último anclaje (o barra cero si la condición de anclaje nunca hubiera sido true) y la barra actual. Opcional. El valor por defecto es false.
Ejemplo

//@version=5
indicator("Weekly Pivots", max_lines_count=500, overlay=true)
timeframe = "1W"
typeInput = input.string("Traditional", "Type", options=["Traditional", "Fibonacci", "Woodie", "Classic", "DM", "Camarilla"])
weekChange = timeframe.change(timeframe)
pivotPointsArray = ta.pivot_point_levels(typeInput, weekChange)
if weekChange
    for pivotLevel in pivotPointsArray
        line.new(time, pivotLevel, time + timeframe.in_seconds(timeframe) * 1000, pivotLevel, xloc=xloc.bar_time)
Devoluciones
Un array<float> con valores numéricos que representan 11 niveles de punto pivote: [P, R1, S1, R2, S2, R3, S3, R4, S4, R5, S5]. Los niveles ausentes del type especificado devuelven valores na (por ejemplo, "DM" solo calcula P, R1 y S1).
Observaciones
El parámetro developing no puede ser true cuando type se establece como "Woodie", porque el cálculo del período depende de su apertura, lo que significa que el valor pivote puede encontrarse disponible o no, pero en ningún caso puede estar en desarrollo. Si se utiliza de forma conjunta, el indicador devolverá un error de ejecución.
ta.pivothigh()2 sobrecargas

Esta función devuelve el precio del punto pivote alto. Devuelve 'NaN' si no hay un punto pivote alto.
Sintaxis y sobrecargas
ta.pivothigh(leftbars, rightbars) → series float
ta.pivothigh(source, leftbars, rightbars) → series float
Argumentos
leftbars (series int/float) Fuerza izquierda.
rightbars (series int/float) Fuerza derecha.
Ejemplo

//@version=5
indicator("PivotHigh", overlay=true)
leftBars = input(2)
rightBars=input(2)
ph = ta.pivothigh(leftBars, rightBars)
plot(ph, style=plot.style_cross, linewidth=3, color= color.red, offset=-rightBars)
Devoluciones
Precio del punto o 'NaN'.
Observaciones
Si los parámetros 'leftbars' o 'rightbars' son series, debe utilizar la función max_bars_back para la variable 'source'.
ta.pivotlow()2 sobrecargas

Esta función devuelve el precio del punto pivote bajo. Devuelve 'NaN' si no hay un punto pivote bajo.
Sintaxis y sobrecargas
ta.pivotlow(leftbars, rightbars) → series float
ta.pivotlow(source, leftbars, rightbars) → series float
Argumentos
leftbars (series int/float) Fuerza izquierda.
rightbars (series int/float) Fuerza derecha.
Ejemplo

//@version=5
indicator("PivotLow", overlay=true)
leftBars = input(2)
rightBars=input(2)
pl = ta.pivotlow(close, leftBars, rightBars)
plot(pl, style=plot.style_cross, linewidth=3, color= color.blue, offset=-rightBars)
Devoluciones
Precio del punto o 'NaN'.
Observaciones
Si los parámetros 'leftbars' o 'rightbars' son series, debe utilizar la función max_bars_back para la variable 'source'.
ta.range()2 sobrecargas

Devuelve la diferencia entre el valor máximo y mínimo de una serie.
Sintaxis y sobrecargas
ta.range(source, length) → series int
ta.range(source, length) → series float
Argumentos
source (series int) Serie de valores a procesar.
length (series int) Número de barras (longitud)
Devoluciones
La diferencia entre los valores mínimo y máximo de la serie.
Observaciones
Se ignoran los valores na de la serie source; la función calcula sobre la cantidad length de valores no na.
ta.rising()

Compruebe si la serie source está aumentando respecto a la longitud length de las barras.
Sintaxis
ta.rising(source, length) → series bool
Argumentos
source (series int/float) Serie de valores a procesar.
length (series int) Número de barras (longitud)
Devoluciones
true si el source actual es mayor que cualquier source anterior para length barras históricas; de lo contrario, false.
Observaciones
Se ignoran los valores na de la serie source.
Ver también
ta.falling
ta.rma()

Media móvil utilizada en RSI. Es la media móvil ponderada exponencialmente con alfa = 1 / longitud.
Sintaxis
ta.rma(source, length) → series float
Argumentos
source (series int/float) Serie de valores a procesar.
length (simple int) Número de barras (longitud)
Ejemplo

//@version=5
indicator("ta.rma")
plot(ta.rma(close, 15))

//the same on pine
pine_rma(src, length) =>
    alpha = 1/length
    sum = 0.0
    sum := na(sum[1]) ? ta.sma(src, length) : alpha * src + (1 - alpha) * nz(sum[1])
plot(pine_rma(close, 15))
Devoluciones
Media móvil exponencial de source con alpha = 1 / length.
Observaciones
Se ignoran los valores na de la serie source; la función calcula sobre la cantidad length de valores no na.
Ver también
ta.sma
ta.ema
ta.wma
ta.vwma
ta.swma
ta.alma
ta.rsi
ta.roc()

Calcula el porcentaje de cambio (tasa de cambio) entre el valor actual de source y su valor length de barras históricas.
Se calcula usando la siguiente fórmula: 100 * change(src, length) / src[length].
Sintaxis
ta.roc(source, length) → series float
Argumentos
source (series int/float) Serie de valores a procesar.
length (series int) Número de barras (longitud)
Devoluciones
Tasa de cambio de source respecto a ‘`length`` barras históricas.
Observaciones
Los valores na de la serie source se incluyen en los cálculos y generarán un resultado na.
ta.rsi()

Índice de fuerza relativa. Se calcula utilizando la ta.rma() de los cambios al alza y a la baja de source respecto a length de las últimas barras.
Sintaxis
ta.rsi(source, length) → series float
Argumentos
source (series int/float) Serie de valores a procesar.
length (simple int) Número de barras (longitud)
Ejemplo

//@version=5
indicator("ta.rsi")
plot(ta.rsi(close, 7))

// same on pine, but less efficient
pine_rsi(x, y) => 
    u = math.max(x - x[1], 0) // upward ta.change
    d = math.max(x[1] - x, 0) // downward ta.change
    rs = ta.rma(u, y) / ta.rma(d, y)
    res = 100 - 100 / (1 + rs)
    res

plot(pine_rsi(close, 7))
Devoluciones
Índice de fuerza relativa.
Observaciones
Se ignoran los valores na de la serie source; la función calcula sobre la cantidad length de valores no na.
Ver también
ta.rma
ta.sar()

El SAR parabólico (parada y retroceso parabólicos) es un método ideado por J. Welles Wilder, Jr., para encontrar reversiones potenciales en la dirección del precio de mercado de los productos negociados.
Sintaxis
ta.sar(start, inc, max) → series float
Argumentos
start (simple int/float) Inicio.
inc (simple int/float) Incremento.
max (simple int/float) Maximo.
Ejemplo

//@version=5
indicator("ta.sar")
plot(ta.sar(0.02, 0.02, 0.2), style=plot.style_cross, linewidth=3)

// The same on Pine Script®
pine_sar(start, inc, max) =>
    var float result = na
    var float maxMin = na
    var float acceleration = na
    var bool isBelow = na
    bool isFirstTrendBar = false
    
    if bar_index == 1
        if close > close[1]
            isBelow := true
            maxMin := high
            result := low[1]
        else
            isBelow := false
            maxMin := low
            result := high[1]
        isFirstTrendBar := true
        acceleration := start
    
    result := result + acceleration * (maxMin - result)
    
    if isBelow
        if result > low
            isFirstTrendBar := true
            isBelow := false
            result := math.max(high, maxMin)
            maxMin := low
            acceleration := start
    else
        if result < high
            isFirstTrendBar := true
            isBelow := true
            result := math.min(low, maxMin)
            maxMin := high
            acceleration := start
            
    if not isFirstTrendBar
        if isBelow
            if high > maxMin
                maxMin := high
                acceleration := math.min(acceleration + inc, max)
        else
            if low < maxMin
                maxMin := low
                acceleration := math.min(acceleration + inc, max)
    
    if isBelow
        result := math.min(result, low[1])
        if bar_index > 1
            result := math.min(result, low[2])
        
    else
        result := math.max(result, high[1])
        if bar_index > 1
            result := math.max(result, high[2])
    
    result
    
plot(pine_sar(0.02, 0.02, 0.2), style=plot.style_cross, linewidth=3)
Devoluciones
SAR parabólico.
ta.sma()

La función sma devuelve la media móvil, es decir la suma de los últimos valores y de x, divididos por y.
Sintaxis
ta.sma(source, length) → series float
Argumentos
source (series int/float) Serie de valores a procesar.
length (series int) Número de barras (longitud)
Ejemplo

//@version=5
indicator("ta.sma")
plot(ta.sma(close, 15))

// same on pine, but much less efficient
pine_sma(x, y) =>
    sum = 0.0
    for i = 0 to y - 1
        sum := sum + x[i] / y
    sum
plot(pine_sma(close, 15))
Devoluciones
Media móvil simple de source respecto a length barras históricas.
Observaciones
Se ignoran los valores na de la serie source.
Ver también
ta.ema
ta.rma
ta.wma
ta.vwma
ta.swma
ta.alma
ta.stdev()

Sintaxis
ta.stdev(source, length, biased) → series float
Argumentos
source (series int/float) Serie de valores a procesar.
length (series int) Número de barras (longitud)
biased (series bool) Determina el tipo de estimación a usar. Opcional. El valor por defecto es true.
Ejemplo

//@version=5
indicator("ta.stdev")
plot(ta.stdev(close, 5))

//the same on pine
isZero(val, eps) => math.abs(val) <= eps

SUM(fst, snd) =>
    EPS = 1e-10
    res = fst + snd
    if isZero(res, EPS)
        res := 0
    else
        if not isZero(res, 1e-4)
            res := res
        else
            15

pine_stdev(src, length) =>
    avg = ta.sma(src, length)
    sumOfSquareDeviations = 0.0
    for i = 0 to length - 1
        sum = SUM(src[i], -avg)
        sumOfSquareDeviations := sumOfSquareDeviations + sum * sum

    stdev = math.sqrt(sumOfSquareDeviations / length)
plot(pine_stdev(close, 5))
Devoluciones
Desviación estándar.
Observaciones
Si biased es true, la función realizará el cálculo utilizando una estimación sesgada de toda la población y, si es false, la muestra se realizará sin sesgos.
Se ignoran los valores na de la serie source; la función calcula sobre la cantidad length de valores no na.
Ver también
ta.dev
ta.variance
ta.stoch()

Estocástico. Se calcula mediante la fórmula: 100 * (cierre - mínimo(mínimo, longitud)) / (máximo(máximo, longitud) - mínimo(mínimo, longitud)).
Sintaxis
ta.stoch(source, high, low, length) → series float
Argumentos
source (series int/float) Serie de fuentes.
high (series int/float) Serie de máximos.
low (series int/float) Serie de mínimos.
length (series int) Longitud (número de barras históricas).
Devoluciones
Estocástico.
Observaciones
Se ignoran los valores na de la serie source.
Ver también
ta.cog
ta.supertrend()

Indicador Supertrend. Supertrend es un indicador de seguimiento de tendencias.
Sintaxis
ta.supertrend(factor, atrPeriod) → [series float, series float]
Argumentos
factor (series int/float) El multiplicador por el cual se multiplicará el ATR.
atrPeriod (simple int) Longitud ATR.
Ejemplo

//@version=5
indicator("Pine Script® Supertrend")

[supertrend, direction] = ta.supertrend(3, 10)
plot(direction < 0 ? supertrend : na, "Up direction", color = color.green, style=plot.style_linebr)
plot(direction > 0 ? supertrend : na, "Down direction", color = color.red, style=plot.style_linebr)

// The same on Pine Script®
pine_supertrend(factor, atrPeriod) =>
    src = hl2
    atr = ta.atr(atrPeriod)
    upperBand = src + factor * atr
    lowerBand = src - factor * atr
    prevLowerBand = nz(lowerBand[1])
    prevUpperBand = nz(upperBand[1])

    lowerBand := lowerBand > prevLowerBand or close[1] < prevLowerBand ? lowerBand : prevLowerBand
    upperBand := upperBand < prevUpperBand or close[1] > prevUpperBand ? upperBand : prevUpperBand
    int _direction = na
    float superTrend = na
    prevSuperTrend = superTrend[1]
    if na(atr[1])
        _direction := 1
    else if prevSuperTrend == prevUpperBand
        _direction := close > upperBand ? -1 : 1
    else
        _direction := close < lowerBand ? 1 : -1
    superTrend := _direction == -1 ? lowerBand : upperBand
    [superTrend, _direction]

[Pine_Supertrend, pineDirection] = pine_supertrend(3, 10)
plot(pineDirection < 0 ? Pine_Supertrend : na, "Up direction", color = color.green, style=plot.style_linebr)
plot(pineDirection > 0 ? Pine_Supertrend : na, "Down direction", color = color.red, style=plot.style_linebr)
Devoluciones
Tupla de dos series supertrend: línea de supertendencia y dirección de la tendencia. Los valores posibles son 1 (dirección descendente) y -1 (dirección ascendente).
Ver también
ta.macd
ta.swma()

Media móvil ponderada simétricamente con longitud fija: 4. Pesos: [1/6, 2/6, 2/6, 1/6].
Sintaxis
ta.swma(source) → series float
Argumentos
source (series int/float) Serie de fuentes.
Ejemplo

//@version=5
indicator("ta.swma")
plot(ta.swma(close))

// same on pine, but less efficient
pine_swma(x) =>
    x[3] * 1 / 6 + x[2] * 2 / 6 + x[1] * 2 / 6 + x[0] * 1 / 6
plot(pine_swma(close))
Devoluciones
Media móvil ponderada simétricamente.
Observaciones
Los valores na de la serie source se incluyen en los cálculos y generarán un resultado na.
Ver también
ta.sma
ta.ema
ta.rma
ta.wma
ta.vwma
ta.alma
ta.tr()

Sintaxis
ta.tr(handle_na) → series float
Argumentos
handle_na (simple bool) Forma en la que se gestionan los valores NaN. Si es true y el cierre del día anterior es NaN, entonces tr se calcula como el máx-mín del día actual. En caso contrario, (es decir, si es false), tr devolverá NaN. Tenga en cuenta que ta.atr utiliza ta.tr(true).
Devoluciones
Rango true: math.max(high - low, math.abs(high - close[1]), math.abs(low - close[1])).
Observaciones
ta.tr(false) es exactamente lo mismo que ta.tr.
Ver también
ta.tr
ta.atr
ta.tsi()

Índice de fuerza verdadera. Utiliza las medias móviles del momentum subyacente de un instrumento financiero.
Sintaxis
ta.tsi(source, short_length, long_length) → series float
Argumentos
source (series int/float) Serie de fuentes.
short_length (simple int) Longitud corta.
long_length (simple int) Longitud larga.
Devoluciones
Índice de fuerza verdadera. Un valor en rango [-1, 1].
Observaciones
Se ignoran los valores na de la serie source; la función calcula sobre la cantidad length de valores no na.
ta.valuewhen()4 sobrecargas

Devuelve el valor de la serie source en la barra donde condition fuera true en la enésima (nth) ocurrencia más reciente.
Sintaxis y sobrecargas
ta.valuewhen(condition, source, occurrence) → series color
ta.valuewhen(condition, source, occurrence) → series int
ta.valuewhen(condition, source, occurrence) → series float
ta.valuewhen(condition, source, occurrence) → series bool
Argumentos
condition (series bool) La condición a buscar.
source (series color) El valor a devolver de la barra en la que se cumple la condición.
occurrence (simple int) Ocurrencia de la condición. La numeración empieza a partir de 0 y retrocede en el tiempo, de forma que '0' es la ocurrencia más reciente de la condition, '1' es el segundo más reciente y así sucesivamente. Debe ser un número entero >= 0.
Ejemplo

//@version=5
indicator("ta.valuewhen")
slow = ta.sma(close, 7)
fast = ta.sma(close, 14)
// Get value of `close` on second most recent cross
plot(ta.valuewhen(ta.cross(slow, fast), close, 1))
Observaciones
Esta función debe ejecutarse en cada barra. No se recomienda utilizarla dentro de una estructura de bucle for o while, ya que podría generar unos resultados inesperados. Tenga en cuenta que usar esta función puede provocar un repintado del indicador.
Ver también
ta.lowestbars
ta.highestbars
ta.barssince
ta.highest
ta.lowest
ta.variance()

La varianza es la expectativa de la desviación cuadrada de una serie de su media (ta.sma) y mide, de forma informal, hasta qué punto un conjunto de números se separan de su media.
Sintaxis
ta.variance(source, length, biased) → series float
Argumentos
source (series int/float) Serie de valores a procesar.
length (series int) Número de barras (longitud)
biased (series bool) Determina el tipo de estimación a usar. Opcional. El valor por defecto es true.
Devoluciones
Variación de source respecto a length barras históricas.
Observaciones
Si biased es true, la función realizará el cálculo utilizando una estimación sesgada de toda la población y, si es false, la muestra se realizará sin sesgos.
Se ignoran los valores na de la serie source; la función calcula sobre la cantidad length de valores no na.
Ver también
ta.dev
ta.stdev
ta.vwap()3 sobrecargas

Precio medio ponderado por volumen.
Sintaxis y sobrecargas
ta.vwap(source) → series float
ta.vwap(source, anchor) → series float
ta.vwap(source, anchor, stdev_mult) → [series float, series float, series float]
Argumentos
source (series int/float) Fuente utilizada para el cálculo del VWAP.
Ejemplo

//@version=5
indicator("Simple VWAP")
vwap = ta.vwap(open)
plot(vwap)
Ejemplo

//@version=5
indicator("Advanced VWAP")
vwapAnchorInput = input.string("Daily", "Anchor", options = ["Daily", "Weekly", "Monthly"])
stdevMultiplierInput = input.float(1.0, "Standard Deviation Multiplier")
anchorTimeframe = switch vwapAnchorInput
    "Daily"   => "1D"
    "Weekly"  => "1W"
    "Monthly" => "1M"
anchor = timeframe.change(anchorTimeframe)
[vwap, upper, lower] = ta.vwap(open, anchor, stdevMultiplierInput)
plot(vwap)
plot(upper, color = color.green)
plot(lower, color = color.green)
Devoluciones
Serie VWAP o tupla [vwap, upper_band, lower_band] si se especifica stdev_mult.
Observaciones
Los cálculos solo comienzan la primera vez que la condición de anclaje se convierte en true. Hasta ese momento, la función devuelve na.
Ver también
ta.vwap
ta.vwma()

La función vwma devuelve la media móvil ponderada por volumen de source respecto a length barras históricas. Es lo mismo que: sma(source * volume, length) / sma(volume, length).
Sintaxis
ta.vwma(source, length) → series float
Argumentos
source (series int/float) Serie de valores a procesar.
length (series int) Número de barras (longitud)
Ejemplo

//@version=5
indicator("ta.vwma")
plot(ta.vwma(close, 15))

// same on pine, but less efficient
pine_vwma(x, y) =>
    ta.sma(x * volume, y) / ta.sma(volume, y)
plot(pine_vwma(close, 15))
Devoluciones
Media móvil ponderada por volumen de source respecto a length barras históricas.
Observaciones
Se ignoran los valores na de la serie source.
Ver también
ta.sma
ta.ema
ta.rma
ta.wma
ta.swma
ta.alma
ta.wma()

La función wma devuelve la media móvil ponderada de source respecto a length barras históricas. En wma los factores de ponderación disminuyen en progresión aritmética.
Sintaxis
ta.wma(source, length) → series float
Argumentos
source (series int/float) Serie de valores a procesar.
length (series int) Número de barras (longitud)
Ejemplo

//@version=5
indicator("ta.wma")
plot(ta.wma(close, 15))

// same on pine, but much less efficient
pine_wma(x, y) =>
    norm = 0.0
    sum = 0.0
    for i = 0 to y - 1
        weight = (y - i) * y
        norm := norm + weight
        sum := sum + x[i] * weight
    sum / norm
plot(pine_wma(close, 15))
Devoluciones
Media móvil ponderada de source respecto a length barras históricas.
Observaciones
Se ignoran los valores na de la serie source.
Ver también
ta.sma
ta.ema
ta.rma
ta.vwma
ta.swma
ta.alma
ta.wpr()

Williams %R. El oscilador muestra el precio de cierre actual respecto a los máximos y mínimos de las barras 'length' anteriores.
Sintaxis
ta.wpr(length) → series float
Argumentos
length (series int) Número de barras.
Ejemplo

//@version=5
indicator("Williams %R", shorttitle="%R", format=format.price, precision=2)
plot(ta.wpr(14), title="%R", color=color.new(#ff6d00, 0))
Devoluciones
Williams %R.
Observaciones
Se ignoran los valores na de la serie source.
Ver también
ta.mfi
ta.cmo
table()

Convierte na en table
Sintaxis
table(x) → series table
Argumentos
x (series table) Valor a convertir al tipo especificado, normalmente na.
Devoluciones
Valor del argumento después de convertirse en table.
Ver también
float
int
bool
color
string
line
label
table.cell()

Función que define una celda en la tabla y establece sus atributos.
Sintaxis
table.cell(table_id, column, row, text, width, height, text_color, text_halign, text_valign, text_size, bgcolor, tooltip, text_font_family) → void
Argumentos
table_id (series table) Un objeto table.
column (series int) Índice de la columna de la celda. La numeración comienza en 0.
row (series int) Índice de la fila de la celda. La numeración comienza en 0.
text (series string) Texto que se mostrará dentro de la celda. Opcional. El valor por defecto es una cadena vacía.
width (series int/float) Ancho de la celda expresado en porcentaje del espacio visual del indicador. Opcional. Por defecto, se ajusta de forma automática la altura en función del texto que contenga la celda. El valor 0 tiene el mismo efecto.
height (series int/float) Altura de la celda expresada en porcentaje del espacio visual del indicador. Opcional. Por defecto, se ajusta de forma automática la altura en función del texto que contenga la celda. El valor 0 tiene el mismo efecto.
text_color (series color) Color del texto. Opcional. El valor por defecto es color.black.
text_halign (series string) Alineación horizontal del texto de la celda. Opcional. El valor por defecto es text.align_center. Valores posibles text.align_left, text.align_center, text.align_right.
text_valign (series string) Alineación vertical del texto de la celda. Opcional. El valor por defecto es text.align_center. Valores posibles text.align_top, text.align_center, text.align_bottom.
text_size (series string) Tamaño del texto. Parámetro opcional cuyo valor por defecto es size.normal. Valores posibles: size.auto, size.tiny, size.small, size.normal, size.large, size.huge.
bgcolor (series color) Color de fondo del texto. Opcional. Por defecto sin color.
tooltip (series string) La descripción emergente que se mostrará dentro de la celda. Opcional.
text_font_family (series string) Familia de fuentes del texto. Opcional. El valor por defecto es font.family_default. Valores posibles: font.family_default, font.family_monospace.
Observaciones
Esta función no crea la tabla en sí, sino que define sus celdas. Para usarla, primero necesita crear un objeto table con table.new.
Cada llamada a table.cell sobrescribe todas las propiedades previamente definidas en una celda. Si activa table.cell dos veces seguidas, por ejemplo, la primera vez con text='Test Text' y la segunda con text_color=color.red pero sin un nuevo argumento de texto, siendo el valor por defecto del 'text' una cadena vacía, sobrescribirá 'Test Text', y su celda mostrará una cadena vacía. Si por el contrario, desea modificar cualquiera de las propiedades de la celda, utilice las funciones table.cell_set_*().
Un único script solo puede mostrar una tabla en cada posición posible. Si se aplica table.cell a varias barras para cambiar el mismo atributo de una celda (por ejemplo, cambiar el color de fondo de la celda a rojo en la primera barra y después a amarillo en la segunda), solo se reflejará en la tabla el último cambio, es decir, el fondo de la celda será amarillo. Siempre que sea posible, evite cualquier configuración innecesaria en las propiedades de celda, colocando una llamada de función en un bloque if barstate.islast, de forma que se restrinja su ejecución a la última barra de la serie.
Ver también
table.cell_set_bgcolor
table.cell_set_height
table.cell_set_text
table.cell_set_text_color
table.cell_set_text_halign
table.cell_set_text_size
table.cell_set_text_valign
table.cell_set_width
table.cell_set_tooltip
table.cell_set_bgcolor()

Función que establece el color de fondo de la celda.
Sintaxis
table.cell_set_bgcolor(table_id, column, row, bgcolor) → void
Argumentos
table_id (series table) Un objeto table.
column (series int) Índice de la columna de la celda. La numeración comienza en 0.
row (series int) Índice de la fila de la celda. La numeración comienza en 0.
bgcolor (series color) Color de fondo de la celda.
Ver también
table.cell_set_height
table.cell_set_text
table.cell_set_text_color
table.cell_set_text_halign
table.cell_set_text_size
table.cell_set_text_valign
table.cell_set_width
table.cell_set_tooltip
table.cell_set_height()

Función que establece la altura de la celda.
Sintaxis
table.cell_set_height(table_id, column, row, height) → void
Argumentos
table_id (series table) Un objeto table.
column (series int) Índice de la columna de la celda. La numeración comienza en 0.
row (series int) Índice de la fila de la celda. La numeración comienza en 0.
height (series int/float) Altura de la celda expresada en porcentaje de la ventana del gráfico. Pasar 0 ajusta de forma automática la altura en función del texto que contenga la celda.
Ver también
table.cell_set_bgcolor
table.cell_set_text
table.cell_set_text_color
table.cell_set_text_halign
table.cell_set_text_size
table.cell_set_text_valign
table.cell_set_width
table.cell_set_tooltip
table.cell_set_text()

Función que establece el texto en la celda especificada.
Sintaxis
table.cell_set_text(table_id, column, row, text) → void
Argumentos
table_id (series table) Un objeto table.
column (series int) Índice de la columna de la celda. La numeración comienza en 0.
row (series int) Índice de la fila de la celda. La numeración comienza en 0.
text (series string) Texto que se mostrará dentro de la celda.
Ejemplo

//@version=5
indicator("TABLE example")
var tLog = table.new(position = position.top_left, rows = 1, columns = 2, bgcolor = color.yellow, border_width=1)
table.cell(tLog, row = 0, column = 0, text = "sometext", text_color = color.blue)
table.cell_set_text(tLog, row = 0, column = 0, text = "sometext")
Ver también
table.cell_set_bgcolor
table.cell_set_height
table.cell_set_text_color
table.cell_set_text_halign
table.cell_set_text_size
table.cell_set_text_valign
table.cell_set_width
table.cell_set_tooltip
table.cell_set_text_color()

Función que establece el color del texto dentro de la celda.
Sintaxis
table.cell_set_text_color(table_id, column, row, text_color) → void
Argumentos
table_id (series table) Un objeto table.
column (series int) Índice de la columna de la celda. La numeración comienza en 0.
row (series int) Índice de la fila de la celda. La numeración comienza en 0.
text_color (series color) Color del texto.
Ver también
table.cell_set_bgcolor
table.cell_set_height
table.cell_set_text
table.cell_set_text_halign
table.cell_set_text_size
table.cell_set_text_valign
table.cell_set_width
table.cell_set_tooltip
table.cell_set_text_font_family()

Función que establece la familia de fuentes del texto dentro de la celda.
Sintaxis
table.cell_set_text_font_family(table_id, column, row, text_font_family) → void
Argumentos
table_id (series table) Un objeto table.
column (series int) Índice de la columna de la celda. La numeración comienza en 0.
row (series int) Índice de la fila de la celda. La numeración comienza en 0.
text_font_family (series string) Familia de fuentes del texto. Valores posibles: font.family_default, font.family_monospace.
Ejemplo

//@version=5
indicator("Example of setting the table cell font")
var t = table.new(position.top_left, rows = 1, columns = 1)
table.cell(t, 0, 0, "monospace", text_color = color.blue)
table.cell_set_text_font_family(t, 0, 0, font.family_monospace)
Ver también
table.new
font.family_default
font.family_monospace
table.cell_set_text_halign()

Función que establece la alineación horizontal del texto de la celda.
Sintaxis
table.cell_set_text_halign(table_id, column, row, text_halign) → void
Argumentos
table_id (series table) Un objeto table.
column (series int) Índice de la columna de la celda. La numeración comienza en 0.
row (series int) Índice de la fila de la celda. La numeración comienza en 0.
text_halign (series string) Alineación horizontal del texto de una celda. Valores posibles: text.align_left, text.align_center, text.align_right.
Ver también
table.cell_set_bgcolor
table.cell_set_height
table.cell_set_text
table.cell_set_text_color
table.cell_set_text_size
table.cell_set_text_valign
table.cell_set_width
table.cell_set_tooltip
table.cell_set_text_size()

Función que establece el tamaño del texto de la celda.
Sintaxis
table.cell_set_text_size(table_id, column, row, text_size) → void
Argumentos
table_id (series table) Un objeto table.
column (series int) Índice de la columna de la celda. La numeración comienza en 0.
row (series int) Índice de la fila de la celda. La numeración comienza en 0.
text_size (series string) Tamaño del texto. Valores posibles: size.auto, size.tiny, size.small, size.normal, size.large, size.huge.
Ver también
table.cell_set_bgcolor
table.cell_set_height
table.cell_set_text
table.cell_set_text_color
table.cell_set_text_halign
table.cell_set_text_valign
table.cell_set_width
table.cell_set_tooltip
table.cell_set_text_valign()

Función que establece la alineación vertical del texto de una celda.
Sintaxis
table.cell_set_text_valign(table_id, column, row, text_valign) → void
Argumentos
table_id (series table) Un objeto table.
column (series int) Índice de la columna de la celda. La numeración comienza en 0.
row (series int) Índice de la fila de la celda. La numeración comienza en 0.
text_valign (series string) Alineación vertical del texto de la celda. Valores posibles: text.align_top, text.align_center, text.align_bottom.
Ver también
table.cell_set_bgcolor
table.cell_set_height
table.cell_set_text
table.cell_set_text_color
table.cell_set_text_halign
table.cell_set_text_size
table.cell_set_width
table.cell_set_tooltip
table.cell_set_tooltip()

Función que establece la descripción emergente en la celda especificada.
Sintaxis
table.cell_set_tooltip(table_id, column, row, tooltip) → void
Argumentos
table_id (series table) Un objeto table.
column (series int) Índice de la columna de la celda. La numeración comienza en 0.
row (series int) Índice de la fila de la celda. La numeración comienza en 0.
tooltip (series string) La descripción emergente que se mostrará dentro de la celda.
Ejemplo

//@version=5
indicator("TABLE example")
var tLog = table.new(position = position.top_left, rows = 1, columns = 2, bgcolor = color.yellow, border_width=1)
table.cell(tLog, row = 0, column = 0, text = "sometext", text_color = color.blue)
table.cell_set_tooltip(tLog, row = 0, column = 0, tooltip = "sometext")
Ver también
table.cell_set_bgcolor
table.cell_set_height
table.cell_set_text_color
table.cell_set_text_halign
table.cell_set_text_size
table.cell_set_text_valign
table.cell_set_width
table.cell_set_text
table.cell_set_width()

Función que establece el ancho de la celda.
Sintaxis
table.cell_set_width(table_id, column, row, width) → void
Argumentos
table_id (series table) Un objeto table.
column (series int) Índice de la columna de la celda. La numeración comienza en 0.
row (series int) Índice de la fila de la celda. La numeración comienza en 0.
width (series int/float) Ancho de la celda expresado en porcentaje de la ventana del gráfico. Pasar 0 ajusta de forma automática el ancho en función del texto que contenga la celda.
Ver también
table.cell_set_bgcolor
table.cell_set_height
table.cell_set_text
table.cell_set_text_color
table.cell_set_text_halign
table.cell_set_text_size
table.cell_set_text_valign
table.cell_set_tooltip
table.clear()

La función elimina una celda o una secuencia de celdas de la tabla. Las celdas se eliminan en forma de rectángulo donde start_column y start_row especifican la esquina superior izquierda, así como end_column y end_row la esquina inferior derecha.
Sintaxis
table.clear(table_id, start_column, start_row, end_column, end_row) → void
Argumentos
table_id (series table) Un objeto table.
start_column (series int) Índice de la columna de la primera celda a eliminar. La numeración comienza en 0.
start_row (series int) Índice de la fila de la primera celda a eliminar. La numeración comienza en 0.
end_column (series int) Índice de la columna de la última celda a eliminar. Opcional. Por defecto, es el mismo argumento utilizado para start_column. La numeración comienza en 0.
end_row (series int) Índice de la fila de la última celda que se eliminará. Opcional. Por defecto es el mismo argumento utilizado para start_row. La numeración comienza a partir de 0.
Ejemplo

//@version=5
indicator("A donut", overlay=true)
if barstate.islast
    colNum = 8, rowNum = 8
    padding = "◯"
    donutTable = table.new(position.middle_right, colNum, rowNum)
    for c = 0 to colNum - 1
        for r = 0 to rowNum - 1
            table.cell(donutTable, c, r, text=padding, bgcolor=#face6e, text_color=color.new(color.black, 100))
    table.clear(donutTable, 2, 2, 5, 5)
Ver también
table.delete
table.new
table.delete()

Función que elimina una tabla.
Sintaxis
table.delete(table_id) → void
Argumentos
table_id (series table) Un objeto table.
Ejemplo

//@version=5
indicator("table.delete example")
var testTable = table.new(position = position.top_right, columns = 2, rows = 1, bgcolor = color.yellow, border_width = 1)
if barstate.islast
    table.cell(table_id = testTable, column = 0, row = 0, text = "Open is " + str.tostring(open))
    table.cell(table_id = testTable, column = 1, row = 0, text = "Close is " + str.tostring(close), bgcolor=color.teal)
if barstate.isrealtime
    table.delete(testTable)
Ver también
table.new
table.clear
table.merge_cells()

La función combina la secuencia de celdas de la tabla en una sola celda. Las celdas se combinan en forma de rectángulo donde start_column y start_row especifican la esquina superior izquierda, y end_column y end_row determinan la esquina inferior derecha.
Sintaxis
table.merge_cells(table_id, start_column, start_row, end_column, end_row) → void
Argumentos
table_id (series table) Un objeto table.
start_column (series int) Índice de la columna de la primera celda a combinar. La numeración comienza en 0.
start_row (series int) Índice de la fila de la primera celda a combinar. La numeración comienza en 0.
end_column (series int) Índice de la columna de la última celda a combinar. La numeración comienza en 0.
end_row (series int) Índice de la fila de la última celda a combinar. La numeración comienza en 0.
Ejemplo

//@version=5
indicator("table.merge_cells example")
SMA50  = ta.sma(close, 50)
SMA100 = ta.sma(close, 100)
SMA200 = ta.sma(close, 200)
if barstate.islast
    maTable = table.new(position.bottom_right, 3, 3, bgcolor = color.gray, border_width = 1, border_color = color.black)
    // Header
    table.cell(maTable, 0, 0, text = "SMA Table")
    table.merge_cells(maTable, 0, 0, 2, 0)
    // Cell Titles
    table.cell(maTable, 0, 1, text = "SMA 50")
    table.cell(maTable, 1, 1, text = "SMA 100")
    table.cell(maTable, 2, 1, text = "SMA 200")
    // Values
    table.cell(maTable, 0, 2, bgcolor = color.white, text = str.tostring(SMA50))
    table.cell(maTable, 1, 2, bgcolor = color.white, text = str.tostring(SMA100))
    table.cell(maTable, 2, 2, bgcolor = color.white, text = str.tostring(SMA200))
Observaciones
Esta función combinará las celdas, incluso cuando sus propiedades aún no estén definidas con table.cell.
La celda combinada resultante hereda todos sus valores de la celda localizada en start_column:start_row, excepto el ancho y alto, que se basan en otras celdas de las columnas y filas vecinas, por lo que no se pueden establecer manualmente.
Para modificar la celda combinada con cualquiera de las funciones table.cell_set_*, apunte a la celda en las coordenadas start_column:start_row.
Se generará un error cuando se intente combinar una celda a la que ya se hubiera realizado una combinación.
Ver también
table.delete
table.new
table.new()

Función que crea una nueva tabla.
Sintaxis
table.new(position, columns, rows, bgcolor, frame_color, frame_width, border_color, border_width, force_overlay) → series table
Argumentos
position (series string) Posición de la tabla. Los valores posibles son: position.top_left, position.top_center, position.top_right, position.middle_left, position.middle_center, position.middle_right, position.bottom_left, position.bottom_center, position.bottom_right.
columns (series int) Número de columnas de la tabla.
rows (series int) Número de filas de la tabla.
bgcolor (series color) Color de fondo de la tabla. Opcional. Por defecto sin color.
frame_color (series color) Color del marco exterior de la tabla. Opcional. Por defecto sin color.
frame_width (series int) Ancho del marco exterior de la tabla. Opcional. El valor por defecto es 0.
border_color (series color) Color de los bordes de las celdas (excluyendo el marco exterior). Opcional. Por defecto sin color.
border_width (series int) Ancho de los bordes de las celdas (excluyendo el marco exterior). Opcional. El valor por defecto es 0.
force_overlay (const bool) Si es true, el dibujo se mostrará en el panel principal del gráfico, incluso cuando el script ocupe un panel separado. Es opcional. Por defecto es false.
Ejemplo

//@version=5
indicator("table.new example")
var testTable = table.new(position = position.top_right, columns = 2, rows = 1, bgcolor = color.yellow, border_width = 1)
if barstate.islast
    table.cell(table_id = testTable, column = 0, row = 0, text = "Open is " + str.tostring(open))
    table.cell(table_id = testTable, column = 1, row = 0, text = "Close is " + str.tostring(close), bgcolor=color.teal)
Devoluciones
ID de un objeto de tabla que se puede pasar a otras funciones table.*().
Observaciones
Esta función crea el propio objeto table, pero no se mostrará hasta que sus celdas estén rellenas. Para definir una celda y cambiar su contenido o atributos, use table.cell y otras funciones de table.cell_*().
Una llamada a table.new solo puede mostrar una tabla (la última dibujada), pero la propia función se recalculará en cada barra en la que se utilice. Por razones de rendimiento, es aconsejable utilizar table.new junto con la palabra clave var (de forma que el objeto de la tabla se cree solo en la primera barra) o en un bloque if barstate.islast (para que el objeto de la tabla solo se cree en la última barra).
Ver también
table.cell
table.clear
table.delete
table.set_bgcolor
table.set_border_color
table.set_border_width
table.set_frame_color
table.set_frame_width
table.set_position
table.set_bgcolor()

Función que establece el color de fondo de una tabla.
Sintaxis
table.set_bgcolor(table_id, bgcolor) → void
Argumentos
table_id (series table) Un objeto table.
bgcolor (series color) Color de fondo de la tabla. Opcional. Por defecto sin color.
Ver también
table.clear
table.delete
table.new
table.set_border_color
table.set_border_width
table.set_frame_color
table.set_frame_width
table.set_position
table.set_border_color()

Función que establece el color de los bordes de las celdas dentro de la tabla (excluyendo el marco exterior).
Sintaxis
table.set_border_color(table_id, border_color) → void
Argumentos
table_id (series table) Un objeto table.
border_color (series color) Color de los bordes. Opcional. Por defecto sin color.
Ver también
table.clear
table.delete
table.new
table.set_frame_color
table.set_border_width
table.set_bgcolor
table.set_frame_width
table.set_position
table.set_border_width()

Función que establece el ancho de los bordes de las celdas dentro de la tabla (excluyendo el marco exterior).
Sintaxis
table.set_border_width(table_id, border_width) → void
Argumentos
table_id (series table) Un objeto table.
border_width (series int) Ancho de los bordes. Opcional. El valor por defecto es 0.
Ver también
table.clear
table.delete
table.new
table.set_frame_color
table.set_frame_width
table.set_bgcolor
table.set_border_color
table.set_position
table.set_frame_color()

Función que establece el color del marco exterior de una tabla.
Sintaxis
table.set_frame_color(table_id, frame_color) → void
Argumentos
table_id (series table) Un objeto table.
frame_color (series color) Color del marco de la tabla. Opcional. Por defecto sin color.
Ver también
table.clear
table.delete
table.new
table.set_border_color
table.set_border_width
table.set_bgcolor
table.set_frame_width
table.set_position
table.set_frame_width()

Función que establece el ancho del marco exterior de una tabla.
Sintaxis
table.set_frame_width(table_id, frame_width) → void
Argumentos
table_id (series table) Un objeto table.
frame_width (series int) Ancho del marco exterior de la tabla. Opcional. El valor por defecto es 0.
Ver también
table.clear
table.delete
table.new
table.set_frame_color
table.set_border_width
table.set_bgcolor
table.set_border_color
table.set_position
table.set_position()

Función que establece la posición de una tabla.
Sintaxis
table.set_position(table_id, position) → void
Argumentos
table_id (series table) Un objeto table.
position (series string) Posición de la tabla. Los valores posibles son: position.top_left, position.top_center, position.top_right, position.middle_left, position.middle_center, position.middle_right, position.bottom_left, position.bottom_center, position.bottom_right.
Ver también
table.clear
table.delete
table.new
table.set_bgcolor
table.set_border_color
table.set_border_width
table.set_frame_color
table.set_frame_width
ticker.heikinashi()

Crea un identificador de ticker para solicitar valores de barras Heiken Ashi.
Sintaxis
ticker.heikinashi(symbol) → simple string
Argumentos
symbol (simple string) Identificador de símbolo ticker.
Ejemplo

//@version=5
indicator("ticker.heikinashi", overlay=true) 
heikinashi_close = request.security(ticker.heikinashi(syminfo.tickerid), timeframe.period, close)

heikinashi_aapl_60_close = request.security(ticker.heikinashi("AAPL"), "60", close)
plot(heikinashi_close)
plot(heikinashi_aapl_60_close)
Devoluciones
Valor de la cadena del id del ticker que se puede proporcionar a la función request.security.
Ver también
syminfo.tickerid
syminfo.ticker
request.security
ticker.renko
ticker.linebreak
ticker.kagi
ticker.pointfigure
ticker.inherit()

Construye un ID de ticker para el symbol especificado con parámetros adicionales heredados del ID de ticker pasado en la llamada a la función, permitiendo al script solicitar los datos de un símbolo utilizando los mismos modificadores que tiene el from_tickerid, entre los que se incluyen: la sesión de horario ampliado, el ajuste de dividendos, la conversión de divisas, los tipos de gráficos no estándar, el ajuste retroactivo, la liquidación al cierre, etc.
Sintaxis
ticker.inherit(from_tickerid, symbol) → simple string
Argumentos
from_tickerid (simple string) El ID del ticker del que heredar los modificadores.
symbol (simple string) El símbolo para el que se creará el nuevo ID del ticker.
Ejemplo

//@version=5
indicator("ticker.inherit")

//@variable A "NASDAQ:AAPL" ticker ID with Extender Hours enabled.
tickerExtHours = ticker.new("NASDAQ", "AAPL", session.extended)
//@variable A Heikin Ashi ticker ID for "NASDAQ:AAPL" with Extended Hours enabled.
HAtickerExtHours = ticker.heikinashi(tickerExtHours)
//@variable The "NASDAQ:MSFT" symbol with no modifiers.
testSymbol = "NASDAQ:MSFT"
//@variable A ticker ID for "NASDAQ:MSFT" with inherited Heikin Ashi and Extended Hours modifiers.
testSymbolHAtickerExtHours = ticker.inherit(HAtickerExtHours, testSymbol)

//@variable The `close` price requested using "NASDAQ:MSFT" with inherited modifiers. 
secData = request.security(testSymbolHAtickerExtHours, "60", close, ignore_invalid_symbol = true)
//@variable The `close` price requested using "NASDAQ:MSFT" without modifiers. 
compareData = request.security(testSymbol, "60", close, ignore_invalid_symbol = true)

plot(secData, color = color.green)
plot(compareData)
Observaciones
Si el ID del ticker construido hereda un modificador que no se aplica al símbolo (por ejemplo, si el from_tickerid tiene activada la opción Horario ampliado, pero no existe dicha opción para el symbol), el script ignorará el modificador cuando solicite datos utilizando el ID.
ticker.kagi()

Crea un identificador de ticker para solicitar valores Kagi.
Sintaxis
ticker.kagi(symbol, reversal) → simple string
Argumentos
symbol (simple string) Identificador de símbolo ticker.
reversal (simple int/float) Importe de reversión (valor absoluto del precio).
Ejemplo

//@version=5
indicator("ticker.kagi", overlay=true) 
kagi_tickerid = ticker.kagi(syminfo.tickerid, 3)
kagi_close = request.security(kagi_tickerid, timeframe.period, close)
plot(kagi_close)
Devoluciones
Valor de la cadena del id del ticker que se puede proporcionar a la función request.security.
Ver también
syminfo.tickerid
syminfo.ticker
request.security
ticker.heikinashi
ticker.renko
ticker.linebreak
ticker.pointfigure
ticker.linebreak()

Crea un identificador de ticker para solicitar valores de ruptura de línea.
Sintaxis
ticker.linebreak(symbol, number_of_lines) → simple string
Argumentos
symbol (simple string) Identificador de símbolo ticker.
number_of_lines (simple int) Número de linea.
Ejemplo

//@version=5
indicator("ticker.linebreak", overlay=true) 
linebreak_tickerid = ticker.linebreak(syminfo.tickerid, 3)
linebreak_close = request.security(linebreak_tickerid, timeframe.period, close)
plot(linebreak_close)
Devoluciones
Valor de la cadena del id del ticker que se puede proporcionar a la función request.security.
Ver también
syminfo.tickerid
syminfo.ticker
request.security
ticker.heikinashi
ticker.renko
ticker.kagi
ticker.pointfigure
ticker.modify()

Crea un identificador de ticker para solicitar datos adicionales para el script.
Sintaxis
ticker.modify(tickerid, session, adjustment, backadjustment, settlement_as_close) → simple string
Argumentos
tickerid (simple string) Nombre del símbolo con el prefijo del mercado de valores, por ejemplo 'BATS:MSFT', 'NASDAQ:MSFT' o tickerid con la sesión y ajuste desde la función ticker.new.
session (simple string) Tipo de sesión. Argumento opcional. Valores posibles: session.regular, session.extended. El tipo de sesión del gráfico actual es syminfo.session. Si no se indica la sesión, se utiliza el valor syminfo.session.
adjustment (simple string) Tipo de ajuste. Argumento opcional. Valores posibles: adjustment.none, adjustment.splits, adjustment.dividends. Si no se determina el tipo de ajuste, se utiliza el valor establecido por defecto (puede que sea diferente, en función el instrumento que se utiliza).
backadjustment (simple backadjustment) Especifica si los datos de contratos pasados en símbolos de futuros continuos se ajustan retroactivamente. Este ajuste solo afecta a los datos de los símbolos que tienen esta opción disponible en sus gráficos y es opcional. El valor por defecto es backadjustment.inherit, lo que significa que el identificador de ticker modificado hereda el ajuste del identificador de ticker pasado al parámetro tickerid o hereda el valor por defecto del símbolo si el tickerid no especifica este ajuste. Valores posibles: backadjustment.inherit, backadjustment.on, backadjustment.off.
settlement_as_close (simple settlement) Especifica si el valor close de un símbolo de futuros representa el precio de cierre real o el precio de liquidación en "1D" y marcos de tiempo superiores. Este ajuste solo afecta a los datos de los símbolos con esta opción disponible en sus gráficos y es opcional. El valor por defecto es settlement_as_close.inherit, lo que significa que el ticker ID modificado hereda el ajuste del tickerid pasado a la función o hereda el valor por defecto del símbolo del gráfico si el tickerid no especifica este ajuste. Valores posibles: settlement_as_close.inherit, settlement_as_close.on, settlement_as_close.off.
Ejemplo

//@version=5
indicator("ticker_modify", overlay=true)
t1 = ticker.new(syminfo.prefix, syminfo.ticker, session.regular, adjustment.splits)
c1 = request.security(t1, "D", close)
t2 = ticker.modify(t1, session.extended)
c2 = request.security(t2, "2D", close)
plot(c1)
plot(c2)
Devoluciones
Valor de la cadena del id del ticker que se puede proporcionar a la función request.security.
Ver también
syminfo.tickerid
syminfo.ticker
syminfo.session
session.extended
session.regular
ticker.heikinashi
adjustment.none
adjustment.splits
adjustment.dividends
backadjustment.inherit
backadjustment.on
backadjustment.off
settlement_as_close.inherit
settlement_as_close.on
settlement_as_close.off
ticker.new()

Crea un identificador de ticker para solicitar datos adicionales para el script.
Sintaxis
ticker.new(prefix, ticker, session, adjustment, backadjustment, settlement_as_close) → simple string
Argumentos
prefix (simple string) Prefijo del mercado. Por ejemplo: 'BATS', 'NYSE', 'NASDAQ', el prefijo de mercado de las series principales es syminfo.prefix.
ticker (simple string) Nombre del ticker. Por ejemplo, 'AAPL', 'MSFT', 'EURUSD'. El nombre del ticker de la serie principal es syminfo.ticker.
session (simple string) Tipo de sesión. Argumento opcional. Valores posibles: session.regular, session.extended. El tipo de sesión del gráfico actual es syminfo.session. Si no se indica la sesión, se utiliza el valor syminfo.session.
adjustment (simple string) Tipo de ajuste. Argumento opcional. Valores posibles: adjustment.none, adjustment.splits, adjustment.dividends. Si no se determina el tipo de ajuste, se utiliza el valor establecido por defecto (puede que sea diferente, en función el instrumento que se utiliza).
backadjustment (simple backadjustment) Especifica si los datos de contratos pasados en símbolos de futuros continuos se ajustan retroactivamente. Este ajuste solo afecta a los datos de los símbolos con esta opción disponible en sus gráficos y es opcional. El valor por defecto es backadjustment.inherit, lo que significa que el nuevo ticker ID hereda la configuración por defecto del símbolo. Valores posibles: backadjustment.inherit, backadjustment.on, backadjustment.off.
settlement_as_close (simple settlement) Especifica si el valor close de un símbolo de futuros representa el precio de cierre real o el precio de liquidación en "1D" y en intervalos de tiempo superiores. Este ajuste solo afecta a los datos de los símbolos con esta opción disponible en sus gráficos y es opcional. El valor por defecto es settlement_as_close.inherit, lo que significa que el nuevo ticker ID hereda la configuración por defecto del símbolo del gráfico. Valores posibles: settlement_as_close.inherit, settlement_as_close.on, settlement_as_close.off.
Ejemplo

//@version=5
indicator("ticker.new", overlay=true) 
t = ticker.new(syminfo.prefix, syminfo.ticker, session.regular, adjustment.splits)
t2 = ticker.heikinashi(t)
c = request.security(t2, timeframe.period, low, barmerge.gaps_on)
plot(c, style=plot.style_linebr)
Devoluciones
Valor de la cadena del id del ticker que se puede proporcionar a la función request.security.
Observaciones
Puede utilizar el valor devuelto de la función ticker.new como argumento de entrada para las funciones ticker.heikinashi, ticker.renko, ticker.linebreak, ticker.kagi, ticker.pointfigure.
Ver también
syminfo.tickerid
syminfo.ticker
syminfo.session
session.extended
session.regular
ticker.heikinashi
adjustment.none
adjustment.splits
adjustment.dividends
backadjustment.inherit
backadjustment.on
backadjustment.off
settlement_as_close.inherit
settlement_as_close.on
settlement_as_close.off
ticker.pointfigure()

Crea un identificador de ticker para solicitar valores Point & Figure.
Sintaxis
ticker.pointfigure(symbol, source, style, param, reversal) → simple string
Argumentos
symbol (simple string) Identificador de símbolo ticker.
source (simple string) La fuente para calcular el punto y la figura. Los valores posibles son: 'hl', 'close'.
style (simple string) Método de asignación del tamaño del recuadro: 'ATR', 'Tradicional'
param (simple int/float) Longitud ATR si style es igual a 'ATR', o tamaño del recuadro si style es igual a 'Traditional'.
reversal (simple int) Importe de reversión.
Ejemplo

//@version=5
indicator("ticker.pointfigure", overlay=true) 
pnf_tickerid = ticker.pointfigure(syminfo.tickerid, "hl", "Traditional", 1, 3)
pnf_close = request.security(pnf_tickerid, timeframe.period, close)
plot(pnf_close)
Devoluciones
Valor de la cadena del id del ticker que se puede proporcionar a la función request.security.
Ver también
syminfo.tickerid
syminfo.ticker
request.security
ticker.heikinashi
ticker.renko
ticker.linebreak
ticker.kagi
ticker.renko()

Crea un identificador de ticker para solicitar los valores Renko.
Sintaxis
ticker.renko(symbol, style, param, request_wicks, source) → simple string
Argumentos
symbol (simple string) Identificador de símbolo ticker.
style (simple string) Método de asignación del tamaño del recuadro: 'ATR', 'Tradicional'
param (simple int/float) Longitud ATR si style es igual a 'ATR', o tamaño del recuadro si style es igual a 'Traditional'.
request_wicks (simple bool) Especifica si se devuelven los valores wick para las velas Renko. Cuando es true, high y low los valores solicitados de un símbolo usando el ticker formado por esta función incluirán valores wick cuando estén presentes. Cuando false, high y low sean siempre iguales a open o close. Opcional. Por defecto es false. Puede encontrar una explicación detallada de cómo se calculan las velas Renko en nuestro Centro de ayuda.
source (simple string) Fuente utilizada para calcular los bricks. Opcional. Valores posibles: "Close", "OHLC". El valor por defecto es "Close".
Ejemplo

//@version=5
indicator("ticker.renko", overlay=true) 
renko_tickerid = ticker.renko(syminfo.tickerid, "ATR", 10)
renko_close = request.security(renko_tickerid, timeframe.period, close)
plot(renko_close)
Ejemplo

//@version=5
indicator("Renko candles", overlay=false)
renko_tickerid = ticker.renko(syminfo.tickerid, "ATR", 10)
[renko_open, renko_high, renko_low, renko_close] = request.security(renko_tickerid, timeframe.period, [open, high, low, close])
plotcandle(renko_open, renko_high, renko_low, renko_close, color = renko_close > renko_open ? color.green : color.red)
Devoluciones
Valor de la cadena del id del ticker que se puede proporcionar a la función request.security.
Ver también
syminfo.tickerid
syminfo.ticker
request.security
ticker.heikinashi
ticker.linebreak
ticker.kagi
ticker.pointfigure
ticker.standard()

Crea un ticker para solicitar datos de un gráfico típico que no se ve afectado por modificadores como la sesión con horario ampliado, el ajuste de dividendos, la conversión de divisas, así como los cálculos de tipos de gráficos atípicos: Heikin Ashi, Renko, etc. Entre otras cosas, esto permite recuperar valores de gráficos típicos cuando el script se está ejecutando en un gráfico atípico.
Sintaxis
ticker.standard(symbol) → simple string
Argumentos
symbol (simple string) ID de ticker que debe convertirse en su formato estándar. Opcional. Por defecto es syminfo.tickerid.
Ejemplo

//@version=5
indicator("ticker.standard", overlay = true)
// This script should be run on a non-standard chart such as HA, Renko...

// Requests data from the chart type the script is running on.
chartTypeValue = request.security(syminfo.tickerid, "1D", close)

// Request data from the standard chart type, regardless of the chart type the script is running on.
standardChartValue = request.security(ticker.standard(syminfo.tickerid), "1D", close)

// This will not use a standard ticker ID because the `symbol` argument contains only the ticker — not the prefix (exchange).
standardChartValue2 = request.security(ticker.standard(syminfo.ticker), "1D", close)

plot(chartTypeValue)
plot(standardChartValue, color = color.green)
Devoluciones
Cadena que representa el ticker de un gráfico estándar en el formato "prefix:ticker". Si el argumento symbol no contiene la información de prefijo y ticker, la función devuelve el argumento proporcionado.
Ver también
request.security
time()3 sobrecargas

La función time devuelve el tiempo UNIX de la barra actual para un intervalo de tiempo y una sesión determinados o NaN si el punto del tiempo está fuera de la sesión.
Sintaxis y sobrecargas
time(timeframe, bars_back) → series int
time(timeframe, session, bars_back) → series int
time(timeframe, session, timezone, bars_back) → series int
Argumentos
timeframe (series string) Timeframe (intervalo de tiempo). Una cadena vacía se interpreta como el intervalo de tiempo actual del gráfico.
bars_back (series int) Si se especifica, la función devuelve la timestamp de la barra N barras históricas en relación con la barra actual en el intervalo de tiempo especificado. Pasando un número negativo de -1 a -500, permite a la función solicitar la hora prevista de una barra futura. Opcional. El valor por defecto es 0.
Ejemplo

//@version=5
indicator("Time", overlay=true)
// Try this on chart AAPL,1
timeinrange(res, sess) => not na(time(res, sess, "America/New_York")) ? 1 : 0
plot(timeinrange("1", "1300-1400"), color=color.red)

// This plots 1.0 at every start of 10 minute bar on a 1 minute chart:
newbar(res) => ta.change(time(res)) == 0 ? 0 : 1
plot(newbar("10"))
A la hora de configurar una sesión, puede especificar tanto las horas y los minutos, como los días de la semana que se incluirán en esa sesión.
Si no se especifican los días, se considera que la sesión se ha fijado de domingo (1) a sábado (7), es decir, "1100-2000" es lo mismo que "1100-1200:1234567".
Esto lo puede modificar especificando los días. Por ejemplo, se puede comprobar en el siguiente script que no se colorean los sábados y domingos de un símbolo negociado durante los siete días de la semana y una sesión de 24 horas:
Ejemplo

//@version=5
indicator("Time", overlay=true)
t1 = time(timeframe.period, "0000-0000:23456")
bgcolor(not na(t1) ? color.new(color.blue, 90) : na)
Un argumento session puede incluir varias sesiones diferentes, separadas por comas. Por ejemplo, el siguiente script resaltará las barras de 10:00 a 11:00 y de 14:00 a 15:00 (solo días laborables):
Ejemplo

//@version=5
indicator("Time", overlay=true)
t1 = time(timeframe.period, "1000-1100,1400-1500:23456")
bgcolor(not na(t1) ? color.new(color.blue, 90) : na)
Devoluciones
Tiempo UNIX.
Observaciones
Tiempo UNIX es el número de milisegundos que han transcurrido desde las 00:00:00 UTC, del 1 de enero de 1970.
Ver también
time
time_close()3 sobrecargas

Devuelve la hora en UNIX del cierre de la barra actual para el intervalo de tiempo y la sesión especificados, o na si el punto de tiempo está fuera de la sesión. En los gráficos de ticks y en los basados en precios, como Renko, rotura de línea, Kagi, punto y figura, y rango, esta función devuelve una marca de tiempo na para la última barra en tiempo real (ya que la hora de cierre futura es impredecible), aunque la marca de tiempo de cualquier barra anterior es válida.
Sintaxis y sobrecargas
time_close(timeframe, bars_back) → series int
time_close(timeframe, session, bars_back) → series int
time_close(timeframe, session, timezone, bars_back) → series int
Argumentos
timeframe (series string) Resolución. Una cadena vacía se interpreta como la resolución actual del gráfico.
bars_back (series int) Si se especifica, la función devuelve la timestamp de la barra N barras históricas en relación con la barra actual en el intervalo de tiempo especificado. Pasando un número negativo de -1 a -500, permite a la función solicitar la hora prevista de una barra futura. Opcional. El valor por defecto es 0.
Ejemplo

//@version=5
indicator("Time", overlay=true)
t1 = time_close(timeframe.period, "1200-1300", "America/New_York")
bgcolor(not na(t1) ? color.new(color.blue, 90) : na)
Devoluciones
Tiempo UNIX.
Observaciones
Tiempo UNIX es el número de milisegundos que han transcurrido desde las 00:00:00 UTC, del 1 de enero de 1970.
Ver también
time_close
timeframe.change()

Detecta los cambios en el timeframe especificado.
Sintaxis
timeframe.change(timeframe) → series bool
Argumentos
timeframe (series string) Cadena formateada según las Especificaciones de la cadena de intervalos de tiempo (timeframe) del manual de usuario.
Ejemplo

//@version=5
// Run this script on an intraday chart.
indicator("New day started", overlay = true)
// Highlights the first bar of the new day.
isNewDay = timeframe.change("1D")
bgcolor(isNewDay ? color.new(color.green, 80) : na)
Devoluciones
Devuelve true en la primera barra de un nuevo timeframe, false en caso contrario.
timeframe.from_seconds()2 sobrecargas

Convierte una cantidad de segundos en una cadena de intervalo de tiempo válida.
Sintaxis y sobrecargas
timeframe.from_seconds(seconds) → simple string
timeframe.from_seconds(seconds) → series string
Argumentos
seconds (simple int) El número de segundos en el intervalo de tiempo.
Ejemplo

//@version=5
indicator("HTF Close", "", true)
int chartTf = timeframe.in_seconds()
string tfTimes5 = timeframe.from_seconds(chartTf * 5)
float htfClose = request.security(syminfo.tickerid, tfTimes5, close)
plot(htfClose)
Devoluciones
Una cadena de intervalo de tiempo conforme a timeframe string specifications.
Observaciones
Si no existe un intervalo de tiempo válido para la cantidad de segundos proporcionada, se devolverá la franja horaria válida inmediatamente superior. Por consiguiente, un segundo o menos devolverá "1s", de 2 a 5 segundos devolverá "5s", y 604.799 segundos (un segundo menos que 7 días) devolverá "7D".
Si los segundos representan exactamente dos o más intervalos de tiempo válidos, se utilizará el que tenga la mayor unidad base. De este modo, 604.800 segundos (7 días) devuelven "1S", no "7D".
Todos los valores superiores a 31.622.400 (366 días) devuelven "12M".
Ver también
timeframe.in_seconds
request.security
request.security_lower_tf
timeframe.in_seconds()2 sobrecargas

Convierte una cadena de intervalo de tiempo en segundos.
Sintaxis y sobrecargas
timeframe.in_seconds(timeframe) → simple int
timeframe.in_seconds(timeframe) → series int
Argumentos
timeframe (simple string) Cadena de intervalo de tiempo en formato timeframe string specifications. Opcional. Por defecto timeframe.period.
Ejemplo

//@version=5
indicator("`timeframe_in_seconds()`")

// Get a user-selected timeframe.
tfInput = input.timeframe("1D")

// Convert it into an "int" number of seconds.
secondsInTf = timeframe.in_seconds(tfInput)

plot(secondsInTf)
Devoluciones
La representación "int" del número de segundos en la cadena timeframe.
Observaciones
Cuando el intervalo de tiempo es "1M" o más, para el cálculo se utilizan 2628003 segundos en un mes, lo que representa 30,4167 (365/12) días.
Ver también
input.timeframe
timeframe.period
timeframe.from_seconds
timestamp()5 sobrecargas

La función timestamp de registro horario devuelve el tiempo UNIX de la fecha y hora indicadas.
Sintaxis y sobrecargas
timestamp(dateString) → const int
timestamp(year, month, day, hour, minute, second) → simple int
timestamp(year, month, day, hour, minute, second) → series int
timestamp(timezone, year, month, day, hour, minute, second) → simple int
timestamp(timezone, year, month, day, hour, minute, second) → series int
Argumentos
dateString (const string) Cadena que contiene la fecha y, de forma opcional, la hora y la zona horaria. El formato debe cumplir el estándar IETF RFC 2822 o ISO 8601 ("DD MM YYYY hh:mm:ss ±hhmm" or "YYYY-MM-DDThh:mm:ss±hh:mm", por lo tanto, "20 febrero 2020" o "2020-02-20"). En el caso de que no se indique ninguna hora, se utiliza "00:00". Si no se proporciona una zona horaria, se utiliza GMT+0. Tenga en cuenta que esto difiere respecto al comportamiento habitual de la función, que devuelve la hora en la zona horaria del mercado de valores.
Ejemplo

//@version=5
indicator("timestamp")
plot(timestamp(2016, 01, 19, 09, 30), linewidth=3, color=color.green)
plot(timestamp(syminfo.timezone, 2016, 01, 19, 09, 30), color=color.blue)
plot(timestamp(2016, 01, 19, 09, 30), color=color.yellow)
plot(timestamp("GMT+6", 2016, 01, 19, 09, 30))
plot(timestamp(2019, 06, 19, 09, 30, 15), color=color.lime)
plot(timestamp("GMT+3", 2019, 06, 19, 09, 30, 15), color=color.fuchsia)
plot(timestamp("Feb 01 2020 22:10:05"))
plot(timestamp("2011-10-10T14:48:00"))
plot(timestamp("04 Dec 1995 00:12:00 GMT+5"))
Devoluciones
Tiempo UNIX.
Observaciones
Tiempo UNIX es el número de milisegundos que han transcurrido desde las 00:00:00 UTC, del 1 de enero de 1970.
Ver también
time
weekofyear()2 sobrecargas

Sintaxis y sobrecargas
weekofyear(time) → series int
weekofyear(time, timezone) → series int
Argumentos
time (series int) Tiempo UNIX en milisegundos.
Devoluciones
Semana del año (en zona horaria de mercado) para el tiempo UNIX proporcionado.
Observaciones
Tiempo UNIX es el número de milisegundos que han transcurrido desde las 00:00:00 UTC, del 1 de enero de 1970.
Observe que esta función devuelve la semana respecto a la fecha del comienzo de la barra. Para las sesiones overnight (por ejemplo, EURUSD, donde la sesión del lunes empieza el domingo, a las 17:00) el valor puede ser 1 punto inferior respecto a la semana del día del trading.
Ver también
weekofyear
time
year
month
dayofmonth
dayofweek
hour
minute
second
year()2 sobrecargas

Sintaxis y sobrecargas
year(time) → series int
year(time, timezone) → series int
Argumentos
time (series int) Tiempo UNIX en milisegundos.
Devoluciones
Año (en zona horaria del mercado de valores) para el tiempo UNIX proporcionado.
Observaciones
Tiempo UNIX es el número de milisegundos que han transcurrido desde las 00:00:00 UTC, del 1 de enero de 1970.
Observe que esta función devuelve el día en función de la fecha del comienzo de la barra. Para las sesiones overnight (por ejemplo, EURUSD, en la que la sesión del lunes empieza el domingo, a las 17:00 UTC-4) este valor puede ser 1 punto inferior respecto al año desde el día del trading.
Ver también
year
time
month
dayofmonth
dayofweek
hour
minute
second
