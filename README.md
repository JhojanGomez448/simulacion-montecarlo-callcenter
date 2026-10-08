# Simulación Monte Carlo — Optimización Operativa de un Call Center

**Autor:** Jhojan Raul Zambrano Gomez
**Curso:** Modelado y Simulación — Fundación Universitaria Compensar
**Docente:** Alexander Reyes Moreno
**Fecha:** Mayo 2026

## Descripción

Simulación de eventos discretos (Monte Carlo) para determinar cuántos agentes necesita un Call Center. El modelo calibra λ, μ y σ directamente desde `Call_Center_Data.csv` y compara configuraciones de 2, 3, 5 y 10 agentes, además de escenarios *what-if* de demanda y duración de llamadas.

**Resultado clave:** con 5 agentes se obtiene un nivel de servicio de ~99.8 % y casi 0 abandonos por jornada, incluso con +30 % de demanda (~99.4 %).

## Estructura

```
├── Jhojan_Modelado_y_Simulacion.ipynb   ← Modelo principal, escenarios y gráficas
├── verificacion_VV.ipynb                ← Verificación y validación (genera evidencias_VV/)
├── Call_Center_Data.csv                 ← Dataset (1 251 registros)
├── requirements.txt
├── informe/
│   ├── Jhojan_Modelado_y_Simulacion.pdf ← Informe técnico
│   └── Poster_PFC_ZambranoJhojan.pdf    ← Póster
└── evidencias_VV/
    ├── trazabilidad.md
    ├── pruebas_extremos.txt
    ├── validacion_datos_reales.txt
    ├── sensibilidad.csv
    └── fig_sensibilidad.png
```

## Instalación y ejecución

```bash
git clone https://github.com/JhojanGomez448/simulacion-montecarlo-callcenter.git
cd simulacion-montecarlo-callcenter
pip install -r requirements.txt
jupyter notebook
```

1. Abre `Jhojan_Modelado_y_Simulacion.ipynb` y ejecuta todas las celdas (*Run All*).
2. Abre `verificacion_VV.ipynb` y ejecútalo para regenerar `evidencias_VV/`.

Ambos notebooks deben ejecutarse con `Call_Center_Data.csv` en la misma carpeta.

## Reproducibilidad

Semilla `42` y `500` réplicas por configuración (variables `SEED` y `simulations`/`REPLICAS`). Las cifras exactas pueden variar ligeramente entre ejecuciones del notebook principal y las del informe.

## Parámetros calibrados

| Parámetro | Valor | Distribución |
|---|---|---|
| λ (llegadas) | ≈ 0.414 llam/min | Exponencial (inter-llegadas) |
| Tiempo de atención | μ = 2.63 min, σ = 0.395 min | Normal truncada (≥ 0.1) |
| Paciencia del cliente | media = 3 min | Exponencial |
| Turno simulado | 480 min | — |
| SLA | 80 % en < 20 s | — |

## Escenarios what-if

| Escenario | Factor λ | Factor μ | Agentes |
|---|---|---|---|
| Base | ×1.00 | ×1.00 | 3, 5, 7 |
| Alta demanda (+30 %) | ×1.30 | ×1.00 | 3, 5, 7 |
| Mayor duración (+20 %) | ×1.00 | ×1.20 | 3, 5, 7 |

## Verificación y validación

- **Trazabilidad** modelo conceptual ↔ código: `evidencias_VV/trazabilidad.md`
- **Pruebas de extremos** (c = 0, c = 1000, monotonicidad): `evidencias_VV/pruebas_extremos.txt`
- **Validación con datos reales**: abandono real 10.93 % vs. modelo (2 agentes) 9.16 %
- **Sensibilidad** (λ, μ, paciencia): `evidencias_VV/sensibilidad.csv`

## Limitaciones

Sin variación intradiaria de la demanda, agentes homogéneos, una sola cola y sin tipos de llamada. Como trabajo futuro: proceso de Poisson no homogéneo, enrutamiento por habilidades y optimización de costos.
