# SPEC 01 — MVP Landing Page JMcode

> **Status:** Draft · **Depends on:** Ninguno (primer spec del proyecto) · **Date:** 2026-06-23
> **Objective:** Construir el MVP de la landing page de JMcode (header, hero, servicios, sobre nosotros, contacto vía WhatsApp y footer) con Astro y Tailwind, aplicando la identidad de marca y usando contenido placeholder donde aún no hay contenido real.

## Scope

**In:**

- Scaffold del proyecto con Astro usando la integración oficial de Tailwind y la estructura estándar (`src/pages`, `src/components`, `src/layouts`).
- Header con logo, enlaces de navegación a las secciones (anchor scroll) y CTA hacia contacto.
- Hero con titular, subtítulo y CTA, usando colores y tipografías de marca.
- Sección de Servicios con los 3 servicios (apps, web, sistemas web), cada uno con ícono, descripción corta y ejemplos de proyectos en formato de texto (sin enlaces ni galería).
- Sección Sobre nosotros con texto placeholder y avatares/ilustraciones genéricas como placeholder del equipo (sin fotos reales).
- Sección de Contacto con un formulario funcional (nombre, servicio de interés, mensaje) que al enviarse abre un enlace `wa.me` con el texto prellenado, usando un número de WhatsApp placeholder con código +58.
- Footer con logo, enlaces de navegación, enlaces a redes sociales (placeholder/mock), información de contacto y copyright.
- Configuración del tema de Tailwind con los colores de marca (`#F66136` naranja, `#111111` negro) y las tipografías (Teko Bold como principal, Poppins como complementaria).
- Diseño responsive (mobile, tablet, desktop).
- Metadatos básicos de la página (title, description, favicon) usando el logo de marca.

**Out of scope (for future specs):**

- Despliegue del sitio (solo se entrega `pnpm build` funcionando).
- Versión en inglés / soporte multi-idioma.
- Número de WhatsApp real (se deja un placeholder, se reemplaza después).
- Fotos reales del equipo (se reemplazan después de los placeholders).
- Enlaces reales o galería de ejemplos de proyectos.
- Implementación real de redes sociales (los enlaces quedan como mock).
- Backend propio, CMS o base de datos.
- Analítica (Google Analytics u otro).
- SEO avanzado (sitemap, structured data, Open Graph completo).

## Data model

Esta landing es mayormente estática, pero se centralizan los textos repetibles/listas en archivos de datos dentro de `src/data/` para no hardcodear contenido dentro de los componentes.

```js
// src/data/navigation.js
export const navLinks = [
  { label: "Inicio", href: "#hero" },
  { label: "Servicios", href: "#servicios" },
  { label: "Sobre nosotros", href: "#nosotros" },
  { label: "Contacto", href: "#contacto" },
];

export const socialLinks = [
  { name: "Instagram", href: "#" }, // placeholder, se reemplaza luego
  { name: "Facebook", href: "#" },
  { name: "LinkedIn", href: "#" },
];
```

```js
// src/data/services.js
export const services = [
  {
    icon: "apps", // identificador de ícono, se resuelve en el componente
    title: "Desarrollo de aplicaciones",
    description: "Descripción corta del servicio.",
    examples: ["Ejemplo de proyecto tipo A", "Ejemplo de proyecto tipo B"],
  },
  {
    icon: "web",
    title: "Desarrollo de páginas web",
    description: "Descripción corta del servicio.",
    examples: ["Ejemplo de proyecto tipo A", "Ejemplo de proyecto tipo B"],
  },
  {
    icon: "systems",
    title: "Desarrollo de sistemas web",
    description: "Descripción corta del servicio.",
    examples: ["Ejemplo de proyecto tipo A", "Ejemplo de proyecto tipo B"],
  },
];
```

```js
// src/data/contact.js
export const contact = {
  whatsappNumber: "+584121234567", // placeholder, se reemplaza por el número real
};
```

Convenciones:

- Los textos reales de servicios, "sobre nosotros" y ejemplos de proyectos son placeholders que el usuario reemplaza después; el spec solo fija su forma (campos), no su contenido final.
- El avatar/ilustración del equipo en "Sobre nosotros" no es data-driven: se renderiza como un bloque visual genérico (ej. ícono o iniciales), sin un arreglo de personas.

## Implementation plan

