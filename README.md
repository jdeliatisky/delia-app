# delia-app

## Plataforma de Transcripción y Captura de Cuentos Infantiles

## Contexto
Certamen Nacional de Literatura Infantil y Juvenil (Ciclo escolar 2025–2026, México).

## Objetivo general
Crear una plataforma web interna que permita cargar, transcribir y revisar cuentos manuscritos de niñas y niños de primaria (6 a 12 años), generando un archivo final listo para edición editorial por colegio.

## Roles de usuario
- **Administrador (ADMIN)**: acceso total, crea/edita usuarios, exporta archivos finales, accede a revisiones y validaciones.
- **Embajador / Subgerente**: registra colegios, configura niveles/grados/grupos, sube archivos, descarga lista definitiva de alumnos. No transcribe.
- **Jefa de Captura / Revisión**: revisa transcripciones, confirma nombres/continuaciones, responde preguntas de IA y aprueba cuentos.

## Estructura de la aplicación (pantallas)
1. **Panel principal (Dashboard)**
   - Resumen: colegios activos, cuentos subidos, transcritos, en revisión, archivos rebotados.
   - Accesos rápidos: subir archivos, revisar transcripciones, exportar colegio.
2. **Gestión de usuarios**
   - Crear/editar: nombre, email, contraseña, rol.
   - Activar/desactivar usuarios.
   - Cierre de sesión sin errores de navegación.
3. **Gestión de colegios**
   - Nombre, ciudad, estado, ciclo escolar.
   - Configuración: nivel (PRIMARIA), grados (1° a 6°), grupos (A, B, C… o vacío).
4. **Configuración del colegio**
   - Definir grado, grupo, relación con alumnos y orden final del libro.
5. **Carga de listado oficial de alumnos**
   - Formatos: Excel, imagen, PDF o texto manual.
   - Campos mínimos: nombre completo oficial, grado, grupo.
6. **Centro de carga de archivos (Cuentos)**
   - Subir múltiples imágenes o PDFs multipágina.
   - Seleccionar: colegio, nivel, grado, grupo.
   - Campo opcional: “Este archivo es continuación de: Nombre del alumno”.
7. **Validación de calidad (Rebote)**
   - Antes de OCR: si el archivo está borroso/oscuro/fuera de foco/ilegible ➜ se rebota.
   - No se transcribe y se indica motivo exacto al embajador.

## Reglas críticas de transcripción por IA
### No negociables
- ❌ No resumir, recortar, inventar contenido.
- ❌ No cambiar puntuación ni sintaxis.
- ❌ No separar palabras pegadas.
- ✔️ Solo corregir ortografía.
  - Ejemplos: “unavez” se deja “unavez”, “micasa” se deja “micasa”, “porquenome” se deja igual.

### Título
- Si el niño escribió título → usarlo.
- Si no hay título → usar exactamente: **SIN TITULO**.

### Continuaciones (texto en dos hojas/imágenes)
- Detectar continuidad por similitud de nombre y texto.
- Sugerir: “Parece que este archivo es continuación de: <Nombre – Grado/Grupo> ¿Confirmar?”
- Una vez confirmado: transcribir como un solo cuento continuo, sin separaciones ni “Parte 2”.

## Identificación del alumno (mapeo de nombres)
- Extraer nombre detectado del encabezado.
- Buscar primero en mismo grado y grupo; si no hay match, buscar en todo el colegio.
- Elegir el match con mayor similitud y guardar: `nombre_detectado`, `nombre_oficial`, `student_id`.
- Si similitud < 92% ➜ pedir confirmación humana.

## Dudas y prechequeo inteligente
- Si una palabra es ambigua, la IA **no decide**.
- Marcar como **REQUIERE REVISION** y generar pregunta con opciones.
- No cerrar el texto hasta que se responda.

## Contenido inapropiado o ilegible
- **Lenguaje agresivo/altisonante**: no se edita ni suaviza; reemplazar por cuento nuevo acorde a edad y extensión. Marcar como **REEMPLAZADO_IA**.
- **Escritura ilegible**: reemplazar por cuento IA acorde a edad. Marcar como **IA_ILEGIBLE**.

## Revisión humana
- Confirmar nombres dudosos y continuaciones.
- Resolver ambigüedades.
- Aprobar cuentos con estado final **APROBADO**.

## Exportaciones
1. **Exportar Libro (Word)**
   - Un archivo Word por colegio, sin portada/índice automático/numeración.
   - Orden: por grado y grupo.
   - Cada cuento: nombre completo del alumno, título en negrita, texto completo respetado.
2. **Exportar Lista de Alumnos**
   - Botón: “Descargar lista de alumnos del colegio”.
   - Formato: Excel o CSV, ordenado por grado y grupo.
   - Incluye alumnos con cuento y sin cuento.

## Datos extra a rescatar (si existen)
- colegio, maestra, grado, grupo, edad, ciudad, estado.
- Si no es legible → `NULL`.
- ❌ No inventar.

## Principios fundamentales
- El niño es el autor.
- La plataforma aprende la caligrafía, no la corrige.
- La IA acompaña, no impone.
- Siempre existe validación humana.

## Resultado final esperado
- Plataforma estable.
- Flujo claro y sin errores de navegación.
- Transcripción confiable.
- Libro listo para edición editorial.
- Escalable a otros países e idiomas.
