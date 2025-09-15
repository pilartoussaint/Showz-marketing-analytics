# 🎟️ Showz — Marketing Analytics & Growth (2017–2018)

## 📌 Contexto
Has sido invitada a realizar prácticas en *Showz*, una plataforma de venta de entradas de eventos.  
Tu primera misión es *optimizar el gasto de marketing* respondiendo preguntas clave sobre *adquisición, activación, retención y monetización*.

Trabajarás con:
- Registros de *visitas* (servidor) — 2017-01 a 2018-12
- *Pedidos* (ingresos) en el mismo periodo
- *Gastos de marketing* por fuente/día

---

## 🎯 Objetivos
1. Entender *cómo usan el servicio* los clientes y *cuándo comienzan a comprar*.
2. Calcular *cuánto aporta cada cliente* (LTV) y *cuándo se recupera* el coste de adquisición (Payback).
3. Evaluar *rentabilidad por fuente* de adquisición (ROMI) y *dispositivo*.
4. Recomendar *dónde invertir* y *cuánto*.

---

## 🗂️ Datos y rutas
Coloca los CSV en ./datasets/ (o ajusta las rutas en el notebook):

- /datasets/visits_log_us.csv — visitas (servidor)
  - Uid (usuario), Device, Start Ts, End Ts, Source Id  
  (Fechas en AAAA-MM-DD; convertir a datetime)

- /datasets/orders_log_us.csv — pedidos
  - Uid, Buy Ts, Revenue

- /datasets/costs_us.csv — gastos de marketing
  - source_id, dt, costs

---

## 🛠️ Metodología (pipeline)
*Paso 1 — Acceso y preparación*
- Carga de datos, tipificación (datetime, category, float)
- Limpieza/normalización (nulos, duplicados, consistencia de Device y Source Id)
- Construcción de *sesiones*: duración = End Ts − Start Ts
- Generación de calendarios *diario/semanal/mensual*

*Paso 2 — Métricas operativas y de producto*
- *Uso*  
  - *DAU/WAU/MAU*: usuarios únicos por día/semana/mes  
  - *Sesiones por día* y por usuario  
  - *Duración media de sesión*  
  - *Retention* (cohortes por primera visita/registro) y *return rate*
- *Ventas*  
  - *Time-to-first-purchase* (Conversion 0d, 1d, …) por cohorte/canal  
  - *# pedidos* por ventana temporal  
  - *AOV* (Average Order Value) = Revenue / # pedidos  
  - *ARPU* (Average Revenue per User) = Revenue / # usuarios
  - *LTV* (cumulativo) ≈ suma de ARPU por usuario/cohorte a lo largo del tiempo*
- *Marketing*  
  - *Spend* total/por fuente/por tiempo  
  - *CAC* (Coste de Adquisición) por fuente = Spend fuente / # nuevos usuarios fuente  
  - *ROMI* = (Ingresos atribuibles − Spend) / Spend  (o LTV/CAC − 1 según enfoque)
  - *Payback* = meses hasta que LTV ≥ CAC

\* Nota: si Revenue ya es margen de contribución, LTV es directo; si es ingreso bruto, documenta el supuesto (p.ej., margen = 100% por falta de datos).

*Dimensiones*
- *Dispositivo* (Device)
- *Fuente* de adquisición (Source Id)
- *Tiempo* (día/semana/mes, cohortes)

*Visualizaciones*
- Series temporales (DAU/WAU/MAU, Spend, Revenue)  
- Barras apiladas por *fuente/dispositivo*  
- Cohort plots (retención, time-to-conversion)  
- Diagramas de caja para sesión/ingreso si procede

---

## 📊 Preguntas guía (del proyecto)
*Visitas*
- ¿Cuántas personas usan Showz cada *día/semana/mes*?
- ¿Cuántas *sesiones/día*? ¿y por usuario?
- *Duración media* de sesión
- *Frecuencia de retorno*

*Ventas*
- ¿Cuándo empiezan a comprar? (Conversion 0d, 1d… por cohorte/canal)
- ¿Cuántos *pedidos* por periodo?
- *Ticket promedio* (AOV)
- *LTV* por cohorte/fuente

*Marketing*
1) *Gasto* total/por fuente/over time  
2) *CAC* por fuente  
3) *ROMI* y *Payback* por fuente

> Traza gráficos comparando *dispositivos* y *fuentes, y **evolución* temporal.

---

## 🧮 Fórmulas clave (resumen)
- DAU = usuarios únicos por día  
- AOV = sum(Revenue) / # pedidos  
- ARPU = sum(Revenue) / # usuarios  
- LTV_t (cohorte) ≈ Σ ARPU_mes hasta t  
- CAC_fuente = Σ costs_fuente / # nuevos usuarios_fuente  
- ROMI = (Ingresos atribuibles − Σ costs) / Σ costs = LTV/CAC − 1 (en versión per-user)  
- Payback = min t s.t. LTV_t ≥ CAC

---

## ▶️ Cómo ejecutar
1. Clona este repo  
2. Instala dependencias:
   ```bash
   pip install -r requirements.txt
