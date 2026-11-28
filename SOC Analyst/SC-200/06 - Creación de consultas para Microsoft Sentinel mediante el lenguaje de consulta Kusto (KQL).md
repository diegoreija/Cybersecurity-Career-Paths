# 🛡️ MANUAL DE CREACIÓN DE CONSULTAS PARA MICROSOFT SENTINEL MEDIANTE EL LENGUAJE DE CONSULTA KUSTO (KQL)

### *Guía de Referencia Operativa y Análisis de Logs — Basada Estrictamente en la Fuente Oficial*

---

---

&gt; **Nota Metodológica de Cobertura:** Este documento constituye la síntesis completa y detallada de la guía oficial sobre la creación de consultas para Microsoft Sentinel mediante el lenguaje de consulta Kusto (KQL). Toda la información contenida en este manual se deriva estrictamente de los contenidos y conceptos presentados en la fuente de origen, manteniendo la máxima fidelidad conceptual, procesal y terminológica sin incorporar suposiciones ni elementos externos.

---

## 🚀 Matriz de Consulta Rápida: Operadores, Sintaxis y Mecanismos de KQL

| Operador / Instrucción | Mecanismo y Sintaxis Clave                        | Función Operativa                                                          | Resultado u Objetivo Principal                                                        |
| ---------------------- | ------------------------------------------------- | -------------------------------------------------------------------------- | ------------------------------------------------------------------------------------- |
| **Canalización ()**    | Encadenamiento secuencial de izquierda a derecha. | Conecta la salida de un operador como entrada directa del siguiente.       | Permite el flujo continuo de transformación desde el origen de datos.                 |
| \*\*\*\*               | o                                                 | Búsqueda global de cadenas en múltiples tablas y columnas simultáneamente. | Localiza información cuando se desconoce la ubicación o tabla exacta.                 |
| \*\*\*\*               | o                                                 | Filtrado de filas mediante predicados lógicos y temporales.                | Selecciona exclusivamente el subconjunto de filas que cumplen las condiciones.        |
| \*\*\*\*               | o                                                 | Vinculación de nombres a expresiones o conjuntos dinámicos.                | Facilita la modularidad, descomposición y reutilización de código complejo.           |
| \*\*\*\*               |                                                   | Creación y anexo de columnas calculadas a partir de funciones.             | Enriquece el conjunto de datos con nuevos atributos derivados en tiempo de ejecución. |
| \*\*\*\*               |                                                   | Clasificación de filas según una o varias columnas especificadas.          | Organiza la salida en orden ascendente o descendente (por defecto descendente).       |
| \*\*\*\*               |                                                   | Selección explícita, renombrado e inserción de columnas.                   | Optimiza el rendimiento reduciendo el tamaño del conjunto de datos devuelto.          |

---

## 1\. Estructura y Sintaxis Fundamental del Lenguaje de Consulta Kusto (KQL)

El lenguaje de consulta Kusto (KQL) constituye el motor fundamental para el procesamiento y análisis de datos dentro de Microsoft Sentinel. Una consulta de KQL se define operativamente como una solicitud de solo lectura orientada a procesar datos ingestados y devolver resultados estructurados. La sintaxis se expresa en texto sin formato mediante un modelo de flujo de datos intuitivo diseñado para simplificar la lectura, escritura y automatización de análisis forenses. Para organizar y estructurar la información almacenada, KQL adopta entidades de esquema conceptualmente equivalentes a las de SQL, organizadas jerárquicamente en bases de datos, tablas y columnas. La estructura formal de cualquier consulta se compone de una secuencia de instrucciones donde al menos una de ellas debe ser una instrucción de expresión tabular, la cual se encarga de generar la malla final de filas y columnas que constituye el resultado visible para el analista.

La mecánica de ejecución de KQL se fundamenta en la transmisión secuencial de información a través del delimitador de canalización o pipe (). El flujo de datos inicia formalmente en el origen de datos primario, representado por la tabla invocada, y avanza de forma continua conectando operadores de consulta tabulares. La regla sintáctica central establece que todo el contenido o conjunto de datos situado a la izquierda de la barra vertical se procesa e introduce como entrada directa para el operador posicionado a su derecha. Un flujo analítico típico puede iniciar invocando una tabla de eventos de seguridad como , aplicar un filtro de filas basado en los valores de una columna específica, resumir la información resultante contando el número de registros agrupados por cuenta de usuario y finalizar limitando la cantidad de filas proyectadas para optimizar la visualización final.

