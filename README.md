# Neurona de Izhikevich — TP Redes Neuronales 2026

Simulación numérica del modelo de neuronas de Izhikevich (2003): una neurona individual integrada con **Runge-Kutta 4** y una **red de 1000 neuronas** con ruido integrada con **Euler-Maruyama**.


---

## El modelo

El modelo reduce la dinámica de Hodgkin-Huxley a dos ecuaciones:

```
dv/dt = g2·v² + g1·v + g0 − u + I(t)
du/dt = b·(c·v − u)

si v ≥ v+  →  v ← v−,   u ← u + Δu
```

| Símbolo | Qué representa |
|---|---|
| `v` | Potencial de membrana (mV) |
| `u` | Variable de recuperación (activación de K⁺ / inactivación de Na⁺) |
| `b` | Velocidad de recuperación de `u` |
| `c` | Sensibilidad de `u` a las fluctuaciones de `v` |
| `v−` | Valor de reseteo de `v` después de un disparo |
| `Δu` | Incremento de `u` después de un disparo |
| `I(t)` | Corriente de entrada externa |

Parámetros base: `g2 = 0.04`, `g1 = 5`, `g0 = 140`, `v+ = 30 mV`.

> Nota: la notación `b, c, v−, Δu`, corresponde a `a, b, c, d` del paper original.

---

## Contenido

### Parte 1 — Neurona individual

1. Integración con RK4 en `t ∈ [0, 200]` ms, paso `h = 0.1`, con `v(0) = −70` y `u(0) = c·v(0)`.
2. Corriente escalón: `I = 0` si `t < 10`, `I = 10` si `t ≥ 10`.
3. Reproducción de los 8 patrones de disparo de la Figura 2 del paper:

| Caso | Nombre | b | c | v− | Δu |
|---|---|---|---|---|---|
| RS | Regular spiking | 0.02 | 0.2 | −65 | 8 |
| IB | Intrinsically bursting | 0.02 | 0.2 | −55 | 4 |
| CH | Chattering | 0.02 | 0.2 | −50 | 2 |
| FS | Fast spiking | 0.1 | 0.2 | −65 | 2 |
| TC1 | Thalamo-cortical (despolarizado) | 0.02 | 0.25 | −65 | 0.05 |
| TC2 | Thalamo-cortical (hiperpolarizado) | 0.02 | 0.25 | −65 | 0.05 |
| RZ | Resonator | 0.1 | 0.26 | −65 | 2 |
| LTS | Low-threshold spiking | 0.02 | 0.25 | −65 | 2 |

### Parte 2 — Red de neuronas

- Traducción a Python del código MATLAB del paper.
- 800 neuronas excitatorias + 200 inhibitorias con parámetros heterogéneos (`r_i ~ U[0,1]`).
- Matriz de conexiones: `a_ij = 0.5·r_ij` (excitatorias) y `a_ij = −r_ij` (inhibitorias).
- Ruido gaussiano (σ = 5 excitatorias, σ = 2 inhibitorias) integrado con Euler-Maruyama.
- Reemplazo del escalón de Heaviside por `z(v) = (87 + v)/450 − 0.0193`.
- Reproducción de la Figura 3 del paper (raster plot de disparos).

---

## Estructura del repositorio

```
Neurona-Izhikevich/
├── README.md
├── requirements.txt
├── notebooks/
│   ├── parte1_neurona_individual.ipynb
│   └── parte2_red_neuronas.ipynb
├── src/
│   ├── izhikevich.py        # ecuaciones del modelo y reseteo
│   ├── integradores.py      # RK4 y Euler-Maruyama
│   └── corrientes.py        # I1, I2, I3, I4
├── figuras/                 # gráficos exportados para el informe
└── informe/
    └── TP1_Apellidos.pdf    # informe final (máx. 4 páginas)
```

---

**Dependencias:** Python 3.10+, `numpy`, `matplotlib`, `jupyter`.

---

## Referencias

1. Izhikevich, E. M. (2003). *Simple model of spiking neurons*. IEEE Transactions on Neural Networks, 14(6), 1569–1572.
2. Izhikevich, E. M. (2006). *Dynamical Systems in Neuroscience: The Geometry of Excitability and Bursting*. MIT Press.
3. Hodgkin, A. L., & Huxley, A. F. (1952). *A quantitative description of membrane current and its application to conduction and excitation in nerve*. The Journal of Physiology, 117(4), 500–544.

---


Docentes: Francisco Tamarit, Juan Perotti, Tristán Osán, Franco Milana.
