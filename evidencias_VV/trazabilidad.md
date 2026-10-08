# Trazabilidad Modelo Conceptual ↔ Código Python

| Elemento del modelo conceptual       | Implementación en Python                             | Verificado | Evidencia                          |
|--------------------------------------|------------------------------------------------------|------------|------------------------------------|
| Llegadas exponenciales (λ)           | `np.random.exponential(1.0 / lam)`                  | ✓          | Histograma de inter-llegadas       |
| Tiempo de atención Normal truncada   | `max(0.1, np.random.normal(mu, sigma))`             | ✓          | Q-Q plot / histograma vs Normal    |
| Cola FIFO con c servidores           | Lista `agents[c]`; asignación a `min(agents)`       | ✓          | Inspección de asignaciones         |
| Paciencia exponencial del cliente    | `np.random.exponential(patience_mean)` vs wait_time | ✓          | Contador `abandoned_calls`         |
| KPI: Nivel de servicio (SLA 80/20)   | `sum(w < 0.333) / len(waiting_times)`               | ✓          | Definición 20 s = 0.333 min        |
| KPI: Tiempo de espera promedio       | `np.mean(waiting_times)` — solo llamadas atendidas  | ✓          | Abandonos excluidos correctamente  |
| Réplicas independientes (Monte Carlo)| Loop `for _ in range(REPLICAS)`                     | ✓          | Varianza inter-réplica calculada   |
| Semilla reproducible                 | `np.random.seed(SEED)` al inicio del script         | ✓          | Resultados idénticos con seed=42   |

## Observaciones de lógica

- Los abandonos **no** se registran en `waiting_times`, de modo que `avg_wait` 
  refleja únicamente llamadas efectivamente atendidas. Esto es correcto.
- La paciencia se genera **una vez por llamada** (independiente del agente asignado), 
  evitando dependencias espurias.
- La asignación usa `agents.index(min(agents))`: en caso de empate elige el primer 
  agente disponible, comportamiento FIFO coherente.
- El tiempo de servicio se trunca en `max(0.1, ...)` para evitar tiempos negativos 
  derivados de la distribución Normal.
