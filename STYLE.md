# Editorial Cálido — Guía de estilo

Sistema visual reutilizable para proyectos web pequeños y de página única. Pensado para experiencias informativas con personalidad: landings, regalos digitales, microsites de producto, dashboards de marca.

Tres pilares que tienen que estar **todos** para que funcione:

1. **Calidez suave** (color y atmósfera)
2. **Jerarquía editorial** (tipografía y espacio)
3. **Interactividad ligera** (micro-respuestas, no micro-animaciones)

Si quitas uno, el estilo se cae.

---

## 1 · Concepto base

- **Densidad sin saturación**. Las páginas en este estilo suelen tener mucho menos contenido del que parece, pero se sienten "completas" porque cada bloque tiene textura propia (gradiente, ring, eyebrow, número grande). Lo plano se evita con **capas de fondo** (gradiente del body + cards translúcidas + cards-en-cards), no con más contenido.
- **Mobile-first siempre**. `max-w-xl` (576px) en stack vertical. La tipografía y el espaciado responden, pero la *estructura* es la misma en todos los tamaños — un solo flujo lineal. Esto es lo que hace que se sienta nativo en móvil sin necesidad de breakpoints complejos.
- **El movimiento es respuesta, no decoración**. Las decoraciones flotantes (nubes, partículas, formas) son la única excepción "atmosférica" tolerada. Todo lo demás (pop del contador, drift al click, scale del botón) **reacciona** a algo. Si hay animaciones que no responden a nada del usuario, mátalas.

---

## 2 · Paleta

### Fondo (gradiente compuesto, fijo)

```css
background:
  radial-gradient(ellipse at 20% -10%, #fef3c7 0%, transparent 55%),
  radial-gradient(ellipse at 90% 10%, #ffe4e6 0%, transparent 50%),
  linear-gradient(180deg, #e0f2fe 0%, #f0f9ff 40%, #fef9c3 100%);
background-attachment: fixed;
```

Receta: dos elipses radiales en esquinas opuestas (acentos cálidos) sobre un linear-gradient base (frío arriba → cálido abajo). El `attachment: fixed` evita que el gradiente "scroll-stretchee" en móvil.

Los tonos exactos pueden cambiarse según el proyecto, pero la estructura "dos radiales + un linear" es la firma del estilo.

### Cards

- Superficie: `bg-white/70` o `/80` + `backdrop-blur` (clave para que el gradiente del fondo se "infiltre" sutilmente en la card)
- Borde: `ring-1 ring-slate-900/5` (5% de opacidad — casi invisible pero define el corte)
- Sombra: `shadow-sm`. Nunca más fuerte.
- Radio: `rounded-2xl` (16px) para cards principales, `rounded-xl` (12px) para inset.

### Texto (jerarquía estricta de slates)

| Uso | Token |
|---|---|
| Headings | `slate-900` |
| Cuerpo principal | `slate-700` |
| Soporte | `slate-600` |
| Eyebrows / metadatos | `slate-500` |
| Footer / sutilezas | `slate-400` |

### Acentos cálidos (úsalos con miedo)

- `amber-600` para números/porcentajes destacados
- `amber-700/80` para eyebrows en cards inset
- Gradientes acento solo en CTAs principales: `from-amber-200 via-amber-300 to-orange-300`

### Estados

| Estado | Color |
|---|---|
| Bien / positivo | `emerald-600` |
| Cuidado | `amber-600` |
| Malo / alerta | `rose-600` |

**Regla central**: el body es cálido difuso. Las cards son neutras (blanco translúcido). El acento cálido vive **dentro** de momentos puntuales (un número, un eyebrow, un CTA). Esa rotación es lo que evita la sobredosis.

---

## 3 · Tipografía

### Pareo display serif + sans

- **Fraunces** (variable, opsz, weights 600/800) para *display*: H1, números grandes, headlines de cards.
- **Inter** (weights 400/500/600) para todo el cuerpo, eyebrows, micro-copy.

Por qué funciona: Fraunces tiene curvas casi caligráficas que aportan humanidad y "carácter editorial". Inter es la sans-serif neutra más confiable. El contraste serif/sans **señala jerarquía sin recurrir a colores**.

