# Reglas para Flashcards — Física I Coloquio

## Formato general
- Tamaño **A5 horizontal** (960×678px internos)
- Escala proporcional con JS para adaptarse a cualquier pantalla sin romper el layout
- Cada card tiene **dos caras** — se voltea con **animación 3D flip horizontal** al hacer click
- Navegación entre cards con **flechas adelante/atrás**

---

## Estructura de cada cara

### FRENTE
- Título centrado arriba, separado del resto por línea punteada
- Dos columnas debajo:
  - **Izquierda:** definición + fórmula principal + casos importantes
  - **Derecha:** diagrama SVG + nota técnica al pie
- Los bloques de la columna izquierda usan `justify-content: space-between` para llenar el alto disponible sin quedar amontonados arriba

### DORSO
- Dos columnas:
  - **Izquierda:** desarrollo/demostración paso a paso + unidad SI
  - **Derecha:** diagrama técnico de derivación + ejemplo numérico

---

## Diseño visual

- **Estética:** pastel académico — colores suaves diferenciados por tipo de contenido, muy aireado
- Cada sección tiene su **color identitario** con label flotante que rompe el borde superior (estilo cuaderno técnico, con `::before` posicionado en `top: -8px`)
- Tipografías:
  - **Fraunces** — títulos y display (serif con carácter)
  - **Inter Tight** — cuerpo de texto
  - **JetBrains Mono** — ecuaciones y fórmulas

---

## Paleta base

| Variable | Color | Uso |
|---|---|---|
| `--paper` | `#faf6ec` | Fondo general de la card |
| `--paper-2` | `#f5efde` | Fondos secundarios internos |
| `--ink` | `#1c1a17` | Texto principal |
| `--ink-2` | `#54514a` | Texto secundario |
| `--ink-3` | `#8b877e` | Texto terciario / labels |
| `--line` | `#d9d0bb` | Bordes y separadores |

---

## Colores por tipo de sección

| Sección | Fondo | Acento | Stroke |
|---|---|---|---|
| Definición | `#f3ecd9` ocre claro | `#8b6f3f` ocre | `#c9a86b` |
| Fórmula | `#e8eef2` azul gris | `#2d4f72` azul oscuro | `#7fa3c9` |
| Demostración / Cálculo | `#ede5e8` rosa polvo | `#8a4a5a` rosa oscuro | `#c89aa8` |
| Ejemplo | `#e3ece4` sage claro | `#3d5e44` sage oscuro | `#87b194` |
| Diagrama | `#f7f0dd` parchment | — | `#c9a86b` dorado |
| Cases / Notas | `#f5efde` tan | `#6b5e3f` | borde left `#c89aa8` |

---

## Colores de vectores en diagramas SVG

| Vector | Color | Descripción |
|---|---|---|
| **r** (posición) | `#c2622d` | Naranja cálido |
| **F** (fuerza) | `#3a7d44` | Verde hoja |
| **M** (momento) | `#1c1a17` | Negro Van Dyke |
| **d** (brazo de palanca) | `#2d4f72` | Azul |
| Plano / fondo SVG | `#7fa3c9` | Azul claro |
| Pivote / estructura | `#54514a` | Gris oscuro cálido |

---

## Contenido de las cards

- **Base operativa** (operaciones vectoriales básicas, sumas, etc.): NO se resume en card, se anota como ✳ *cosa a saber*
- Formato de contenido por card: **Definición → Fórmula → Demostración → Ejemplo numérico**
- Técnico y preciso — sin paja, pero sin suprimir información importante
- Estilo de ingeniería: usar terminología correcta, incluir unidades SI, nombrar propiedades usadas en demostraciones

---

## Diagramas SVG

- Estilo físico real: palanca con pivote triangular sobre suelo, plano en perspectiva 3D, flechas vectoriales con `<marker>`
- Cada vector con su color identitario (ver tabla arriba)
- Incluir leyenda compacta dentro del panel cuando hay varios vectores
- Ángulos marcados con arco + letra griega en italic (Fraunces)
- Marcador de ángulo recto con pequeño cuadrado

---

## Escalado responsive (JS)

```js
function scaleCard() {
  var scene = document.getElementById('scene');
  var W = 960;
  var H = 678;
  var scale = Math.min(window.innerWidth / W, window.innerHeight / H) * 0.96;
  scene.style.transform = 'scale(' + scale + ')';
  scene.style.position = 'fixed';
  scene.style.top = '0';
  scene.style.left = '0';
  scene.style.marginLeft = ((window.innerWidth - W) / 2) + 'px';
  scene.style.marginTop = ((window.innerHeight - H) / 2) + 'px';
}
scaleCard();
window.addEventListener('resize', scaleCard);
```

---

## Chips de navegación y metadata

- **Top izquierda:** `Física I · Cap X · Tema`
- **Top centro:** pill `FRENTE` (azul) o `DORSO` (sage)
- **Top derecha:** `tema 01` en mono
- **Bottom derecha:** `↻ click para ver el dorso / frente` en italic suave
- Barra de progreso en el header del sitio: `X / total` en mono + barra fill verde

---

## Reglas para extraer y resumir contenido de los PDFs

### Qué incluir
- **Definición formal completa** — no recortar, aunque sea larga. Es lo primero que se evalúa en el coloquio.
- **Todas las fórmulas relevantes** — la vectorial y la escalar si existen ambas.
- **Demostración completa** — paso a paso, sin saltear pasos. Mostrar la propiedad o ley usada en cada paso. No resumir la demo a "se puede demostrar que...".
- **Ejemplo numérico** — siempre con números concretos, unidades, y resultado destacado.
- **Casos límite o importantes** — ej. qué pasa cuando un parámetro es 0 o máximo.
- **Para qué sirve** — una línea de contexto físico real (ej. "permite calcular sin conocer el ángulo exacto").

### Qué NO incluir
- Derivaciones largas que no son parte de la demostración (ej. demostraciones de teoremas auxiliares ya vistos)
- Repeticiones del enunciado en distintas palabras
- Contenido que sea **base operativa** (sumas de vectores, producto escalar básico, etc.) — esos van como ✳ *cosa a saber* en el índice, sin card propia

### Cómo distribuir entre FRENTE y DORSO
- **FRENTE:** lo que tenés que saber decir de entrada — definición, fórmula principal, diagrama conceptual, casos importantes
- **DORSO:** lo que tenés que poder desarrollar si te preguntan — demostración, derivación del módulo, aplicaciones, ejemplo numérico con cuentas

### Nivel de tecnicidad
- Estilo ingeniería: terminología precisa, unidades SI siempre explícitas, propiedades matemáticas nombradas (ej. "por distributividad del producto vectorial")
- No simplificar en exceso — si la demo usa un paso no trivial, explicarlo
- No agregar intuición física que no esté en el apunte original del profesor

---

## Notas generales

- Los bloques del FRENTE deben llenar el alto de su columna — usar `space-between` y padding generoso, no amontonar arriba
- La fórmula principal va en tipografía grande (Fraunces italic ~1.4rem) dentro del bloque azul
- El ejemplo numérico va siempre en el bloque sage verde, con el resultado final destacado con `background` inline
- No usar colores que no estén en la paleta definida sin consultar primero
