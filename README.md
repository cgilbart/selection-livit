# Selección Livit

Plataforma interna de gestión de procesos de selección para Livit Design.

---

## Estructura del proyecto

```
seleccion-livit/
│
├── admin/
│   └── dashboard.html        ← Panel de administración (Lucía)
│
├── candidato/
│   └── index.html            ← Vista del candidato (acceso por magic link)
│
├── assets/                   ← (reservado para futuros recursos compartidos)
│
└── README.md                 ← Este archivo
```

---

## Cómo funciona

### Panel de admin (`admin/dashboard.html`)
Lucía accede directamente a esta URL desde el navegador.
Desde aquí puede:
- Crear candidatos y generar su magic link de acceso
- Ver el estado de cada candidato en tiempo real
- Subir el CV desde LinkedIn
- Consultar los resultados del test DISC con puntuaciones y perfil
- Asignar pruebas técnicas desde la biblioteca
- Ver las entregas de los candidatos
- Cambiar el estado del proceso y añadir notas internas
- Gestionar vacantes

### Vista del candidato (`candidato/index.html`)
El candidato accede mediante un enlace único con su token:
```
https://seleccion-livit.netlify.app/candidato/?token=XXXX-XXXX-XXXX
```
El flujo es:
1. Ve el vídeo corporativo de Livit
2. Confirma su email para verificar su identidad
3. Completa el cuestionario conductual DISC (24 preguntas)
4. Accede a su dashboard con el estado de su proceso
5. Cuando Lucía le asigna una prueba técnica, puede descargarla y subir su entrega

---

## Base de datos (Supabase)

Proyecto: `tdotlurmytvcdgmlmecc`

### Tablas
| Tabla | Descripción |
|-------|-------------|
| `candidatos` | Perfil completo, estado actual y magic token de acceso |
| `vacantes` | Ofertas abiertas y cerradas |
| `resultados_disc` | Puntuaciones D/I/S/C, perfil y informe de cada candidato |
| `preguntas_personalidad` | Preguntas adicionales por vacante (v2) |
| `respuestas_personalidad` | Respuestas de cada candidato |
| `biblioteca_pruebas` | Pruebas técnicas disponibles para asignar |
| `pruebas_tecnicas` | Asignación de prueba a candidato + entrega |
| `historial_estados` | Registro de todos los cambios de estado con timestamp |

### Storage (buckets)
| Bucket | Contenido |
|--------|-----------|
| `cvs` | CVs subidos por Lucía (privado) |
| `pruebas` | Enunciados y entregas de pruebas técnicas (privado) |
| `media` | Vídeo corporativo de bienvenida (público) |

---

## Estados del proceso

```
preseleccionado → perfil_incompleto → tests_completados
→ prueba_asignada → prueba_entregada → entrevista_tecnica
→ oferta / descartado
```

Los cambios automáticos (sin intervención de Lucía):
- El candidato completa el DISC → `tests_completados`
- Lucía asigna una prueba → `prueba_asignada`
- El candidato sube su entrega → `prueba_entregada`

Los cambios manuales (Lucía desde el panel):
- `entrevista_tecnica`, `oferta`, `descartado`

---

## Despliegue

- **Repositorio:** github.com/cgilbar/seleccion-livit
- **Hosting:** Netlify (despliega automáticamente con cada push a `main`)
- **Base de datos:** Supabase
- **Protección:** El site de Netlify tiene contraseña — solo acceso interno

---

## Pendiente (v2)

- [ ] Sistema de envío automático de emails con el magic link
- [ ] Informe DISC completo en PDF descargable
- [ ] Preguntas de personalidad por vacante
- [ ] Notificaciones automáticas al candidato al cambiar de estado
- [ ] Autenticación real para el panel de admin (ahora es acceso directo por URL)

