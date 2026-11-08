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
  "paper": "slide",
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
| `graph` | Gráfica 2D de una o varias funciones, con `marks` opcionales (máximos, mínimos, inflexión, absolutos). |
| `graph3d` | Superficie z = f(x, y) con su rango, su vista (`az`, `el`, `zoom`) y `marks` opcionales (mínimos, máximos, puntos de silla). |
| `chart` | Gráfico estadístico: `kind` = `bars` (barras, con `hi` resaltadas), `box` (diagrama de caja) o `normal` (campana con el área entre `a` y `b`). |
| `ptree` | Árbol de probabilidad: causas con `p`, `l` = P(suceso \| causa) y los productos de cada camino. |
| `venn` | Diagrama de Venn: `n` (2 o 3), `expr` (expresión sombreada) o `shade` (zonas elegidas a mano, como máscaras A = 1, B = 2, C = 4) y `showEls`. |
| `net` | Grafo: `nodes` (con `label`, `x`, `y` y tipo de capa), `edges` (con peso opcional `w`), `directed` y rótulos de capa `caps`. |

`strokes` guarda el dibujo a mano alzada de cada página. Los archivos antiguos sin `app` ni `formato` también se abren.

## Uso en clase (resumen)

Pulsa **?** dentro de la pizarra para ver todos los atajos.

- **Escribir**: `x^2`, `1/2`, `sqrt`, `in` (∈), `EE` (∃), `AA` (∀), `RR` (ℝ)… Intro crea una línea nueva.
- **Menús**: Matriz, Sistema, Desarrollo, **Símbolos ▾** (básicos —potencia, subíndice, fracción, raíz, paréntesis, traspuesta, inversa, flecha de paso—, lógica, conjuntos, números, relaciones, operaciones, flechas y griegas) y **Cálculo ▾** (derivadas, integrales y límites). Los símbolos van todos dentro de los desplegables para que la barra no ocupe espacio.
- **Desarrollo** (Alt+L): pasos alineados por el «=» con explicación a la derecha. Intro copia la fila anterior para editar solo lo que cambia.
- **Calcular** (Ctrl+=) actúa sobre la línea en la que escribes.
- **Operaciones ▾**: al elegir una operación aparece la barra **«Elige la línea»**. Haz clic en la línea o en el paso del desarrollo al que quieres aplicar la operación (se resalta al pasar el ratón). **Intro** usa la línea activa y **Esc** cancela. Así, con varias ecuaciones en la misma página, decides cuál se resuelve.
- **Operaciones ▾**: todas paso a paso.

| Grupo | Operaciones |
| --- | --- |
| Matrices y sistemas | Gauss y Gauss-Jordan paso a paso (matriz, ampliada o sistema, con su clasificación); inversa por Gauss-Jordan; determinante por 2×2/Sarrus, cofactores o Gauss; regla de Cramer; discusión de sistemas con un parámetro; producto desarrollado; autovalores, autovectores y diagonalización. |
| Vectores | Producto escalar y vectorial, norma, ángulo y proyección. |
| Ecuaciones | Resolver paso a paso (1.º y 2.º grado, polinómicas con raíces racionales), siguiente paso y comprobación. |
| Aritmética | Simplificar y operar fracciones, m.c.d. y m.c.m. (factores primos y Euclides), descomposición en factores primos. |
| Polinomios | Desarrollar, Ruffini o división larga, raíces y factorización. |
| Estadística | Medidas de una lista de datos: media, mediana, moda, rango, varianza y desviación típica (poblacional y muestral), cuartiles, RIC y coeficiente de variación, con diagrama de caja. Tabla de frecuencias (nᵢ, fᵢ, Nᵢ, Fᵢ, %) con diagrama de barras. Los datos se separan con comas, o con `;` si usas coma decimal. |
| Probabilidad | Espacio muestral (moneda, dado, conjuntos como `{R,V,A}` o `moneda^3`) con regla de Laplace, y la tabla de sumas si son dos dados. Tabla de contingencia con marginales, conjuntas, condicionadas e independencia. **Asistente de árbol de probabilidad y Bayes**, con el árbol, la probabilidad total y las probabilidades a posteriori. Binomial `X\sim B(10, 0.3), P(X\le 3)` con diagrama de barras resaltado. Normal `X\sim N(100, 15), P(85\le X\le 115)` tipificando, con la campana y el área sombreada. |
| Combinatoria | n!, combinaciones, variaciones y permutaciones; triángulo de Pascal. |
| Lógica y conjuntos | Tabla de verdad con su clasificación; unión, intersección, diferencia, complementario, partes y producto cartesiano; **diagrama de Venn** de la expresión de la línea. |
| Cálculo | Derivada con las reglas aplicadas; **máximos y mínimos de f(x)** (puntos críticos, clasificación con f″ o con el signo de f′, puntos de inflexión, tabla de crecimiento, extremos absolutos en ±∞ o en un intervalo `x∈[a,b]` y gráfica con los puntos marcados); **extremos de f(x, y)** (gradiente, puntos críticos, hessiana y clasificación en mínimo, máximo o punto de silla, con la superficie 3D marcada); gráfica de funciones y tabla de valores. |

