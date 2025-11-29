# Errores encontrados y corregidos - Contenedores (Gomez)

## 1. Error en la versión de Docker Compose
**Problema:**  
El archivo tenía `version: "3.8"` pero no era necesario y generaba advertencias.  
**Solución:**  
Se eliminó la directiva y se dejó el formato moderno sin versionado.

---

## 2. Error en la configuración del contenedor nginx
**Problema:**  
El contenedor hacía referencia a `{container="nginx-practica"}` en Promtail, pero ese nombre no existía.  
**Solución:**  
Se corrigió agregando la etiqueta correcta `container_name: nginx-practica` en el servicio.

---

## 3. Ruta incorrecta en volúmenes
**Problema:**  
Algunos volúmenes no coincidían con rutas reales del proyecto.  
**Solución:**  
Se corrigieron las rutas para coincidir con los directorios creados dentro de `contenedores/`.

