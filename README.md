# Compatibilidad en Y — Garrahan 2020

Verificador de compatibilidad de medicamentos administrados en Y.  
Fuente: Farmacia Hospital de Pediatría Garrahan · Versión 2020

**3.131 pares documentados · 93 medicamentos · PWA instalable offline**

---

## Estructura de archivos

```
garrahan-compat/
├── index.html            ← app completa (UI + lógica + 3131 pares de datos)
├── manifest.json         ← metadata PWA (nombre, ícono, colores)
├── service-worker.js     ← caché offline
├── icons/
│   ├── icon-192.png      ← ícono app (reemplazar con logo del hospital)
│   └── icon-512.png      ← ícono app grande
└── README.md             ← este archivo
```

---

## Correr localmente en VS Code

### Opción A — Live Server (más simple)
1. Instalar extensión **Live Server** (Ritwick Dey) en VS Code
2. Click derecho sobre `index.html` → **Open with Live Server**
3. Se abre en `http://127.0.0.1:5500`
4. Cualquier cambio guardado recarga el browser automáticamente

### Opción B — Claude Code
1. Abrir terminal en VS Code (`Ctrl+Ñ`)
2. Navegar a la carpeta: `cd garrahan-compat`
3. Iniciar Claude Code: `claude`
4. Pedirle que levante un servidor: `"levantá un servidor local en el puerto 5500"`

### Testear offline (Chrome)
1. Abrir `http://127.0.0.1:5500` en Chrome
2. DevTools (`F12`) → Application → Service Workers → tildar **Offline**
3. Recargar → debe funcionar sin conexión

---

## Subir a GitHub y activar GitHub Pages

### Paso 1 — Crear repositorio
```bash
# En la terminal de VS Code, dentro de la carpeta garrahan-compat/
git init
git add .
git commit -m "feat: app inicial compatibilidad en Y Garrahan 2020"
```

### Paso 2 — Conectar con GitHub
```bash
# Crear repo en github.com (sin README, sin .gitignore)
# Luego conectar:
git remote add origin https://github.com/TU_USUARIO/garrahan-compat.git
git branch -M main
git push -u origin main
```

### Paso 3 — Activar GitHub Pages
1. Ir al repositorio en github.com
2. **Settings** → **Pages** (menú izquierdo)
3. Source: **Deploy from a branch**
4. Branch: **main** · Folder: **/ (root)**
5. Click **Save**
6. En ~2 minutos la URL queda disponible: `https://TU_USUARIO.github.io/garrahan-compat/`

### Paso 4 — Actualizar cuando haya nueva versión
```bash
git add .
git commit -m "data: actualización planilla 2024"
git push
```
GitHub Pages despliega automáticamente en ~1 minuto.

---

## Actualizar los datos

Los datos están en `index.html`, en el array `const PAIRS`.  
Cada entrada: `["MEDICAMENTO_A", "MEDICAMENTO_B", "RESULTADO"]`

Códigos:
| Código | Significado |
|--------|-------------|
| `C` | Compatible |
| `I` | Incompatible |
| `V` | Variable (verificar concentración y solvente) |
| `U` | No probada |
| `Q` | Incierta |

---

## Íconos para producción

Los íconos actuales son placeholders. Para producción reemplazar con el logo del hospital:
- `icons/icon-192.png` → 192×192 px, fondo transparente o color
- `icons/icon-512.png` → 512×512 px

---

## Notas técnicas

- **Stack**: HTML + CSS + JS vanilla. Sin dependencias, sin build step.
- **Datos fuente**: extraídos automáticamente del PDF oficial Garrahan 2020 por análisis de color de celdas.
- **Conflictos**: 10 pares tenían colores distintos entre triángulo superior e inferior. Se resolvió tomando el resultado más conservador (menos permisivo).
- **PWA**: requiere HTTPS en producción. GitHub Pages y la mayoría de intranets con certificado SSL lo satisfacen.
