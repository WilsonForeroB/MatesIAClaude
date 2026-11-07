# Pizarra MatIA

Pizarra para dar clase de matemáticas: fórmulas con atajos de teclado, matrices, sistemas, desarrollos alineados por el «=», operaciones paso a paso, gráficas y dibujo a mano alzada. Sigue el sistema de diseño de MatIA.

> Generado con asistencia de IA (Claude) para Mercanza. Pendiente de revisión por el desarrollador responsable antes de usarse en producción.

## Estructura

```
pizarramatia/
├── pizarra.html      ← la pizarra (un único fichero, sin instalación)
├── README.md         ← este documento
└── historial/        ← las clases guardadas en JSON (no se suben a Git)
    └── .gitkeep
```

- **`pizarra.html`** contiene todo: estilos, editor y motor de cálculo. Se abre con doble clic.
- **`historial/`** guarda una clase por archivo `.json`. Está en el `.gitignore` (`pizarramatia/historial/*`), así que las pruebas y las clases no se suben al repositorio. Solo se versiona `.gitkeep` para que la carpeta exista al clonar.

## Requisitos

- **Navegador recomendado: Chrome o Edge** (escritorio). Son los que dejan que la página guarde directamente en la carpeta `historial`.
- **Firefox y Safari** también sirven para dar clase. En el historial solo pueden *leer* la carpeta; para guardar descargan el JSON y hay que moverlo a `historial/` a mano.
- **Conexión a internet al abrir la página**: el editor de fórmulas (MathLive 0.108.2, licencia MIT) y las fuentes se cargan de un CDN. Una vez abierta, todos los cálculos se hacen en el navegador, sin red ni servidores.

## Cómo funciona el historial

1. Abre `pizarra.html` y pulsa **Historial**.
2. Pulsa **Conectar carpeta** y elige `pizarramatia/historial`. Si eliges `pizarramatia`, se usa (o se crea) su subcarpeta `historial`. El navegador pide permiso para leer y guardar en ella.
3. Escribe un **título de la clase** y pulsa **Guardar en el historial**. Se crea un archivo con la fecha y el título, por ejemplo `2026-11-07_10-48_ecuaciones-de-primer-grado.json`.
4. Si marcas **Guardado automático**, ese mismo archivo se reescribe unos segundos después de cada cambio.
5. En la lista del historial puedes:
   - **Abrir**: sustituye la pizarra actual por la clase guardada.
   - **Añadir páginas**: añade sus páginas detrás de las actuales, para reutilizar ejercicios.
   - **Borrar**: elimina el archivo de la carpeta, tras pedir confirmación.

La pizarra recuerda la carpeta. La próxima vez basta con pulsar **Reconectar** si el navegador vuelve a pedir permiso.

Además, la pizarra se guarda sola en el propio navegador (`localStorage`) y, desde **Archivo**, se puede descargar o abrir un `.json` suelto, o imprimir y guardar como PDF.

### Formato del JSON

```json
{
  "app": "MatIA Pizarra",
  "formato": 1,
  "guardado": "2026-11-07T09:48:00.000Z",
  "title": "Ecuaciones de primer grado",
  "pages": [
    {
      "blocks": [
        { "type": "math", "v": "x+1=5", "align": "left" },
        { "type": "deriv", "rows": [{ "l": "x+1", "r": "5", "note": "Planteamiento" }] },
        { "type": "table", "rows": [["x", "-1", "0", "1"], ["f(x)", "1", "0", "1"]] },
        { "type": "graph", "fns": ["f(x)=x^2"], "xmin": -5, "xmax": 5 }
      ],
      "strokes": []
    }
  ],
  "cur": 0
}
```

Tipos de bloque:

| Tipo | Qué es |
| --- | --- |
| `math` | Línea de fórmula en LaTeX. |
| `text` | Línea de texto. |
| `deriv` | Desarrollo alineado por el «=»: filas con lado izquierdo `l`, lado derecho `r` y explicación `note`. |
| `table` | Tabla de Ruffini, de verdad o de valores. |
| `graph` | Gráfica de una o varias funciones. |