### Carga

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Fraunces:opsz,wght@9..144,600;9..144,800&family=Inter:wght@400;500;600&display=swap" rel="stylesheet">
```

### Escala

| Elemento | Clases |
|---|---|
| Hero | `text-5xl sm:text-6xl font-display font-extrabold` |
| Stat principal | `text-6xl font-display font-extrabold` |
| Card heading | `text-2xl font-display font-bold` |
| Body | `text-base` Inter 400/500 |
| Eyebrow | `text-xs uppercase tracking-[.2em]` o `tracking-widest` |
| Microcopy | `text-[11px]` o `text-[10px]` |

### Tabular nums

Cualquier número que cambie en runtime (countdowns, contadores, porcentajes que actualizan): aplica `font-variant-numeric: tabular-nums` (clase utilitaria recomendada: `.num-tabular`). Sin esto los dígitos se mueven al actualizar y mata la sensación de pulido.

---

## 4 · Layout

### Estructura

```html
<main class="mx-auto max-w-xl px-5 py-10 sm:py-14">
  <section>...</section>                <!-- hero -->
  <section class="mt-10">...</section>  <!-- card 1 -->
  <section class="mt-6">...</section>   <!-- card 2 -->
  <section class="mt-8">...</section>   <!-- interactive -->
  <section class="mt-10">...</section>  <!-- big stat -->
</main>
```

### Ritmo vertical asimétrico

`mt-6`, `mt-8`, `mt-10` mezclados a propósito. Uniformizar todo a `mt-8` se siente robótico. La asimetría sutil = humanidad.

### Padding interno de cards

`p-5` o `p-6`. No más, no menos. Más es lujoso pero vacío; menos es asfixiado.

---

## 5 · Cards y sub-cards

### Card principal (translúcida)

```html
<div class="rounded-2xl bg-white/80 backdrop-blur ring-1 ring-slate-900/5 shadow-sm p-5">
  <p class="text-xs uppercase tracking-widest text-slate-500 mb-3">Eyebrow</p>
  <!-- contenido principal -->
</div>
```

### Sub-card / inset (gradiente cálido)

```html
<div class="rounded-xl bg-gradient-to-r from-amber-50 to-rose-50 p-3 text-center">
  <p class="text-[10px] uppercase tracking-widest text-amber-700/80 mb-1">Eyebrow</p>
  <p class="font-display text-lg font-bold text-slate-900">Valor</p>
