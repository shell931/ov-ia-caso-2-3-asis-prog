# 🌐 Web de Resultados - Deploy

Sitio web con los resultados visuales de las pruebas de modelos coresidentes en AWS g7e.

## 📁 Contenido

```
web-resultados/
├── index.html              ← Página principal
├── a-32b.html             ← Escenario A: Dos modelos grandes
├── b-7b.html              ← Escenario B: Dos modelos 7B
├── c-32b-solo.html        ← Escenario C: Un modelo 32B
├── d-7b-32b.html          ← Escenario D: 7B + coder 32B
├── resumen-32b.html       ← Resumen ejecutivo
├── 30-concurrencias.html  ← Guía de concurrencias
├── metodo.html            ← Metodología
├── politica.html          ← Política de uso
├── estilos.css            ← Estilos (Roboto Mono)
└── .nojekyll              ← Para GitHub Pages
```

## 🚀 Opciones de Deploy

### **Opción 1: GitHub Pages** (RECOMENDADO)

```bash
# 1. Asegúrate de que el repo es PÚBLICO
# En GitHub: Settings → Change visibility → Make public

# 2. Habilitar GitHub Pages
# Settings → Pages → Source: Deploy from a branch
# Branch: main, Folder: /web-resultados

# 3. Esperar ~2 minutos

# URL: https://shell931.github.io/ov-ia-caso-2-3-asis-prog/web-resultados/
```

**Nota**: Si el repo es privado, GitHub Pages requiere plan Pro.

---

### **Opción 2: Cloudflare Pages** (Gratis + Rápido)

```bash
# 1. Ve a: https://dash.cloudflare.com/
# 2. Pages → Create a project → Connect to Git
# 3. Selecciona el repo: ov-ia-caso-2-3-asis-prog
# 4. Build settings:
#    - Framework: None
#    - Build command: (vacío)
#    - Build output: /web-resultados
# 5. Deploy

# URL: https://ov-ia-caso-2-3-asis-prog.pages.dev
```

**Ventajas:**
- ✅ Gratis ilimitado
- ✅ CDN global
- ✅ SSL automático
- ✅ Preview deploys

---

### **Opción 3: Vercel** (Gratis + Fácil)

```bash
# Desde terminal:
cd ~/Documents/APPS/overthere/ov-ia-caso-2-3-asis-prog
npm install -g vercel

# Deploy
vercel --prod

# O desde web:
# https://vercel.com/new
# → Import Git Repository
# → Root Directory: web-resultados
```

---

### **Opción 4: Netlify** (Drag & Drop)

```bash
# 1. Ve a: https://app.netlify.com/drop
# 2. Arrastra la carpeta web-resultados/
# 3. ¡Listo!

# URL: https://random-name-12345.netlify.app
```

---

### **Opción 5: OpenPouch / ShipStatic** (Temporal)

**OpenPouch:**
```bash
# Instalar
npm install -g @openpouch/cli

# Deploy
cd web-resultados/
openpouch deploy .

# URL: https://docs-xxxxx.openpouch.sh
```

**ShipStatic:**
```bash
# https://shipstatic.com
# Drag & drop la carpeta
# URL: https://glowing-flare-xxxxx.shipstatic.com
```

⚠️ **Nota**: URLs temporales (expiran en días/semanas)

---

## 🔧 Deploy Local (Testing)

```bash
cd web-resultados/

# Python 3
python3 -m http.server 8080

# Node.js
npx http-server -p 8080

# PHP
php -S localhost:8080

# Abrir: http://localhost:8080
```

---

## 📦 Deploy con Docker

```dockerfile
# Dockerfile
FROM nginx:alpine
COPY web-resultados/ /usr/share/nginx/html/
EXPOSE 80
```

```bash
# Build y run
docker build -t resultados-web .
docker run -d -p 8080:80 resultados-web

# Abrir: http://localhost:8080
```

---

## 🎨 Personalización

### **Cambiar Colores**

Edita `estilos.css`:

```css
:root {
  --color-primario: #your-color;
  --color-acento: #your-accent;
}
```

### **Cambiar Tipografía**

La web usa **Roboto Mono** (Google Fonts). Para cambiar:

```css
/* En estilos.css */
@import url('https://fonts.googleapis.com/css2?family=Tu+Fuente&display=swap');

body {
  font-family: 'Tu Fuente', monospace;
}
```

---

## 🔗 URLs Históricas

| Servicio | URL | Estado |
|----------|-----|--------|
| OpenPouch | https://docs-p0m8un.openpouch.sh | ❌ Expirado (Sep 11) |
| ShipStatic | https://glowing-flare-y49yqm3.shipstatic.com | ❌ Expirado |
| GitHub Pages | https://shell931.github.io/ia-analisis/ | ❌ Repo privado |

---

## ✅ Checklist Pre-Deploy

- [ ] Verificar que todos los links funcionan
- [ ] Probar en local primero
- [ ] Revisar que estilos.css cargue correctamente
- [ ] Verificar `.nojekyll` está presente (para GitHub Pages)
- [ ] Testear en móvil/tablet
- [ ] Validar HTML (https://validator.w3.org/)

---

## 🆘 Troubleshooting

### **Estilos no cargan**

```html
<!-- Verificar ruta en HTML -->
<link rel="stylesheet" href="estilos.css">
<!-- NO: href="/estilos.css" -->
```

### **GitHub Pages 404**

```bash
# Asegúrate de que .nojekyll existe
touch .nojekyll
git add .nojekyll
git commit -m "fix: Add .nojekyll for GitHub Pages"
git push
```

### **Fonts no cargan**

```css
/* Verificar en estilos.css */
@import url('https://fonts.googleapis.com/css2?family=Roboto+Mono:wght@400;700&display=swap');
```

---

**Última actualización**: Sep 17, 2026  
**Tipografía**: Roboto Mono (Google Fonts)  
**Compatibilidad**: Todos los navegadores modernos