`strokes` guarda el dibujo a mano alzada de cada página. Los archivos antiguos sin `app` ni `formato` también se abren.

## Uso en clase (resumen)

Pulsa **?** dentro de la pizarra para ver todos los atajos.

- **Escribir**: `x^2`, `1/2`, `sqrt`, `in` (∈), `EE` (∃), `AA` (∀), `RR` (ℝ)… Intro crea una línea nueva.
- **Menús**: Matriz, Sistema, Desarrollo, Símbolos y Cálculo (derivadas, integrales y límites).
- **Desarrollo** (Alt+L): pasos alineados por el «=» con explicación a la derecha. Intro copia la fila anterior para editar solo lo que cambia.
- **Calcular** (Ctrl+=) actúa sobre la línea en la que escribes.
- **Operaciones ▾**, **Gauss** y **Gauss-Jordan**: al elegirlos aparece la barra **«Elige la línea»**. Haz clic en la línea o en el paso del desarrollo al que quieres aplicar la operación (se resalta al pasar el ratón). **Intro** usa la línea activa y **Esc** cancela. Así, con varias ecuaciones en la misma página, decides cuál se resuelve.
- **Operaciones ▾**: todas paso a paso.

| Grupo | Operaciones |
| --- | --- |
| Matrices | Inversa por Gauss-Jordan; determinante por 2×2/Sarrus, cofactores o Gauss; regla de Cramer; discusión de sistemas con un parámetro; producto desarrollado; autovalores, autovectores y diagonalización. |
| Vectores | Producto escalar y vectorial, norma, ángulo y proyección. |
| Ecuaciones | Resolver paso a paso (1.º y 2.º grado, polinómicas con raíces racionales), siguiente paso y comprobación. |
| Aritmética | Simplificar y operar fracciones, m.c.d. y m.c.m. (factores primos y Euclides), descomposición en factores primos. |
| Polinomios | Desarrollar, Ruffini o división larga, raíces y factorización. |
| Combinatoria | n!, combinaciones, variaciones y permutaciones; triángulo de Pascal. |
| Lógica y conjuntos | Tabla de verdad con su clasificación; unión, intersección, diferencia, complementario, partes y producto cartesiano. |
| Cálculo | Derivada con las reglas aplicadas, gráfica de funciones (rueda para acercar, arrastrar para mover) y tabla de valores. |

- **Dibujar** (Alt+D): lápiz, rotulador, flecha y goma. Un lápiz digital dibuja siempre, aunque estés en modo Escribir.
- **Modo proyector**: pantalla completa, tamaño de letra, tema claro u oscuro y ocultar barras (H).
- **Páginas**: «+ Página» añade una página detrás de la actual y «− Página» elimina la actual; si es la única, la vacía.
- **Avisos con Pío**: las confirmaciones (borrar página o clase, pizarra nueva) usan un diálogo propio con los colores de la pizarra, no los avisos del navegador. Si vas a abrir otra clase, empezar una nueva o abrir un archivo con cambios sin guardar, la pizarra ofrece **Guardar y continuar**, **Continuar sin guardar** o **Cancelar**. Al cerrar la pestaña no pregunta nada: la pizarra ya está guardada en el navegador y, con el guardado automático, también en el historial.
- **Pío** acompaña en la cabecera (logotipo), en la página vacía, en la barra «Elige la línea», en el menú de operaciones, en la ayuda y en el historial.

## Límites conocidos

- El cálculo es exacto con fracciones. Las raíces se simplifican, pero no hay un motor simbólico completo: las ecuaciones racionales o con varias incógnitas no se resuelven paso a paso.
- La discusión con parámetro admite un parámetro y sistemas cuadrados.
- Los autovalores irracionales se dan con la fórmula exacta en el caso de grado 2. En grados mayores solo se buscan raíces racionales.

## Desarrollo

- `pizarra.html` es autocontenido; las únicas dependencias externas son MathLive (CDN jsDelivr, versión fija) y Google Fonts (solución temporal según el sistema de diseño).
- No guarda credenciales ni datos personales. El historial vive solo en la carpeta local de quien la usa.