- **Gráficos ▾**:
  - **Gráfica 2D**: funciones y = f(x); usa la función de la línea activa y admite varias separadas por `;`. Se acerca con la rueda y se mueve arrastrando.
  - **Gráfica 3D**: superficies z = f(x, y), coloreadas de bajo a alto. Se gira arrastrando, se acerca con la rueda y se cambian la función y los rangos de x e y.
  - **Grafo**: herramientas Mover, Nodo (clic en un hueco), Arista (arrastrar de un nodo a otro) y Borrar. Doble clic para renombrar un nodo o poner peso a una arista. Puede ser dirigido o no, y el botón **Matriz de adyacencia** añade la matriz y los grados debajo.
  - **Diagrama de Venn**: 2 o 3 conjuntos (A, B, C) dentro del universal U. Escribe la expresión (A ∩ B, A ∪ B, A ∖ B, Aᶜ, (A ∪ B)ᶜ, A Δ B…) con el teclado o con sus botones y se sombrea su zona, con color y rayado. Si en la página has definido `A = {…}`, `B = {…}`, `C = {…}` o `U = {…}`, cada elemento aparece en su zona y debajo sale el conjunto resultado. Con un clic en una zona la sombreas o la quitas a mano.
  - **Asistente de red neuronal**: escribe las neuronas por capa (por ejemplo `3, 4, 4, 2`) y monta el grafo por capas: entradas x₁…, ocultas h¹₁… y salidas ŷ₁…, con sesgo (+1) y pesos w opcionales. También está en el grafo, con el botón «Red neuronal…».
- **Dibujar** (Alt+D): lápiz, rotulador, flecha y goma. Un lápiz digital dibuja siempre, aunque estés en modo Escribir.
- **Tamaño de página** (Vista ▾): **Libre** (continua, como siempre), **A4 vertical**, **A4 horizontal**, **A3 horizontal** o **Diapositiva 16:9**. En los tamaños fijos se ve el borde de la hoja, la hoja se ajusta al ancho de la pantalla, se marca con una línea discontinua dónde termina la página si el contenido no cabe, y al imprimir o guardar en PDF cada página sale con ese tamaño.
- **Presentar** (botón de la cabecera): pantalla completa con solo la página actual, escalada para que quepa entera, como una diapositiva. **→ / ← / espacio / RePág / AvPág** para pasar, **Inicio / Fin** para ir a la primera o la última, **D** para dibujar encima y **Esc** para salir. Abajo hay una barra que se oculta sola. Recomendado con «Diapositiva 16:9».
- **Exportar gráficos**: cada gráfico (2D, 3D, grafo, red neuronal, Venn, barras, caja, normal, árbol) tiene un botón ⤓ junto a la ×. Permite **descargar PNG** a doble resolución, **descargar SVG** (vectorial; no disponible en la 3D) o **copiar la imagen** para pegarla con Ctrl+V. La imagen incluye el título, la leyenda y el resultado. Con «Fondo claro» sale en colores claros aunque uses el tema oscuro.
- **Modo proyector**: pantalla completa (icono de la cabecera o tecla F), tamaño de letra, tema claro u oscuro y ocultar barras (H).
- **Guardar** (botón verde de la cabecera o **Ctrl+S**): guarda la clase en el historial. Un punto blanco en el botón indica que hay cambios sin guardar. La primera vez pide la carpeta `historial`; después reescribe siempre el archivo de esa clase. En navegadores sin acceso a carpetas, descarga el JSON.
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

## Autoría y derechos

**Pizarra MatIA**, creada por **Wilson Forero** ([github.com/WilsonForeroB](https://github.com/WilsonForeroB)) con asistencia de inteligencia artificial (Claude, de Anthropic).

© 2026 Wilson Forero. **Todos los derechos reservados.** Uso exclusivamente educativo y no comercial. Queda prohibida cualquier reproducción, distribución, comunicación pública, transformación o explotación, total o parcial, con fines comerciales sin autorización expresa y por escrito del autor; su incumplimiento podrá ser objeto de acciones legales conforme a la legislación de propiedad intelectual aplicable. Ver también el `LICENSE` de la raíz del repositorio.

Componentes de terceros, con sus propias licencias: MathLive (© Arno Gourdol, licencia MIT) y las fuentes IBM Plex Sans, JetBrains Mono y Source Serif 4 (SIL Open Font License 1.1).

El aviso aparece también dentro de la pizarra: al pie de la ayuda (**?**), en **Archivo → Acerca de y derechos** y en los metadatos del HTML.
