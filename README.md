# BITMAP COP — Alpha 1.0

Juego web experimental inspirado en el combate isométrico de mechas retro. Proyecto original, sin recursos del videojuego Future Cop: L.A.P.D.

## Inicio

En el directorio del proyecto, ejecutar `python -m http.server 8080` y abrir `http://localhost:8080` en un navegador moderno. Se requiere Internet para cargar Three.js desde esm.sh y para consultar la API pública de Blockstream.

## Controles

WASD/flechas: mover. Ratón: apuntar. Clic/espacio: disparar. T: transformar robot/vehículo. R: recargar bloque y misión.

## Datos y reproducibilidad

1. `publicApi.fetchBlock(height)` consulta Blockstream Esplora: hash del bloque, metadatos y lista **completa** de TXID paginada.
2. `derive(data)` asigna parcelas a una cuadrícula 15×15 usando bytes de cada TXID y deduplicación de celdas; alturas y tamaños derivan de otros bytes. El hash del bloque alimenta una PRNG determinística para posiciones de drones.
3. La geometría es una **interpretación experimental** de transacciones Bitcoin, **no** el estándar geométrico oficial Bitmap.
4. La versión del algoritmo debe mantenerse fija para reproducir mundos. Esta es `experimental-txid-grid-v1`.
5. La ciudad solo se genera después de descargar y verificar formato y cantidad de TXID; no hay datos inventados de respaldo.
6. Se impone un límite de 10 000 transacciones por bloque para este MVP. Algunos bloques grandes no cargarán.
7. La simulación de enemigos y movimiento depende del tiempo de ejecución y de las acciones del jugador, pero la ciudad y posiciones iniciales de enemigos son determinísticas.

## Gateway

`adapters.gateway` es un **contrato placeholder**: aún no se conecta a Gateway. Su implementación debe devolver `{height,hash,txids,source,complete}` con datos verificables. No se presume protocolo ni endpoint de Gateway. `adapters.publicApi` puede sustituirse sin modificar `derive()` ni el motor de juego.

## Limitaciones

- Requiere CORS y disponibilidad de servicios externos.
- No valida criptográficamente el consenso Bitcoin ni prueba de trabajo: confía en la API elegida.
- No reproduce la geometría oficial de Bitmap.
- No incluye sonido, guardado, misiones complejas ni multijugador.
- No se han ejecutado pruebas automatizadas de navegador en este paquete.

## Comprobaciones de esta revisión

Se verificaron mediante Node.js: sintaxis del módulo, paginación de 51 TXID simulados, reproducibilidad de la geometría con datos idénticos, rechazo de datos incompletos e inválidos, y rechazo de TXID duplicados. Se comprobó también la integridad del ZIP. **No se ha verificado el juego completo en un navegador**: el entorno de pruebas bloquea conexiones HTTP locales y acceso a los servicios externos. No considerar este paquete aprobado para producción hasta realizar una prueba real de carga, movimiento, transformación y combate.

## Pasos para Windows

1. Descomprime el ZIP en una carpeta `bitmap-cop`.
2. Abre la carpeta y escribe `cmd` en la barra de dirección del Explorador de archivos; pulsa Enter.
3. Ejecuta `py -m http.server 8080` (o `python -m http.server 8080` si `py` no funciona). Necesitas Python instalado.
4. Abre Chrome o Edge en `http://localhost:8080`.
5. Escribe `800000.bitmap` y pulsa EXPLORAR. Espera a que se descarguen todas las transacciones; puede tardar por la paginación.
6. Prueba WASD, clic para disparar y T para transformar. Para detener el servidor, vuelve a CMD y pulsa Ctrl+C.