</div>
```

El patrón "card-en-card" funciona porque crea **tres niveles de superficie** (fondo gradiente → card neutra → sub-card cálida) con muy poca complejidad. Cada nivel es una afirmación visual de jerarquía.

---

## 6 · Eyebrow + Stat (la fórmula recurrente)

```
EYEBROW EN MAYÚSCULAS  ← text-xs uppercase tracking-widest text-slate-500
NÚMERO GIGANTE         ← text-6xl font-display font-extrabold text-slate-900
descripción contextual ← text-slate-600 max-w-sm mx-auto
```

Esta tríada — etiqueta, valor, contexto — es el **átomo de información** del estilo. Usable para cualquier tipo de stat, countdown, momento de "información importante".

---

## 7 · Micro-interacciones (las que sí, las que no)

### Reveal on load

```js
requestAnimationFrame(() => {
  document.querySelectorAll('.reveal').forEach((el, i) => {
    setTimeout(() => el.classList.add('in'), 80 * i);
  });
});
```

```css
.reveal { opacity: 0; transform: translateY(8px); transition: opacity .5s ease, transform .5s ease; }
.reveal.in { opacity: 1; transform: translateY(0); }
```

Cada section entra con stagger de 80ms. La página se *construye* frente al usuario en vez de aparecer monolítica. **80ms es la cifra mágica**: más se siente lento, menos se pierde.

### Pop al actualizar

```css
@keyframes pop {
  0%   { transform: scale(1); }
  40%  { transform: scale(1.25); }
  100% { transform: scale(1); }
}
.pop { animation: pop .35s ease-out; }
```

Aplicar + remover la clase con un `void el.offsetWidth` entre medio (forzar reflow) cada vez que un número actualiza:

```js
el.classList.remove('pop');
void el.offsetWidth;
el.classList.add('pop');
```

Sensación táctil sin frameworks.

### Scale al press

```css
.btn { transition: transform .12s ease; }
.btn:active { transform: scale(.95); }
```

Doce líneas de CSS hacen que cada CTA se sienta **físico**. Nunca lo omitas.

### Drift-away (elemento que se aleja)

```css
@keyframes drift-away {
  0%   { transform: translate(-50%, 0) scale(1); opacity: 1; }
  100% { transform: translate(var(--dx), var(--dy)) scale(2); opacity: 0; }
}
.drift {
  position: absolute;
  animation: drift-away 1.4s cubic-bezier(.4, 0, .2, 1) forwards;
}
```

```js
const span = document.createElement('span');
span.className = 'drift';
span.style.setProperty('--dx', `calc(-50% + ${(Math.random()*2-1)*180}px)`);
span.style.setProperty('--dy', `${-(220 + Math.random()*80)}px`);
container.appendChild(span);
span.addEventListener('animationend', () => span.remove(), { once: true });
```

Genérico para cualquier "consumo": coleccionar, eliminar, descartar.

### Skeleton shimmer

```css
.skeleton {
  background: linear-gradient(90deg, #e0e7ef 0%, #f1f5f9 50%, #e0e7ef 100%);
  background-size: 200% 100%;
  animation: shimmer 1.6s infinite linear;
  border-radius: .5rem;
}
@keyframes shimmer {
  0%   { background-position: -200% 0; }
  100% { background-position: 200% 0; }
}
```

Para estados de carga. Más elegante que un spinner.

### Lo que NO

- Animar entradas en scroll (Intersection Observer abuse)
- Animaciones de loading "para verse pro" (skeleton shimmer es suficiente)
- Easing exagerado (cubic-bezier dramáticos). Quédate en `ease-out`, `ease-in-out`, o el `(.4, 0, .2, 1)` estándar de Material.
- Stagger de más de 5 elementos (después del 5º, todo aparece junto)

---

## 8 · Decoración atmosférica

### Receta de "elementos flotando"

```html
<div aria-hidden="true" class="pointer-events-none fixed inset-0 -z-10 overflow-hidden">
  <div class="float-slow absolute top-12 left-6 text-5xl opacity-40">☁️</div>
  <div class="float-med absolute top-32 right-10 text-3xl opacity-30">☁️</div>
  <div class="float-slow absolute top-72 left-12 text-4xl opacity-25" style="animation-delay:-2s">☁️</div>
</div>
```

```css
@keyframes float {
  0%, 100% { transform: translate(0, 0); }
  50%      { transform: translate(8px, -10px); }
}
.float-slow { animation: float 8s ease-in-out infinite; }
.float-med  { animation: float 5s ease-in-out infinite; }
```

### Reglas

- `pointer-events-none` siempre (no roban clics)
- `-z-10` (detrás de todo)
- Opacidades **bajas**: 25%, 30%, 40%. Nunca >50%.
- **Tres elementos máximo**, tamaños distintos.
- Periodos distintos (5s, 8s) y `animation-delay` negativo para que no estén sincronizados.

Funciona con emojis, SVG, formas CSS puras. Lo importante es la cadencia desincronizada.

---

## 9 · Reduced motion (no opcional)

```css
@media (prefers-reduced-motion: reduce) {
  .float-slow, .float-med, .drift, .pop, .skeleton, .reveal { animation: none; transition: none; }
  .reveal { opacity: 1; transform: none; }
}
```

Toda la atmósfera se apaga, la funcionalidad sigue. Esto es no-negociable; usuarios con sensibilidad vestibular o cognitiva lo necesitan.

---

## 10 · Accesibilidad táctil (móvil)

```css
button {
  touch-action: manipulation;
  -webkit-tap-highlight-color: transparent;
}
```

`touch-action: manipulation` desactiva el doble-tap-to-zoom de iOS Safari y elimina el delay de 300ms en taps. Crítico para botones que se presionan repetidamente. **Pinch-zoom sigue funcionando** (a11y intacta).

Para botones de interacción intensa, añade también:

```css
.btn-tactile {
  user-select: none;
  -webkit-user-select: none;
}
```

Evita que un long-press dispare la selección de texto y rompa el flow.

---

## 11 · Stack técnico recomendado

- **Tailwind CSS** (compilado localmente, no CDN) — la velocidad de iteración con utilidades es lo que hace este estilo viable solo
- **Google Fonts**: Fraunces + Inter (preconnect + display=swap)
- **Vanilla JS** para micro-interacciones — frameworks son overkill para esto
- **Sin librerías de animación** (Framer Motion, GSAP) — todo se hace con CSS keyframes y class toggles

### Configuración mínima de Tailwind

```js
// tailwind.config.js
module.exports = {
  content: ['./index.html'],
  theme: {
    extend: {
      fontFamily: {
        display: ['Fraunces', 'serif'],
        sans: ['Inter', 'system-ui', 'sans-serif'],
      },
    },
  },
};
```

---

## 12 · Antipatrones que rompen el estilo

| Mal | Por qué |
|---|---|
| Cards con `bg-white` sólido | Anula la profundidad del gradiente del body. Siempre opacidad <100. |
| Sombras `shadow-lg` o más | El estilo es plano-con-profundidad-sutil. Sombras grandes = bootstrap circa 2015. |
| Bordes `border-2 border-slate-300` | Usa `ring-1 ring-slate-900/5`. Más sutil, mejor jerarquía. |
| Más de 2 colores acento | Amber + slate. Si necesitas un tercero (rose, sky, emerald), úsalo solo en estados, no decoración. |
| Solo sans-serif | Pierdes la diferenciación editorial. Fraunces es el alma del estilo. |
| Glassmorphism intenso (blur grande, sat alta) | El `backdrop-blur` por defecto de Tailwind ya basta. |
| Animaciones >500ms | Se sienten lentas. Mantén entre 120ms y 400ms. |
| Texto centrado en cuerpos largos | Solo centrar cosas atómicas (stats, eyebrows). Párrafos a la izquierda. |
| Botones rectangulares sin radio | Mínimo `rounded-xl`. Para CTAs principales: `rounded-full`. |
| Iconos de librería heterogéneos | Si usas iconos, **un solo set** (Lucide, Heroicons, Phosphor). Mezclar destruye la coherencia. |

---

## 13 · Cómo arrancar un proyecto nuevo en este estilo

1. Copia los keyframes y reglas atmosféricas a `base.css` (o equivalente).
2. Configura Tailwind con `Fraunces` + `Inter` en `theme.fontFamily`.
3. Carga las fuentes con preconnect.
4. Empieza el HTML con: gradiente del body + `max-w-xl` stack + un hero con la tríada eyebrow + H1 + sub.
5. Cada bloque de info nuevo va como **card** o como **eyebrow + stat**.
6. Añade UNA decoración atmosférica (3 elementos flotando, opacidad baja).
7. Para CADA acción del usuario, define la respuesta visual antes que la función. *"¿Qué siente?"* antes que *"¿qué hace?"*.
8. Antes de shippear, prueba `prefers-reduced-motion: reduce` en DevTools y verifica que la página sigue siendo usable.

---

## 14 · Checklist pre-deploy

- [ ] Fuentes cargan con `display=swap` y preconnect
- [ ] `touch-action: manipulation` en todos los botones
- [ ] `prefers-reduced-motion` cubierto
- [ ] Números cambiantes con `tabular-nums`
- [ ] OG image 1200×630 con la misma paleta y tipografía
- [ ] Probado en iPhone Safari y Android Chrome (no solo Chrome desktop)
- [ ] Sin sombras grandes ni bordes gruesos
- [ ] Máximo 3 elementos atmosféricos flotando
- [ ] Tailwind compilado localmente, no CDN
- [ ] Easing en animaciones: `ease-out` o `cubic-bezier(.4, 0, .2, 1)`
