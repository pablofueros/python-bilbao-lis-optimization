# ¿Hasta dónde podemos optimizar un algoritmo? Python & Rust — Python Bilbao

Materiales y código de benchmarking de la charla **«¿Hasta dónde podemos
optimizar un algoritmo? Python & Rust»**, presentada por Pablo García en Python
Bilbao el 30 de octubre de 2025, en la Universidad de Deusto.

La charla utiliza el problema [Longest Increasing Subsequence
(LIS)](https://en.wikipedia.org/wiki/Longest_increasing_subsequence) para
separar distintas fuentes de mejora del rendimiento: la complejidad
algorítmica, las decisiones de implementación, el comportamiento del runtime,
el lenguaje y el modo de compilación de Rust.

## Contenido

- [`slides.pdf`](slides.pdf): diapositivas presentadas en el evento.
- [`python/`](python/): implementaciones en Python, desde enfoques
  exponenciales hasta una solución O(n log n).
- [`rust/`](rust/): implementaciones equivalentes en Rust ejecutables con
  `rust-script`.
- [`python/generator.py`](python/generator.py): generación reproducible de los
  datos de entrada.
- [`benchmarks.sh`](benchmarks.sh): comandos de Hyperfine utilizados para
  recoger los resultados.
- [`analysis.py`](analysis.py): notebook de Marimo para comparar y visualizar
  los resultados de Hyperfine.

Los datos de entrada y los resultados de los benchmarks se generan localmente
y están ignorados por Git. Dependen de la máquina utilizada y pueden
reproducirse a partir del código versionado.

## Requisitos

- [uv](https://docs.astral.sh/uv/) para Python y las dependencias del proyecto.
- [rust-script](https://rust-script.org/) para ejecutar los programas Rust
  independientes.
- [Hyperfine](https://github.com/sharkdp/hyperfine) para los benchmarks.

Instala el entorno de Python a partir del lockfile:

```bash
uv sync --frozen
```

## Ejecutar las implementaciones

Primero genera los datos deterministas compartidos:

```bash
uv run python python/generator.py
```

Después ejecuta cualquier implementación con uno de los tamaños disponibles:

```bash
uv run python python/lis_nlogn.py 90
rust-script rust/lis_nlogn.rs 90
```

Ambos programas leen el mismo archivo `data/sequence_90.json` e imprimen la
longitud de una subsecuencia estrictamente creciente de longitud máxima.

## Implementaciones

| Enfoque | Python | Rust | Complejidad |
| --- | --- | --- | --- |
| Backtracking | `lis_bruteforce.py` | `lis_bruteforce.rs` | O(2ⁿ) |
| Recursivo | `lis_recursive.py` | `lis_recursive.rs` | O(2ⁿ) |
| Optimizaciones recursivas | `lis_recursive_opt_1.py`, `lis_recursive_opt_2.py` | — | O(2ⁿ) |
| Variante recursiva con NumPy | `lis_recursive_np.py` | — | O(2ⁿ) |
| Programación dinámica | `lis_dp_n2.py` | `lis_dp_n2.rs` | O(n²) |
| Método de tails/patience sorting | `lis_nlogn.py` | `lis_nlogn.rs` | O(n log n) |

Las implementaciones exponenciales son ejemplos didácticos. Utiliza tamaños
de entrada pequeños: su tiempo de ejecución crece muy rápido.

## Benchmarks y análisis

[`benchmarks.sh`](benchmarks.sh) conserva los comandos utilizados para la
charla. Revisa los intérpretes y tamaños seleccionados antes de ejecutar el
script completo: algunos experimentos son intencionadamente lentos.

Después de generar los resultados, abre el notebook de análisis con:

```bash
uv run marimo edit analysis.py
```

Las gráficas comparan distintos runtimes de Python, la ejecución de Rust en
modo debug y release, y las optimizaciones recursivas incrementales utilizadas
en la presentación.
