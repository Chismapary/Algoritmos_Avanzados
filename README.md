# Algoritmos Avanzados - Coloración de Grafos con GNN

## 📋 Descripción General

Sistema completo para resolver el problema de coloración de grafos usando Graph Neural Networks (GNN) y heurísticas clásicas. El proyecto incluye desde la carga de datos hasta la evaluación comprehensiva con benchmarks simples y complejos (SNAP, DIMACS).

## 🎯 Objetivo del Proyecto

Desarrollar y evaluar un modelo GNN que aprenda a ordenar vértices de manera óptima para el algoritmo greedy de coloración de grafos, comparándolo con heurísticas clásicas como Welsh-Powell, DSATUR y Largest Degree First.

## 📂 Estructura del Proyecto

```
├── librerias.ipynb              # Importaciones y configuración inicial
├── data_loader.ipynb            # Carga y preprocesamiento de grafos
├── heuristics.ipynb             # Heurísticas clásicas de coloración
├── gnn_model.ipynb              # Arquitectura del modelo GNN
├── training.ipynb               # Entrenamiento del modelo
├── evaluation.ipynb             # Evaluación básica
├── benchmark_loader.ipynb       # Carga de benchmarks (SNAP/DIMACS)
├── evaluation_comprehensive.ipynb  # Evaluación completa multi-dataset
├── ejemplo_evaluacion_rapida.ipynb # Tutorial rápido
└── README.md                    # Esta guía
```

## 🚀 Flujo de Trabajo Completo

### 1️⃣ **Configuración Inicial**
```python
# Ejecutar primero
%run librerias.ipynb
```
Carga todas las dependencias: NetworkX, PyTorch, PyTorch Geometric, NumPy, Pandas, etc.

### 2️⃣ **Carga de Datos**
```python
%run data_loader.ipynb
```
**Pipeline de procesamiento:**
- Carga grafo crudo (simulado o real)
- Normalización (elimina lazos, duplicados)
- Reindexación de vértices (0 a n-1)
- Extracción de features (grado, clustering, k-core)
- Normalización de features
- Conversión a formato PyTorch Geometric

**Output:** Objeto `data` con grafo listo para GNN

### 3️⃣ **Heurísticas Clásicas**
```python
%run heuristics.ipynb
```
**Implementaciones incluidas:**
- **Random**: Baseline con orden aleatorio
- **Greedy Natural**: Orden natural de nodos
- **Largest Degree First (LDF)**: Orden por grado descendente
- **Welsh-Powell**: Variante mejorada de LDF
- **DSATUR**: Saturación dinámica (estado del arte)
- **Parallel Greedy**: Simulación de paralelismo

**Experimentos:** Ejecuta múltiples repeticiones y calcula estadísticas

### 4️⃣ **Modelo GNN**
```python
%run gnn_model.ipynb
```
**Arquitectura:**
- 2 capas GCNConv (Graph Convolutional Network)
- Función de activación ReLU
- Dropout para regularización
- Output: Score por nodo para ordenamiento

**Input:** Features de nodos + estructura del grafo  
**Output:** Ordenamiento óptimo para greedy coloring

### 5️⃣ **Entrenamiento**
```python
%run training.ipynb
```
**Proceso:**
- Generación de grafos de entrenamiento
- Función de pérdida: Penaliza ordenamientos que resultan en más colores
- Optimizador Adam
- Entrenamiento por épocas con validación

**Output:** Modelo entrenado listo para evaluación

### 6️⃣ **Evaluación Básica**
```python
%run evaluation.ipynb
```
Compara el modelo GNN contra greedy baseline en el grafo de entrenamiento.

### 7️⃣ **Evaluación Comprehensiva** ⭐
```python
%run evaluation_comprehensive.ipynb
```
Sistema completo de evaluación con múltiples benchmarks y análisis estadístico.

## 🎯 Componentes Principales

### 1. **benchmark_loader.ipynb**
Carga y gestión de datasets de prueba.

**Características:**
- Grafos simples sintéticos (Petersen, ciclos, ruedas, completos, etc.)
- Grafos complejos (scale-free, small-world, random regular, etc.)
- Parser para formato DIMACS
- Parser para formato SNAP
- Suites predefinidas de benchmarks

**Uso básico:**
```python
# Cargar suite simple
benchmarks = cargar_benchmark('suite', nivel='simple')

# Cargar suite compleja
benchmarks = cargar_benchmark('suite', nivel='complejo')

# Cargar archivo DIMACS
benchmarks = cargar_benchmark('dimacs', filepath='path/to/file.col')

# Cargar archivo SNAP
benchmarks = cargar_benchmark('snap', filepath='path/to/edges.txt')

# Generar grafo específico
benchmarks = cargar_benchmark('complejo', tipo='scale_free', n=200, m=5)
```

