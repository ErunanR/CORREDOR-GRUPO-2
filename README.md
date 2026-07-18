# Superposición Corredor Séptima · Grupo 2

Entrega cartográfica del corredor **Calle 127 – Calle 183** (UE-01 a UE-09), Bogotá D.C.
Sistema de referencia: **MAGNA-SIRGAS / Plano Cartesiano Bogotá D.C. 2005** (unificado, geometría original intacta).

## Contenido

| Carpeta | Descripción |
|---|---|
| `SHP/` | Geodatos limpios en 5 grupos: `01_BASE`, `02_PAVIMENTOS`, `03_ESPACIO_PUBLICO`, `04_REDES_SECAS`, `05_REDES_HUMEDAS` (pendiente insumos) |
| `DWG/` | CAD regenerado desde los mismos datos. Abrir `CS7_G2_SUPERPOSICION_MAESTRO.dwg` (ensambla 4 XREF temáticos) |
| `VISOR/` | `CS7_G2_VISOR.html` — visor interactivo autónomo |
| `docs/` | Copia del visor para GitHub Pages |

## Visor interactivo

Doble clic en `VISOR/CS7_G2_VISOR.html` (o publicar `docs/` en GitHub Pages: Settings → Pages → main → /docs).

Funciones: mapa base satelital/claro/OSM · capas por grupo con color editable · filtro Existente / Proyectado / A retirar · cámaras y cajas a escala real (zoom ≥ 18) · popups con ductería y atributos completos · **módulo de avance de obra**: modo avance → clic en elemento → % ejecutado (se pinta verde/naranja y persiste en el equipo) · carga de cronograma (xlsx/csv: Actividad, Inicio, Fin, % Programado) con semáforo al día/atrasado · exportar/importar avance en JSON.

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

## Pendientes

- Redes húmedas (acueducto/alcantarillado): estructura reservada en `SHP/05_REDES_HUMEDAS/`.
- `XREF EP 2/3.dwg`: secciones con CRC inválido ilegibles fuera de AutoCAD; re-exportar con AUDIT+SAVEAS para integrarlos.
