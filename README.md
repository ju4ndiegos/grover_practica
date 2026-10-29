# Práctica: el algoritmo de Grover

Un solo notebook de Jupyter, [`grover_practica.ipynb`](grover_practica.ipynb), para entender el algoritmo de
búsqueda de Grover construyéndolo pieza por pieza con Qiskit y el simulador Aer. Todo corre en una laptop, sin
cuenta de IBM.

## Contenido

| Parte | Tema |
|---|---|
| 0 | Preparación: comprobar la instalación y funciones de apoyo |
| 1 | La compuerta Z multicontrolada `MCMTGate(ZGate(), n-1, 1)` (en los dibujos, `1-Z`), el orden de bits de Qiskit y los controles abiertos |
| 2 | ¿Qué es un oracle? Uno, varios o ningún estado marcado, oracles a partir de funciones, *phase kickback* |
| 3 | El difusor, la inversión respecto a la media y la imagen geométrica |
| 4 | El número de iteraciones t: P(t), t óptimo y sobre-rotación |
| 5 | Varios marcados o ninguno: M = N/4, N/2, > N/2, 0 y N; M desconocido |
| 6 | **Playground**: escribes estados marcados, el *oracle builder* del tutorial de IBM arma el circuito y ves el histograma y la curva P(t) |
| 7 | Reto: un oracle SAT con ancillas y un oracle secreto para descubrir |

Cada parte tiene teoría corta, código guiado y ejercicios. Los ejercicios se escriben donde dice
`# TU CÓDIGO AQUÍ`; muchos se corrigen solos (`check_oracle`, `check_t`, `check_secret`) y todos traen la
solución plegada en **Ver solución**. Ejecuta las celdas en orden: cada parte usa funciones de las anteriores.

El notebook es autocontenido: no depende de ningún módulo propio, solo de qiskit, qiskit-aer, numpy, matplotlib
e ipywidgets.

## Instalación

Necesitas **una** de estas dos opciones. Las dos funcionan en macOS, Linux y Windows.

### Opción A: uv (recomendada)

[uv](https://docs.astral.sh/uv/) instala Python y las versiones exactas de `uv.lock`.

1. Instala uv:
   - macOS / Linux: `curl -LsSf https://astral.sh/uv/install.sh | sh`
   - Windows (PowerShell): `powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"`
2. En la carpeta del repositorio, crea el entorno (descarga Python 3.12 si no lo tienes):

   ```bash
   uv sync
   ```

3. Abre Jupyter:

   ```bash
   uv run jupyter lab
   ```

### Opción B: conda / miniconda

```bash
conda env create -f environment.yml
conda activate grover-practica
jupyter lab
```

Para actualizar un entorno ya creado: `conda env update -f environment.yml --prune`.

### Si usas VS Code

Abre la carpeta y el notebook, y en **Select Kernel → Python Environments** elige `.venv` (uv) o
`grover-practica` (conda). Si la primera celda da `ModuleNotFoundError`, el kernel elegido no es el del entorno.

## Convención de bits

Igual que Qiskit: el string `"q_{n-1} … q_1 q_0"`. **El qubit 0 es el carácter de la derecha.**
`"011"` significa q0 = 1, q1 = 1, q2 = 0 (el entero 3).

## Fórmulas de referencia

Con N = 2ⁿ estados y M marcados:

- sin θ = √(M/N)
- P(éxito tras t iteraciones) = sin²((2t + 1)θ)
- t_opt = ⌊π / (4θ)⌋ ≈ (π/4)·√(N/M)