### 2. **evaluation_comprehensive.ipynb**
Sistema completo de evaluación con múltiples heurísticas.

**Heurísticas evaluadas:**
1. **Random** - Baseline con orden aleatorio
2. **Greedy Natural** - Orden natural de nodos
3. **Largest Degree First** - Orden por grado descendente
4. **Welsh-Powell** - Variante mejorada de LDF
5. **DSATUR** - Saturación dinámica (estado del arte)
6. **GNN-guided** - Guiado por red neuronal (cuando disponible)

**Métricas calculadas:**
- Número de colores (promedio, std, min, max)
- Tiempo de ejecución
- Validez de la coloración
- Estadísticas del grafo (nodos, aristas, densidad, etc.)

## 🎓 Inicio Rápido

### Opción A: Flujo Completo (Desde Cero)
```python
# 1. Configuración
%run librerias.ipynb

# 2. Cargar datos
%run data_loader.ipynb

# 3. Ver heurísticas clásicas
%run heuristics.ipynb

# 4. Definir modelo GNN
%run gnn_model.ipynb

# 5. Entrenar modelo
%run training.ipynb

# 6. Evaluar
%run evaluation.ipynb
```

### Opción B: Solo Evaluación de Benchmarks
```python
# Ejecutar directamente
%run ejemplo_evaluacion_rapida.ipynb
```

## 🧪 Sistema de Evaluación con Benchmarks

### Nivel 1: Evaluación Simple (Comenzar aquí)
```python
# En evaluation_comprehensive.ipynb, ejecutar sección 8
benchmarks_simple = cargar_benchmark('suite', nivel='simple')
df_resultados = evaluar_suite_completa(benchmarks_simple, model=None, repeticiones=3)
```

**Benchmarks incluidos:**
- Grafo de Petersen (10 nodos)
- Ciclo de 20 nodos
- Rueda de 15 nodos
- Completo K10
- Bipartito K10,10
- Árbol de 30 nodos
- Random 30 nodos

**Tiempo estimado:** 1-2 minutos

### Nivel 2: Evaluación Media
```python
# Ejecutar sección 9
benchmarks_medio = cargar_benchmark('suite', nivel='medio')
df_resultados_medio = evaluar_suite_completa(benchmarks_medio, model=None, repeticiones=3)
```

**Benchmarks incluidos:**
- Scale-free (100 nodos)
- Small-world (100 nodos)
- Random regular (100 nodos)
- Geometric (100 nodos)
- Powerlaw cluster (100 nodos)
- Random Erdős-Rényi (100 nodos)

**Tiempo estimado:** 5-10 minutos

### Nivel 3: Evaluación Compleja
```python
# Ejecutar sección 10
benchmarks_complejo = cargar_benchmark('suite', nivel='complejo')
df_resultados_complejo = evaluar_suite_completa(benchmarks_complejo, model=None, repeticiones=3)
```

**Benchmarks incluidos:**
- Scale-free (500 nodos)
- Small-world (500 nodos)
- Random regular (500 nodos)
- Random Erdős-Rényi (500 nodos)
- Powerlaw cluster (500 nodos)

**Tiempo estimado:** 20-30 minutos

## 📊 Interpretación de Resultados

### Análisis Estadístico
El sistema genera automáticamente:

1. **Resumen por método:**
   - Media y desviación estándar de colores
   - Mínimo y máximo de colores
   - Tiempo promedio de ejecución

2. **Ranking de métodos:**
   - Ordenados por número promedio de colores
   - Identifica el mejor método general

3. **Comparación por grafo:**
   - Resultados detallados para cada benchmark
   - Comparación con cotas teóricas (cuando disponibles)
   - Mejor método para cada instancia

### Visualizaciones
Gráficos generados automáticamente:

1. **Comparación de métodos** - Barras horizontales con error bars
2. **Tiempo de ejecución** - Comparación de eficiencia
3. **Escalabilidad** - Nodos vs colores usados
4. **Densidad vs colores** - Comportamiento según estructura

## 🔬 Metodología Científica

### Para un paper o reporte formal:

1. **Ejecutar nivel simple** - Validar implementación
2. **Ejecutar nivel medio** - Obtener resultados principales
3. **Ejecutar nivel complejo** - Demostrar escalabilidad
4. **Agregar DIMACS/SNAP** - Comparar con literatura

### Estructura de reporte sugerida:

```
1. Introducción
   - Problema de coloración de grafos
   - Heurísticas clásicas vs GNN

2. Metodología
   - Benchmarks utilizados (simple/medio/complejo)
   - Heurísticas evaluadas
   - Métricas de evaluación

3. Resultados Experimentales
   - Tabla resumen por nivel
   - Gráficos comparativos
   - Análisis estadístico

4. Discusión
   - Mejor método por tipo de grafo
   - Trade-offs tiempo vs calidad
   - Escalabilidad

5. Conclusiones
```