1. Scaffold el proyecto con `pnpm create astro@latest` y agregar la integración oficial de Tailwind (`pnpm astro add tailwind`). Test manual: `pnpm dev` muestra la página default de Astro sin errores.
2. Configurar el tema de Tailwind con los colores de marca (`#F66136`, `#111111`) y las tipografías (Teko Bold, Poppins), cargando las fuentes (Google Fonts o archivos locales). Crear `src/layouts/Layout.astro` con metadatos básicos (title, description, favicon con el logo). Test manual: una página de prueba con un `<h1>` muestra la tipografía y color correctos.
3. Crear `src/components/Header.astro` usando `src/data/navigation.js` para el logo y los enlaces de navegación. Test manual: el header se renderiza con los enlaces funcionando como anchors.
4. Crear `src/components/Hero.astro` con titular, subtítulo y CTA. Test manual: la sección hero se ve con la identidad de marca y el anchor `#hero` funciona.
5. Crear `src/components/Services.astro` que itera sobre `src/data/services.js` mostrando ícono, descripción y ejemplos de cada servicio. Test manual: se ven las 3 tarjetas de servicio con su contenido.
6. Crear `src/components/AboutUs.astro` con el texto placeholder y los bloques visuales genéricos del equipo (íconos/iniciales). Test manual: la sección se renderiza con el placeholder.
7. Crear `src/components/Contact.astro` con el formulario (nombre, servicio de interés, mensaje) y la lógica que construye el enlace `wa.me` usando `src/data/contact.js`, abriéndolo al enviar. Test manual: llenar el formulario y enviarlo abre WhatsApp Web/app con el mensaje prellenado.
8. Crear `src/components/Footer.astro` con logo, enlaces de navegación, `src/data/navigation.js` (socialLinks), contacto y copyright. Test manual: el footer se renderiza completo.
9. Ensamblar todos los componentes en `src/pages/index.astro` dentro del `Layout`, en el orden Header → Hero → Services → AboutUs → Contact → Footer. Test manual: la página completa hace scroll y navega correctamente entre secciones.
10. Ajustar el diseño responsive de todas las secciones (mobile, tablet, desktop) con los breakpoints de Tailwind. Test manual: revisar visualmente en 375px, 768px y 1280px de ancho.
11. Ejecutar `pnpm build` y corregir cualquier error o warning. Test manual: el build termina sin errores y genera los archivos estáticos en `dist/`.

## Acceptance criteria

- [ ] `pnpm dev` levanta el sitio sin errores en consola.
- [ ] `pnpm build` se ejecuta sin errores ni warnings y genera `dist/`.
- [ ] El header muestra el logo y los enlaces de navegación, y cada enlace navega a su sección correspondiente.
- [ ] La sección hero usa los colores de marca (`#F66136`, `#111111`) y las tipografías Teko Bold / Poppins.
- [ ] La sección de servicios muestra los 3 servicios (apps, web, sistemas web), cada uno con ícono, descripción y ejemplos de proyectos.
- [ ] La sección "Sobre nosotros" muestra el texto placeholder y los bloques visuales genéricos del equipo.
- [ ] Al completar y enviar el formulario de contacto, se abre un enlace `wa.me` hacia el número placeholder (+58) con el mensaje prellenado (nombre, servicio de interés, mensaje).
- [ ] El footer muestra logo, enlaces de navegación, enlaces a redes sociales (placeholder), información de contacto y copyright.
- [ ] El sitio se ve correctamente en anchos de 375px, 768px y 1280px sin overflow horizontal ni elementos rotos.
- [ ] La pestaña del navegador muestra el title, description y favicon configurados.

## Decisions

- **Yes:** Astro + Tailwind. Ya definido en `CLAUDE.md`, no se reabre.
- **Yes:** pnpm como gestor de paquetes en vez de npm. Preferencia explícita del usuario.
- **Yes:** Formulario de contacto integra con WhatsApp (`wa.me`) en vez de un backend de email. Simplicidad para el MVP, sin infraestructura de backend.
- **No:** Servicio de formularios tipo Formspree/Web3Forms. Se descartó en favor del enlace directo a WhatsApp.
- **Yes:** Número de WhatsApp placeholder (+58, número aleatorio). El número real aún no está definido; se reemplaza después.
- **Yes:** Landing solo en español para el MVP. Alcance reducido; inglés se evalúa en otro spec si se necesita.
- **Yes:** Placeholders genéricos (avatares/iniciales) en vez de fotos reales del equipo. No se cuenta con fotos reales ni capacidad de generarlas en este entorno.
- **Yes:** Ejemplos de proyectos como texto descriptivo, sin enlaces ni galería. Aún no existen proyectos reales que enlazar.
- **Yes:** Redes sociales como enlaces mock/placeholder en el footer. Las cuentas reales no están definidas todavía.
- **No:** Despliegue (Vercel/Netlify/etc.) en este spec. El usuario lo dejó explícitamente fuera; solo se entrega `pnpm build` funcionando.
- **No:** Analítica, SEO avanzado, CMS o backend propio. No necesario para el MVP.

## Risks

| Riesgo                                                                                   | Mitigación                                                                                            |
| ----------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------ |
| Los colores/tipografías se transcribieron manualmente del PDF (no se pudo leer automáticamente) | Validar visualmente contra el manual de marca antes de aprobar la implementación final.               |
| La fuente Teko Bold puede no estar disponible en todos los pesos vía Google Fonts          | Verificar disponibilidad; si falta, usar `@font-face` con el archivo de fuente local.                  |
| El comportamiento de `wa.me` varía entre escritorio (WhatsApp Web) y móvil (app)            | Es un comportamiento esperado de WhatsApp, no un bug del sitio; se documenta como tal.                 |
| Contenido placeholder (textos, equipo, proyectos, redes sociales) podría quedar sin reemplazar | Dejarlo explícito como pendiente del cliente antes de cualquier despliegue real (fuera de este spec). |

## What is **not** in this spec

- Despliegue del sitio (Vercel, Netlify, GitHub Pages u otro).
- Versión en inglés / soporte multi-idioma.
- Número de WhatsApp real (queda un placeholder con código +58).
- Fotos reales del equipo (quedan placeholders genéricos).
- Enlaces reales o galería de ejemplos de proyectos.
- Cuentas reales de redes sociales (quedan como enlaces mock).
- Backend propio, CMS o base de datos.
- Analítica (Google Analytics u otro).
- SEO avanzado (sitemap, structured data, Open Graph completo).

Cada uno de estos, si se necesita, va en su propio spec.
