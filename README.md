# Gestión de Proyectos

Herramientas del curso de Project Management (Máster en Project Management, EGCI Escuela de Gerencia) de Valeria Yashan, PMP®, alineadas con PMBOK® 8. El repositorio reúne el Planificador TI, que arma y controla el cronograma y los costos de un proyecto, y el dashboard de Power BI que lee lo que el planificador exporta.

Versión en vivo del planificador: https://valeriayashan.com.ar/herramientas/planificador-ti/

## Qué hay en el repositorio

| Carpeta | Contenido |
|---|---|
| `planificador-ti/app/` | Planificador TI, versión pública (sin los casos de EGCI). |
| `planificador-ti/alumnos-d0f4dda780e1/` | Versión para alumnos, con los cinco casos del Trabajo Integrador. |
| `dashboard/` | Dashboard de Power BI (`Dashboard-Reporte_de_Proyecto.pbix`) y su captura de ejemplo en PDF. |
| `scripts/` | Calibración de costos contra el presupuesto del acta y armado de los casos públicos. |

## Planificador TI

Es una aplicación estática en JavaScript, sin compilación ni servidor. Los datos se guardan en el navegador de cada usuario, con autoguardado y copia de seguridad en un único archivo JSON.

Funciones principales de ambas versiones:

- Cronograma con dependencias FC, CC, FF y CF, ruta crítica, holguras y calendario con feriados.
- Línea base con superposición, inspector de fechas, tareas inactivas y escenarios.
- Valor ganado (EVM): PV, EV, AC, BAC, CPI, SPI, EAC con tres métodos, ETC, TCPI, curva S y cortes de seguimiento.
- Acta de constitución con reserva de contingencia y de gestión (BAC igual a la línea base; presupuesto del proyecto igual a BAC más reserva de gestión).
- Trazabilidad, registro real de tiempos, reglas de alerta, informe de estado, PERT y simulación, problemas y acciones.
- Tablero con semáforo y exportación a PDF, vista de portafolio de cinco casos y modo docente.
- Proyectos propios en blanco o importados desde Excel o CSV, hasta diez guardados en el navegador, con visita guiada.
- Botón «Exportar para Power BI».

Las dos versiones tienen exactamente la misma funcionalidad. Solo la de alumnos incluye los casos de EGCI, las reservas con valor monetario esperado y el control de cambios.

### Uso local

Servir la carpeta `planificador-ti/` con cualquier servidor de archivos y abrir el `index.html` de la versión que se quiera:

```
cd planificador-ti
python3 -m http.server 8000
# http://localhost:8000/app/index.html
# http://localhost:8000/alumnos-d0f4dda780e1/index.html
```

La importación de Excel usa SheetJS desde un CDN, por lo que requiere conexión a internet.

## Dashboard de Power BI

El archivo `dashboard/Dashboard-Reporte_de_Proyecto.pbix` se abre con Power BI Desktop. Tiene cuatro páginas:

1. Resumen ejecutivo: tarjetas con BAC, PV, EV, AC, CPI, SPI, CV, SV, EAC, ETC, VAC y TCPI; medidor de avance planificado contra avance real; y columnas comparativas de PV, EV y AC.
2. Costos y curva S: presupuesto y valor ganado por fase, curva S acumulada con EV y AC en cada corte, y evolución de CPI y SPI por corte.
3. Cronograma: tabla de tareas con EDT, fase, fechas, avance, holgura, desvío del fin y estado de ruta crítica, con filtros por fase y por estado de ruta, y tareas críticas por fase.
4. Riesgos y control: tabla de riesgos con categoría, probabilidad, impacto, puntaje y responsable, tarjetas de riesgos abiertos y altos (puntaje mayor a 4) y matriz de probabilidad contra impacto.

El modelo se alimenta de tablas planas (Proyecto, Tareas, CurvaS, CortesEVM, Calendario y Riesgos) que el planificador exporta con las claves `ProyectoID` y `TareaKey`. Power BI solo lee lo que el planificador ya calculó y no recalcula el cronograma, de modo que los números del informe coinciden con los de la herramienta. Las medidas se definen en la tabla `Medidas` y usan las siglas PV, EV y AC.

### Cómo actualizarlo

1. En el planificador, registrar un corte antes de cada exportación; sin cortes no hay evolución de CPI y SPI.
2. Usar «Exportar para Power BI» y guardar los archivos resultantes.
3. En Power BI Desktop, apuntar las consultas a los archivos exportados y actualizar.

El archivo `dashboard/informe-ejemplo-power-bi.pdf` es una captura fija con datos ficticios del proyecto «vacaenergia». No se actualiza solo y no está publicado en la web de Power BI.

## Scripts

- `scripts/calibrar-costos.js`: ajusta los costos diarios de los casos para que el BAC coincida con el presupuesto del acta de constitución.
- `scripts/build-casos-publicos.js`: genera los casos de ejemplo de la versión pública.

Se ejecutan con Node.js desde la raíz del repositorio original del sitio, donde viven los datos que leen.

## Criterios de la herramienta

- Los feriados se verifican contra el Boletín Oficial, no de memoria.
- En el valor ganado, la fecha de corte debe caer dentro del rango del proyecto y el BAC se calcula con las duraciones de la línea base.
- Un caso sin avance guardado se muestra como «sin dato», no como semáforo.

## Licencia

Todos los derechos reservados © Valeria Yashan. Material de uso docente; no se autoriza su reutilización o redistribución sin permiso.