## 📁 Archivos Generados

Después de ejecutar la evaluación:

- `resultados_simple.csv` - Resultados nivel simple
- `resultados_medio.csv` - Resultados nivel medio
- `resultados_complejo.csv` - Resultados nivel complejo
- `resultados_evaluacion.png` - Visualizaciones

## 🔧 Personalización

### Agregar tu propio benchmark:

```python
# Opción 1: Desde archivo DIMACS
mi_grafo = cargar_benchmark('dimacs', filepath='mi_grafo.col', nombre='MiGrafo')

# Opción 2: Desde archivo SNAP
mi_grafo = cargar_benchmark('snap', filepath='mi_red.txt', nombre='MiRed')

# Opción 3: Generar sintético
mi_grafo = cargar_benchmark('complejo', tipo='scale_free', n=300, m=4)

# Evaluar
resultados = evaluar_todas_heuristicas(mi_grafo[0][0], model=None, repeticiones=5)
```

### Modificar número de repeticiones:

```python
# Más repeticiones = resultados más robustos pero más tiempo
df_resultados = evaluar_suite_completa(benchmarks, model=None, repeticiones=10)
```

### Evaluar con modelo GNN:

```python
# Cargar modelo entrenado
%run training.ipynb

# Evaluar con GNN
df_resultados = evaluar_suite_completa(benchmarks, model=model, repeticiones=5)
```

## 📚 Datasets Externos Recomendados

### DIMACS Challenge Benchmarks
Disponibles en: https://mat.tepper.cmu.edu/COLOR/instances.html

Ejemplos clásicos:
- `queen5_5.col` - Grafo de reinas 5x5
- `myciel3.col` - Grafo de Mycielski
- `anna.col` - Registro de Anna Karenina
- `david.col` - Grafo de David
- `games120.col` - Juegos de 120 equipos

### SNAP Datasets
Disponibles en: http://snap.stanford.edu/data/

Redes reales:
- `facebook_combined.txt` - Red social Facebook
- `email-Enron.txt` - Red de emails Enron
- `ca-GrQc.txt` - Colaboraciones en física
- `wiki-Vote.txt` - Red de votaciones Wikipedia

### Cómo usar:

1. Descargar el archivo
2. Colocarlo en una carpeta `datasets/`
3. Cargar con:
```python
grafo_dimacs = cargar_benchmark('dimacs', filepath='datasets/queen5_5.col')
grafo_snap = cargar_benchmark('snap', filepath='datasets/facebook_combined.txt')
```

## ⚡ Tips de Rendimiento

1. **Empezar con nivel simple** - Validar que todo funciona
2. **Usar menos repeticiones** en grafos grandes (repeticiones=1 o 2)
3. **DSATUR es lento** en grafos grandes (>1000 nodos)
4. **Guardar resultados** frecuentemente con `.to_csv()`
5. **Ejecutar por partes** - No necesitas correr todo de una vez

## 🎓 Para Tesis o Proyecto

### Checklist de evaluación completa:

- [ ] Ejecutar suite simple (validación)
- [ ] Ejecutar suite medio (resultados principales)
- [ ] Ejecutar suite complejo (escalabilidad)
- [ ] Agregar 3-5 benchmarks DIMACS
- [ ] Agregar 2-3 redes reales SNAP
- [ ] Generar todas las visualizaciones
- [ ] Calcular estadísticas significativas
- [ ] Documentar configuración experimental
- [ ] Guardar todos los CSV de resultados
- [ ] Incluir gráficos en el documento

## 📞 Próximos Pasos

### Para Comenzar:
1. **Ejecuta `ejemplo_evaluacion_rapida.ipynb`** - Tutorial interactivo
2. **Revisa el flujo completo** ejecutando notebooks 1-6 en orden
3. **Ejecuta `evaluation_comprehensive.ipynb`** para evaluación completa

### Para Profundizar:
4. **Descarga benchmarks DIMACS/SNAP** y agrégalos a la evaluación
5. **Ajusta hiperparámetros** del modelo GNN
6. **Genera visualizaciones** para tu reporte/tesis
7. **Compara con literatura** usando benchmarks estándar

## 🔗 Referencias

- **DIMACS Benchmarks**: https://mat.tepper.cmu.edu/COLOR/instances.html
- **SNAP Datasets**: http://snap.stanford.edu/data/
- **PyTorch Geometric**: https://pytorch-geometric.readthedocs.io/

## 📝 Notas Importantes

- Los notebooks están diseñados para ejecutarse en orden secuencial
- Cada notebook carga sus dependencias con `%run`
- Los resultados se guardan automáticamente en CSV
- Las visualizaciones se exportan como PNG

¡Buena suerte con tu proyecto! 🚀
