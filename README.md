# 📶 Análisis de clientes de ConnectaTel

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/valercardenas10-rgb/connectatel-analysis/blob/main/notebooks/S7_Version-Estudiante-Project-ConnectaTel.ipynb)

## 🎯 Objetivo del proyecto

ConnectaTel es una empresa de telecomunicaciones con operaciones en México y Colombia. En este proyecto se analiza cómo usan sus clientes los servicios móviles (llamadas y mensajes) con tres objetivos:

- identificar patrones de uso;
- detectar comportamientos atípicos (outliers);
- segmentar a los clientes por edad y por nivel de uso.

Con estos resultados se proponen mejoras a la oferta de planes y estrategias de retención.

## 🗂️ Datasets

| Archivo | Filas | Contenido |
|---|---|---|
| `plans.csv` | 2 | Planes **Basico** y **Premium**: precio mensual, minutos, mensajes y GB incluidos, y costo por excedente. |
| `users_latam.csv` | 4,000 | Clientes: `user_id`, nombre, edad, ciudad, fecha de registro, plan y fecha de baja (`churn_date`). |
| `usage.csv` | 40,000 | Eventos de uso de enero a junio de 2024: tipo (`call` / `text`), fecha, duración de la llamada (minutos) y longitud del mensaje (caracteres). |

## 🔄 Etapas del análisis

1. **Cargar y explorar:** estructura, tipos de datos y vista previa de los 3 datasets.
2. **Problemas de calidad:** nulos, sentinels (`-999`, `"?"`) y fechas fuera de rango.
3. **Limpieza básica:** imputación de `age` con la mediana, `"?"` convertido a nulo, fechas futuras marcadas como nulas y verificación de que los nulos de `duration` y `length` son MAR.
4. **Resumen por usuario:** conteo de mensajes, llamadas y minutos por usuario, unido a la tabla de clientes (`user_profile`).
5. **Visualización y outliers:** histogramas por plan, boxplots y límites con el método IQR.
6. **Segmentación:** grupos por nivel de uso (Bajo, Medio, Alto) y por edad (Joven, Adulto, Adulto Mayor).
7. **Insight ejecutivo:** conclusiones y recomendaciones comerciales.
8. **Publicación:** notebook y README en GitHub.

## 🔍 Hallazgos principales

- **El consumo es bajo y homogéneo:** la mediana por usuario es de 5 mensajes, 4 llamadas y 19.78 minutos en 6 meses, muy por debajo de lo que incluyen los planes.
- **El plan no diferencia el uso:** los clientes Premium (35.12%) consumen casi lo mismo que los Basico (5.52 vs 5.53 mensajes y 4.52 vs 4.45 llamadas en promedio), aunque pagan más del doble.
- **La edad tampoco explica el consumo:** Jóvenes (19.00%), Adultos (50.45%) y Adultos Mayores (30.55%) tienen promedios casi idénticos.
- **Segmentos de uso:** Uso medio 73.58%, Bajo uso 19.48% y Alto uso 6.95%. El 66.5% de los usuarios de Alto uso está en el plan Basico, lo que es una oportunidad de upgrade. Su tasa de bajas es algo mayor (14.0% frente a 11.4–11.5%), aunque con 278 usuarios la diferencia no es concluyente.
- **Calidad de datos:** 55 edades con `-999`, 565 usuarios sin ciudad (14.13%), 40 fechas de registro en 2026, 46 bajas anteriores a la fecha de registro y eventos extremos repetidos (120 minutos y 1,490 caracteres) que conviene validar.

## ▶️ Cómo ejecutar el notebook

### Opción 1: Google Colab

1. Haz clic en el badge **Open In Colab** de arriba.
2. Sube los 3 CSV a Colab o tenlos en la carpeta `data/` del repositorio (ver la nota sobre rutas).
3. Ejecuta **Runtime → Run all**.

### Opción 2: Local (Jupyter)

```bash
pip install pandas numpy seaborn matplotlib jupyter
jupyter notebook notebooks/S7_Version-Estudiante-Project-ConnectaTel.ipynb
```

### ⚠️ Nota sobre las rutas de los datos

En la plataforma de TripleTen, los archivos se leen desde `/datasets/`:

```python
plans = pd.read_csv('/datasets/plans.csv')
users = pd.read_csv('/datasets/users_latam.csv')
usage = pd.read_csv('/datasets/usage.csv')
```

Si ejecutas el notebook desde este repositorio, cambia la ruta a la carpeta `data/`:

```python
plans = pd.read_csv('../data/plans.csv')
users = pd.read_csv('../data/users_latam.csv')
usage = pd.read_csv('../data/usage.csv')
```

En Colab, si clonas el repo, usa la ruta desde la raíz del proyecto (por ejemplo `/content/connectatel-analysis/data/plans.csv`).

## 🔁 Guía de reproducción

1. Clona el repositorio: `git clone https://github.com/valercardenas10-rgb/connectatel-analysis.git`
2. Verifica que los 3 CSV estén en `data/`.
3. Ajusta las rutas de la celda de carga según el entorno (ver la nota de arriba).
4. Ejecuta el notebook de arriba abajo (**Restart & Run All**). Las celdas dependen del orden: la limpieza del paso 3 debe correr antes del resumen del paso 4.

## 📁 Estructura del repositorio

```
connectatel-analysis/
├── data/
│   ├── plans.csv
│   ├── users_latam.csv
│   └── usage.csv
├── notebooks/
│   └── S7_Version-Estudiante-Project-ConnectaTel.ipynb
└── README.md
```

## 🛠️ Herramientas

Python · pandas · numpy · seaborn · matplotlib · Jupyter Notebook
