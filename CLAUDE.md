# CLAUDE.md — Contexto del proyecto webalicia

## Qué es este proyecto

Web estática de presentación para **Alicia Albendín Martínez**, psicóloga colegiada (Nº AN12117) especializada en psicología infantil y juvenil. La web es un landing page de captación orientada a padres de niños con dificultades de aprendizaje (dislexia, TDAH, TEA, dificultades lectoras, problemas emocionales y conductuales).

Replicada a partir de su web original en WordPress (aliciaalbendinpsicologa.com) y rediseñada manteniendo estructura, contenido y colores.

## Stack técnico

| Capa | Tecnología |
|------|-----------|
| Servidor web | nginx:alpine |
| Frontend | HTML5 + CSS3 puro (sin frameworks) |
| JS | Vanilla JS mínimo (acordeón FAQ, scroll reveal, nav sticky) |
| Fuentes | Google Fonts — Playfair Display + Inter |
| Despliegue | Docker + Docker Compose en Raspberry Pi |

Sin backend, sin base de datos. Archivos completamente estáticos.

## Estructura de ficheros

```
webalicia/
├── site/
│   ├── index.html              # Landing page principal (página única larga)
│   ├── pagina-de-ventas.html   # Página de contacto/tarifas para pedir cita
│   ├── aviso-legal.html        # Aviso legal y protección de datos
│   ├── styles.css              # Todo el CSS (tema morado, mobile-first)
│   └── images/
│       ├── alicia.jpg          # Foto principal de Alicia (extraída del .wpress)
│       ├── logo.png            # Logo circular (extraído del .wpress)
│       ├── alicia2.jpg         # Foto alternativa
│       ├── grafico1.png        # Gráficos originales
│       ├── grafico2.png
│       └── grafico3.png
├── docker-compose.yml
└── CLAUDE.md                   # Este fichero
```

## Diseño y colores

| Variable | Hex | Uso |
|----------|-----|-----|
| `--deep` | `#3B2660` | Hero, footer, fondos oscuros |
| `--purple` | `#8B6BAE` | Color principal de marca |
| `--purple-mid` | `#A882C6` | Acentos secundarios |
| `--lilac` | `#D4A8E0` | Detalles, badges |
| `--lilac-soft` | `#EDD6F5` | Fondos suaves |
| `--blush` | `#F7EEF9` | Secciones claras |

Tipografía: **Playfair Display** (display/headings) + **Inter** (cuerpo).

## Secciones del landing (index.html)

1. **Hero** — título principal, foto de Alicia, CTA y badge de colegiada
2. **Stats bar** — 4 cifras (100+ niños, 3+ años experiencia, 60 min sesión, 100% personalizado)
3. **Sobre mí** — foto + texto biográfico y formación
4. **CTA Banner** — llamada a la acción intermedia
5. **¿Te suena esto?** — 5 puntos de dolor de los padres
6. **Lo que me diferencia** — 4 features (calidad, seguridad, contacto, trato)
7. **Servicios** — Terapia con niños / Terapia con padres / Terapia familiar
8. **Testimonios** — 6 testimonios reales de padres y profesores
9. **Tarifas** — Primera sesión 50€ / Sesión individual 70€ / Bono 4 sesiones 220€
10. **Contacto** — Email / WhatsApp / Telegram
11. **CTA Banner** — segunda llamada a la acción
12. **FAQ** — 5 preguntas frecuentes con acordeón
13. **Footer** — con logo, links y copyright

## Datos de contacto (del proyecto real)

| Canal | Valor |
|-------|-------|
| Email | aliciaalbendinpsicologa@gmail.com |
| WhatsApp | +34 672 367 461 |
| Telegram | @aliciaalbendinpsicologa |
| Ubicación | Huelva, Andalucía, España |
| Terapia online | Zoom, Skype, WhatsApp, Discord, Google Meet |

## Despliegue en Raspberry Pi

- **Puerto**: `8096` (mapeado a `80` interno de nginx)
- **URL local**: `http://192.168.0.87:8096`
- **Directorio**: `/home/raspberry/docker/webalicia/`
- **Contenedor**: `webalicia` (nginx:alpine)

### Comandos útiles

```bash
# Reiniciar (solo nginx, no requiere rebuild para cambios en site/)
docker compose -f ~/docker/webalicia/docker-compose.yml restart

# Levantar (primera vez o tras cambiar docker-compose.yml)
docker compose -f ~/docker/webalicia/docker-compose.yml up -d

# Ver logs
docker logs webalicia -f

# Parar
docker compose -f ~/docker/webalicia/docker-compose.yml down
```

> **Importante**: Los ficheros de `site/` están montados como volumen read-only (`./site:/usr/share/nginx/html:ro`). Cualquier cambio en HTML/CSS/JS se refleja **inmediatamente** sin necesidad de rebuild ni restart — basta con recargar el navegador.

## Origen del proyecto

La web fue replicada a partir de un backup `.wpress` (All-in-One WP Migration) de la web original en WordPress con Elementor. El archivo de exportación ya fue eliminado tras extraer el contenido necesario.

## Pendiente / ideas futuras

- Configurar dominio propio con Cloudflare Tunnel
- Añadir formulario de contacto embebido (Calendly, Google Forms, o formulario propio)
- Añadir Google Analytics o similar
- Optimizar imágenes (convertir a WebP)