---

## 2\. Operadores Fundamentales para la Selección y Filtrado de Datos

Dentro del ecosistema de KQL, la selección y delimitación de registros se realiza mediante operadores especializados orientados a la búsqueda global o al filtrado preciso. El operador proporciona la capacidad de realizar búsquedas textuales de forma simultánea a través de múltiples tablas y columnas dentro del entorno. Esta funcionalidad resulta de gran utilidad operativa cuando el analista no tiene certeza previa sobre la tabla o la columna concreta donde se encuentra alojada la información. Por ejemplo, la consulta escanea dicha cadena de texto en la totalidad de las tablas de la base de datos, mientras que la variante restringe el escaneo a tablas específicas como y junto con aquellas cuyo nombre comience con la letra . No obstante, el operador es considerablemente menos eficiente en términos de procesamiento computacional si se compara con operadores de filtrado directo.

Para lograr una delimitación rigurosa de los datos con un rendimiento óptimo, se utiliza el operador , el cual evalúa predicados lógicos sobre cada fila para seleccionar únicamente aquellas que cumplen con la condición especificada. El operador admite múltiples modalidades de filtrado, destacando la restricción temporal mediante funciones como para evaluar eventos del último día, la combinación de múltiples condiciones mediante el operador lógico como en , y la comparación insensible a mayúsculas y minúsculas mediante el operador . Asimismo, el operador facilita el filtrado sobre listas o conjuntos cerrados de valores a través del operador de inclusión , ejemplificado en la instrucción .

---

## 3\. Modularidad, Declaración de Variables y Transformación de Columnas

La legibilidad y mantenibilidad de las consultas complejas se gestiona mediante la instrucción , la cual permite vincular un nombre descriptivo a una expresión determinada. Al asignar una variable mediante , el resto de la consulta puede hacer referencia a dicha expresión utilizando el nombre asignado, favoreciendo la modularidad del código y permitiendo descomponer algoritmos complejos en componentes individuales más simples. La instrucción se utiliza habitualmente para definir variables escalares como o para construir listas y tablas dinámicas que posteriormente se incorporan en cláusulas de filtrado, como en el caso de declarar una lista de cuentas sospechosas vinculada mediante para luego ejecutar la condición .

En cuanto a la manipulación y enriquecimiento del esquema de salida, el operador cumple la función de calcular y anexar nuevas columnas al conjunto de datos existente. Este operador evalúa expresiones sobre las columnas originales y agrega el resultado como una columna adicional sin eliminar la estructura previa. Un caso de aplicación práctica consiste en la extracción del directorio de inicio de un ejecutable a partir de la ruta completa de un proceso, donde se genera una nueva columna denominada aplicando funciones de manipulación de cadenas como y sobre los campos y .

---

## 4\. Ordenación y Control de Proyección de Resultados

La presentación ordenada de la información procesada se controla mediante el operador , el cual clasifica las filas del conjunto de resultados en función de los valores contenidos en una o más columnas. Por defecto, la ordenación aplicada por el motor de consulta se ejecuta en sentido descendente, aunque el analista puede especificar de forma explícita la orientación de cada columna dentro de la misma instrucción. Un ejemplo de clasificación detallada se observa en la sentencia , la cual ordena en primer lugar los registros por la columna de forma descendente y, para los valores coincidentes, ordena secundariamente por la columna en sentido ascendente.

Finalmente, la gestión sobre qué columnas deben ser expuestas en la salida definitiva y la optimización del volumen de datos transferidos se administra a través del conjunto de operadores . En su variante estándar, el operador permite seleccionar de manera explícita la lista exacta de columnas que se incluirán en la tabla final, ofreciendo adicionalmente la capacidad de renombrar campos existentes o insertar nuevas columnas calculadas en la misma instrucción. Al restringir las columnas devueltas a las estrictamente necesarias para el análisis, el operador reduce el tamaño del conjunto de datos resultante, optimizando sustancialmente la velocidad de respuesta de la consulta y el rendimiento general de Microsoft Sentinel.
