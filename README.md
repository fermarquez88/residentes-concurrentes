### Relevamiento de condiciones laborales de residentes y concurrentes en Argentin

#### Objetivos:
1. Determinar las condiciones laborales de los Residentes y Concurrentes de toda Argentina para realizar un estudio descriptivo cuali y cuantitativo.
2. Establecer fundamentos con rigor científico para comunicar a la población general la situación de residentes y concurrentes.

#### Diseño y población: 

Estudio observacional de cohorte retrospectiva de ....

Criterios de inclusión: 
- Titulo de grado médicos
-

Criterios de exclusión:
-Residentes o concurrentes que no fuesen médicos

#### Dataset: 

Los datos fueron extraídos de una encuesta de relevanvamiento sobre las condiciones laborales de los Residentes y concurrentes.

#### Descripción de la encuesta: 
En el mes de Diciembre de 2019 realizamos una encuesta online, voluntaria, publica y anónima para conocer la situación de los profesionales de la salud que formaban parte de un programa de Residencias o Concurrencias en Argentina.

Variables extraídas de los datasets utilizados se encuentra en la lista 1 del anexo

Variables socio-demográficas:
Edad: Mayores de 18 años
Género: Masculino o femenino

Variables clínicas:

Materiales:

PHQ: 
Cuestionario de calidad de vida OMS:

#### Definición de grupos:

Se definió operativamente el riesgo de depresión de acuerdo a la escala PHQ-2, dicotomizando el resultado (etiquetas) en SI (1) o NO (0) y se definieron dos grupos para realizar unas clasificación binaria:
Grupo 0: puntuación < a 3 en PHQ-2.
Grupo 1: puntuación > o = a 3 en PHQ-2.

#### Procesamiento de datos y análisis estadístico: 

Se utilizaron las librerías de --------- del lenguaje de programación R. El código de programación está disponible para su reproducibilidad en: https://github.com/fermarquez2019/residentes-concurrentes

#### Variables categóricas: 
Descritas con frecuencias absolutas y porcentuales. 
Comparación de grupos: para evaluar la independencia se realizó test de Chi-Cuadrado y Kruskal Wallis.

#### Variables numéricas: 
Se valoró la distribución mediante el Test de Shapiro-Wilk. 
Variables con distribución normal: se describieron mediante la media y el desvío estándar.
Variables sin distribución normal: se describieron con la mediana, cuartil inferior (C1), superior (C3), límite inferior (LI) y límite superior (LS).
Comparación de grupos: para evaluar la independencia se realizó test de Mann-Whitney.

Ingeniería de variables

La mayoría de los algoritmos de aprendizaje automático no pueden manejar variables categóricas a menos que sean convertidas en valores numéricos por lo cual las variables nominales (edad, género, diagnóstico del estado de conciencia al ingreso y PCI) fueron preprocesadas con la función get_dummies. Esta permite crear tantas variables nuevas por cada categoría en la cual se codificó la variable original. Por ejemplo para ‘‘Etiología de la lesión’’, registrada como TEC, ACV, Anoxia y Tumor se agregan al dataset una columna TEC (recodificada a 0 y 1 de acuerdo a la presencia o no de este diagnóstico), al igual que para ACV, Anoxia y Tumor. La función permite también automáticamente eliminar una de las columnas generadas y evitar así la colinealidad y que los algoritmos no presenten un sobre ajuste final a los datos. 
Teniendo en cuenta que el número de vectores depende del número de categorías, este método produciría un gran número columnas al procesar las subescalas de la ERC y también relentizaría el aprendizaje significativamente dado que el número de la categorías sería muy alto para la función. Por este motivo para incluir las subescalas de la ERC de forma más eficiente y asegurando que se conserve la naturaleza de estas variables se realizó una codificación ordinal utilizando los valores que se le asignan habitualmente para calcular la ERC-T. 
En estudios previos Portaccio y colaboradores33 reportaron que una mejora en la puntuación total de ERC-R y en diferentes subescalas durante las primeras cuatro semanas de rehabilitación hospitalaria discrimina a los pacientes con mejor pronóstico. En base a esto y las observaciones de la práctica cotidiana se definieron dos nuevas variables para investigar su correlación con EMCS y eventualmente incluirlas en los modelos. 
Variación de la ERC-R: diferencia entre la mejor puntuación ERC-R total (y subescalas) entre el día 8 al 30 de seguimiento, respecto de la puntuación inicial.
Teniendo en cuenta que se seleccionó la mejor puntuación de la ERC-T y sus respectivas subescalas dentro las primeras 4 semanas del seguimiento, el registro no fue siempre exactamente en el mismo tiempo de seguimiento y por lo tanto se decidió desarrollar una nueva variable que ajuste la puntuación de acuerdo al momento en el cual fue realizada la evaluación: 
Tasa de cambio de ERC-R (Velocidad de variación de la ERC). Se la definió operativamente como el coeficiente que surge de la variación de la ERC dividido en la cantidad de días de seguimiento.

Selección de variables:

Se exploraron las diferencias de las variables (categóricas y numéricas) entre el Grupo EMCS y No EMCS y su independencia estadística (p < 0.05 en test Chi Cuadrado o Mann Mann-Whitney y Kruskall-Wallis)
Se establecieron dos puntos de corte para la selección inicial de variables: al ingreso y al mes seguimiento.
Ingreso (16 variables): edad, edad dicotomizada 1 (mayores y menores de 50 años), edad dicotomizada 2 (mayores y menores de 30 años),  género, etiología, tiempo de evolución desde la lesión al ingreso, diagnóstico del estado de conciencia al ingreso (SVSR-ECM), puntuación en ERC-Total y en cada una de las subescalas.
Al mes (75 variables): Todas las variables anteriores y además, la mejor ERC-Total durante el seguimiento con sus respectivas subescalas, sus respectivas mejorías (variación respecto del ingreso) y sus respectiva relaciones entre esta mejoría y el tiempo total de seguimiento en el cual fue observada (tasa de variación). También se incluyó la presencia al menos una de las combinaciones improbables en el total de evaluaciones y presencia específica de las combinaciones improbables (36 variables) que describe Chatelle, Giacino y cols.
Se redujo la cantidad de variables con un modelo de eliminación recursiva de variables con un Bosque aleatorio en las etapas que criterio médico fuese conveniente. Este método asigna un peso a cada una de las variables y las de pesos absolutos más pequeños se eliminan del conjunto. El procedimiento se repite de forma recursiva hasta obtener la variables más adecuadas34 para el modelo predictivo, pero no nos permite saber si la exhaustividad es mejor con 5 o 25 variables en total. Por este motivo realizamos una eliminación recursiva de variables con validación cruzada35 para obtener las mejores variables y el número más óptimo.
Se analizó el coeficiente de correlación entre variables para valorar las interacción con la recuperación de la conciencia y las interacciones entre todas las variables del siguiente modo:
Ordinales o numéricas:
Sin distribución normal: se utilizó coeficiente de correlación de Spearman
Con distribución normal: se utilizó coeficiente de correlación de Pearson
Nominales o binarias: Se utilizó el coeficiente de V de Cramer.
Se realizó la eliminación manual paso a paso de las variables con menor importancia de las que tuviesen una correlación casi perfecta (coeficiente de correlación >0.9), en un segundo paso con correlación fuerte (>0.7 y <0.9) y moderada (>0.5 y <0.7). 


##### Link a la encuesta: http://bit.ly/encuesta_anonima_residentes

##### Link al reporte preliminar de resultados: https://www.jotform.com/report/20202740287905021
