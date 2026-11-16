# POLARIS09 WEB v41 — Despliegue estático estable en Vercel

## Qué corrige
La versión v40 dependía de un `app.py` Python para entregar `public/index.html`. En Vercel esa función podía devolver `500 FUNCTION_INVOCATION_FAILED`. La v41 sirve `index.html` como archivo estático desde la raíz, sin función Python. **No se incluye `app.py` ni `requirements.txt` en la raíz.**

## Publicar en GitHub y Vercel
1. Descomprimir el ZIP. Abrir la carpeta `POLARIS09_v41_Vercel_Estable`.
2. En el repositorio `urrutiaefrain/Gee`, **eliminar o mover los antiguos `app.py`, `requirements.txt` y `public/index.html` de la raíz**, para evitar que Vercel detecte el runtime Python anterior. Si existe `api/` con código antiguo, retirarlo del despliegue.
3. Subir **el contenido de la carpeta**, no la carpeta contenedora ni el ZIP. En la raíz de GitHub deben verse `index.html` y `vercel.json`.
4. Vercel > Settings > Build and Deployment: Root Directory `./`, Framework Preset `Other`, Build Command vacío o desactivado, Output Directory vacío/predeterminado. Guardar.
5. Vercel > Deployments > Redeploy y **desactivar Use existing Build Cache**.
6. Abrir la URL de Vercel. La página debe mostrar el centro GIS sin invocar Python.

Si el repositorio conserva archivos antiguos y prefieres actualizarlo con Git desde Windows:
```powershell
git clone https://github.com/urrutiaefrain/Gee.git
cd Gee
git rm -f app.py requirements.txt public/index.html  # solo si existen
# Copia aquí el CONTENIDO de POLARIS09_v41_Vercel_Estable
git add -A
git commit -m "POLARIS09 v41 - despliegue estatico estable"
git push origin main
```

## Limitaciones importantes
- Interfaz, mapa satelital y delimitación de AOI funcionan en navegador con conexión a las teselas y Leaflet.
- **Los análisis GEE, la lectura de Shapefile ZIP, las simulaciones, el render Blender, la IA visual y los PDF todavía no se ejecutan en Vercel.** Sus botones permanecen deshabilitados para evitar resultados ficticios.
- `desktop_v36/` conserva la aplicación original para Windows, pero está excluida del despliegue de Vercel mediante `.vercelignore`.
- `backend_local/` contiene una API de diagnóstico opcional, no conectada a la web pública.
- Para habilitar las funciones completas se requiere un worker GIS/Blender con cola de trabajos, almacenamiento persistente, autenticación y endpoints de análisis.

## Diagnóstico
Si la página falla, verificar que `index.html` esté en la raíz, que `app.py` NO esté en la raíz, que Vercel use Framework `Other` y que el despliegue nuevo no reutilice la caché. El fallo del mapa puede deberse a bloqueo de CDN o conexión a Internet y no a un error de función de Vercel.
