# Superposición Corredor Séptima · Grupo 2

Entrega cartográfica del corredor **Calle 127 – Calle 183** (UE-01 a UE-09), Bogotá D.C.
Sistema de referencia: **MAGNA-SIRGAS / Plano Cartesiano Bogotá D.C. 2005** (unificado, geometría original intacta).

## Contenido

| Carpeta | Descripción |
|---|---|
| `SHP/` | Geodatos limpios en 5 grupos: `01_BASE`, `02_PAVIMENTOS`, `03_ESPACIO_PUBLICO`, `04_REDES_SECAS`, `05_REDES_HUMEDAS` |
| `DWG/` | CAD regenerado desde los mismos datos. Abrir `CS7_G2_SUPERPOSICION_MAESTRO.dwg` (ensambla 5 XREF temáticos) |
| `VISOR/` | `CS7_G2_VISOR.html` — visor interactivo autónomo |
| `docs/` | Copia del visor para GitHub Pages |

## Visor interactivo

Doble clic en `VISOR/CS7_G2_VISOR.html` (o publicar `docs/` en GitHub Pages: Settings → Pages → main → /docs).

Funciones: mapa base satelital/claro/OSM · capas por grupo con color editable y **control de transparencia por capa (botón ◐)** · filtro Existente / Proyectado / A retirar · cámaras y cajas a escala real (zoom ≥ 18) · popups con ductería y atributos completos · **módulo de avance de obra**: modo avance → clic en elemento → % ejecutado (se pinta verde/naranja y persiste en el equipo) · carga de cronograma (xlsx/csv: Actividad, Inicio, Fin, % Programado) con semáforo al día/atrasado · exportar/importar avance en JSON.

Optimización v3: las capas se construyen solo al encenderlas (carga perezosa) y cada capa es un único objeto GeoJSON — el arranque en web es mucho más rápido.

## Redes secas — esquema normalizado

`OPERADOR · ESTADO · TIPO · DUCTERIA · CAM_INI · CAM_FIN · COSTADO · LONG_M`

| Capa | Elementos | Fuente |
|---|---|---|
| Red_Enel_Tramos / Nodos | 1.682 / 1.837 | SHP + XREF_REDES_PROYECTADAS_ENEL.dwg |
| Red_ETB_Tramos / Camaras | 1.202 / 619 | SHP ETB (cámaras con dimensiones reales del bloque 3D: T-13 2,75×2,05 · T-14 2,78×2,44 · T-16 3,06×3,06 · T-18 2,40×2,40 m) |
| Red_Movistar_Tramos / Camaras | 264 / 150 | SHP + XREF REDES MV_PRO.dwg (CS-275/276/280) |
| Red_Vanti_Tramos | 6.527 | SHP Vanti (material/diámetro/estado desde capa CAD) |
| Redes_Consolidado | 9.675 | Unión de todos los tramos (vista gerencial) |

Notas: dimensiones de cajas Codensa y CS Movistar son representativas (1,2/1,0/1,4/1,7 m) — los bloques origen no traen cota fiable; los círculos Ø0,5 m del DWG Movistar (cada ~6 m) son marcadores de ducto y se excluyeron de cámaras. Capas DWG `40-ROTULO-DUCTERIA` (apagada por defecto) rotula la ductería de cada tramo.

## Redes húmedas — `05_REDES_HUMEDAS` / `CS7_G2_40_REDES_HUMEDAS.dwg`

`SISTEMA · ESTADO · TIPO · LONG_M`

| Capa | Elementos | Fuente |
|---|---|---|
| Red_Acueducto_Matriz_Tramos | 140 | PJ_BASE.dwg (red matriz 24" pipe jacking, incl. capa PROYECCION, derivaciones y ventilación) |
| PJ_Pozos_Trabajo | 35 | PJ_BASE.dwg (pozos de lanzamiento y salida, polígonos a escala) |
| Red_Acueducto_Menor_Tramos | 1.992 | DISREDMENOR-1.dwg (red menor + domiciliarias, exist/proy) |
| Red_Acueducto_Nodos | 25 | PJ_BASE.dwg (válvulas, ventosas, purgas, macromedidor, pozos PJ) |
| Red_Pluvial_Tramos | 492 | XREF-DISPLANPLU.dwg (red nueva/existente/a retirar/colector) |
| Red_Pluvial_Interceptor | 6 | LOCALIZACION SUDS (interceptor proyectado) |
| Red_Pluvial_Nodos | 3.229 | XREF-DISPLANPLU.dwg (pozos y sumideros, proy/exist/retirado/colmatado) |
| SUDS_Zonas | 361 | LOCALIZACION SUDS_Jul_2026.dwg |

## Pendientes

- **Sanitaria**: `DISPLANSAN.dwg` está dañado en origen (102,3 MB reales vs 104,1 MB referenciados internamente; se re-sincronizó y persiste). Abrirlo en AutoCAD (que repara al abrir) y hacer `SAVEAS`, o re-copiar desde la fuente. Con el archivo sano se integra igual que la pluvial.
- Nota PJ: el DWG de pipe jacking trae la mayoría del detalle (plantas de pozos, despieces) dibujado en coordenadas locales cerca del origen (0,0), no georreferenciado; se extrajo todo lo que está en coordenadas MAGNA (trazado, derivaciones, ventilación, 35 pozos de trabajo).
- `XREF EP 2/3.dwg`: secciones con CRC inválido ilegibles fuera de AutoCAD; re-exportar con AUDIT+SAVEAS para integrarlos.
