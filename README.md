# Superposición Corredor Séptima · Grupo 2

Entrega cartográfica del corredor **Calle 127 – Calle 183** (UE-01 a UE-09), Bogotá D.C.
Sistema de referencia: **MAGNA-SIRGAS / Plano Cartesiano Bogotá D.C. 2005** (unificado, geometría original intacta).

## Contenido

| Carpeta | Descripción |
|---|---|
| `SHP/` | Geodatos limpios en 5 grupos: `01_BASE`, `02_PAVIMENTOS`, `03_ESPACIO_PUBLICO`, `04_REDES_SECAS`, `05_REDES_HUMEDAS` |
| `DWG/` | CAD regenerado desde los mismos datos, en **DXF R2010**. Abrir `CS7_G2_SUPERPOSICION_COMPLETO.dxf` (todo en un archivo, 42 capas organizadas) o los 5 temáticos por separado |
| `VISOR/` | `CS7_G2_VISOR.html` — visor interactivo autónomo |
| `docs/` | Copia del visor para GitHub Pages |

## Formato CAD: DXF (y cómo pasarlo a DWG)

Los archivos CAD se entregan en **DXF R2010**, no en DWG. Motivo: la única librería DWG disponible en este entorno (LibreDWG) tiene un escritor experimental que produjo archivos que AutoCAD no interpretaba — de ahí los planos "en blanco" o con una línea suelta de la versión anterior. El DXF es el formato de intercambio CAD estándar, sin pérdida y verificado entidad por entidad.

**Para obtener el DWG** (10 segundos): abrir el `.dxf` en AutoCAD / Civil 3D → *Guardar como* → *Dibujo de AutoCAD (\*.dwg)*. También se incluye `CONVERTIR_A_DWG.scr` (comando `SCRIPT` en AutoCAD) para hacerlo automático.

Verificación realizada sobre cada archivo: re-lectura completa, 0 entidades fuera del corredor, extents correctos (E 105.013–106.600 / N 111.320–118.425).

| Archivo | Entidades |
|---|---|
| CS7_G2_SUPERPOSICION_COMPLETO.dxf | 40.275 (todo) |
| CS7_G2_00_BASE.dxf | 23 |
| CS7_G2_10_PAVIMENTOS.dxf | 471 |
| CS7_G2_20_ESPACIO_PUBLICO.dxf | 8.157 |
| CS7_G2_30_REDES_SECAS.dxf | 20.982 |
| ├ CS7_G2_31_ENEL.dxf | 3.519 |
| ├ CS7_G2_32_ETB.dxf | 1.821 |
| ├ CS7_G2_33_MOVISTAR.dxf | 414 |
| └ CS7_G2_34_VANTI.dxf | 6.527 |
| CS7_G2_40_REDES_HUMEDAS.dxf | 10.642 |
| ├ CS7_G2_41_ACUEDUCTO.dxf | 2.192 |
| ├ CS7_G2_42_PLUVIAL.dxf | 3.727 |
| ├ CS7_G2_43_SUDS.dxf | 361 |
| └ CS7_G2_44_SANITARIA.dxf | 703 |

Los archivos por componente (31–34, 41–44) traen sus capas **encendidas** para trabajarlos de forma aislada.

Capas de códigos y rótulos (`45-COD-*`, `45-ROT-DUCTERIA`, `20-EP-PAISAJISMO`, `30-RED-VANTI-SINCLAS`) vienen **apagadas** por defecto: se encienden cuando se necesita el detalle.

## Nomenclatura

**Nomenclatura real del proyecto** (campo `NOMBRE`), extraída de los textos de los DWG de origen y asociada a cada elemento por proximidad geométrica:

- Sanitaria: `PNS-####` pozos → **324 de 405** con nomenclatura original.
- Pluvial: `PNP-####` pozos nuevos proyectados · `PMP#####` / `PMI#####` / `PMCI#####` pozos existentes · `SUM#####` sumideros → **2.367 de 3.229 nodos** con su nombre original.
- ETB: nombre original de cámara del proyecto (619).
- Tramos pluviales: `DIAMETRO`, `MATERIAL`, `PENDIENTE` parseados del rótulo del plano (ej. `16.01m-Ø20"-PVC-5.00%`), más el texto íntegro en `DATOS_DWG`.

**Código secuencial** (campo `CODIGO`, sur→norte) como respaldo para los elementos que no traen nombre en el plano: `PZ-###`, `SUM-###`, `CJ-###` cajas Enel, `POS-###` postes, `CT-###` centros de transformación, `MOV-###` cámaras Movistar, `VAL/VEN/ACC-###` accesorios de acueducto.

Ambos visibles en el visor (botón 🏷 Etiquetas, prioriza el nombre real) y en las capas `45-COD-*` del CAD.

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
| **Red_Sanitaria_Colector** | **298** | **DISPLANSAN.dxf (colector sanitario proyectado)** |
| **Red_Sanitaria_Pozos** | **405** | **DISPLANSAN.dxf (pozos proy/exist/inicial/a retirar; 324 con nomenclatura `PNS-####`)** |
| SUDS_Zonas | 361 | LOCALIZACION SUDS_Jul_2026.dwg |

## Pendientes

- Nota PJ: el DWG de pipe jacking trae la mayoría del detalle (plantas de pozos, despieces) dibujado en coordenadas locales cerca del origen (0,0), no georreferenciado; se extrajo todo lo que está en coordenadas MAGNA (trazado, derivaciones, ventilación, 35 pozos de trabajo).
- `XREF EP 2/3.dwg`: secciones con CRC inválido ilegibles fuera de AutoCAD; re-exportar con AUDIT+SAVEAS para integrarlos.
