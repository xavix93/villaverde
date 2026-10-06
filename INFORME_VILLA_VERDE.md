# Informe de estado y oportunidades de mejora — Villa Verde

**Fecha:** 2 de octubre de 2026  
**Motor:** Godot 4.7.2  
**Tipo de revisión:** lectura del código y los datos del proyecto. No se ejecutó el juego durante esta revisión. Los cambios recientes de regreso al menú, demolición, cuadrícula y expansión requieren validación en Godot.

## 1. Resumen ejecutivo

Villa Verde ya tiene una base funcional para un juego de construcción de ciudades en 2D isométrico: cuadrícula de 20×20, caminos, colocación de edificios, dinero, población, energía, felicidad, ingresos periódicos, experiencia, niveles, guardado local, menú de inicio y gestos táctiles básicos.

El proyecto todavía está en fase de **prototipo jugable**, no en estado de producto móvil terminado. El ciclo principal existe, pero falta darle objetivos y variedad para que el jugador sepa qué hacer después de construir unas pocas casas. También hay que comprobar que la interfaz y las acciones recientes funcionen al ejecutar la última versión.

**Prioridad recomendada:** validar el flujo completo de juego y guardado; después añadir objetivos guiados y mejorar la construcción de caminos. Esas mejoras aumentarán la claridad y el ritmo de juego antes de añadir más edificios o contenido visual.

## 2. Qué incluye actualmente

| Área | Estado observado en el proyecto |
|---|---|
| Inicio y navegación | Menú con continuar, nueva ciudad y salir en escritorio. El HUD tiene un botón Menú que guarda antes de volver al inicio. |
| Mundo | Mapa isométrico de 20×20 parcelas, ampliable hasta 36×36. La cuadrícula aparece durante la construcción. Cámara con teclado, rueda, arrastre y gestos táctiles de arrastre/zoom. |
| Construcción | Catálogo por categorías, vista previa de ubicación, ocupación de varias celdas, requisito de acceso a camino, demolición con reembolso del 50% y compra de ampliaciones. |
| Economía | Saldo inicial de $5.000, costo por construcción, producción periódica y cobro manual. Los edificios almacenan hasta tres pagos. |
| Progresión | Población, energía, felicidad, experiencia, niveles, desbloqueos y recompensas de nivel. |
| Guardado | Archivo JSON local, guardado manual, autosave cada 30 segundos y guardado al volver al menú o cerrar el juego. La cantidad de ampliaciones queda guardada. |
| Contenido | Seis edificios/elementos de construcción más caminos; datos centralizados en `data/buildings.json`. |
| Móvil | Resolución base apaisada 1280×720, expansión de pantalla y manejo táctil básico. Faltan exportación, pruebas en dispositivos y ajustes propios de publicación. |

## 3. Ciclo de juego actual

1. El jugador inicia con $5.000 y 100 de capacidad energética.
2. Construye caminos ($20 por celda) y los conecta con casas o comercios.
3. Las casas aumentan población y consumen energía; los edificios productivos generan dinero con el tiempo.
4. El jugador toca edificios para cobrar, obtiene experiencia y desbloquea contenido.
5. Al ahorrar suficiente, puede comprar hasta cuatro expansiones del terreno, dos celdas por borde en cada compra.
6. Puede guardar, demoler con reembolso parcial o regresar al menú.

El ciclo es comprensible, pero por ahora es principalmente **construir y esperar**. El jugador no recibe encargos, metas cortas, personajes ni consecuencias que cambien sus decisiones. La felicidad se muestra y modifica los ingresos, pero no produce todavía cambios visibles en la vida de la ciudad.

## 4. Economía y progresión: valores actuales

Los tiempos de retorno siguientes son aproximados, antes de considerar el multiplicador de felicidad, el límite de almacenamiento y el costo de caminos:

| Construcción | Costo | Producción | Retorno aproximado | Requisito principal |
|---|---:|---:|---:|---|
| Casa pequeña | $500 | $50 cada 60 s | 10 min | Nivel 1; 5 habitantes |
| Casa familiar | $1.200 | $120 cada 60 s | 10 min | Nivel 2; 5 habitantes |
| Tienda del barrio | $2.000 | $250 cada 60 s | 8 min | Nivel 2; 15 habitantes |
| Granja y mercado | $1.800 | $180 cada 75 s | 12,5 min | Nivel 2; 10 habitantes |
| Parque pequeño | $350 | No produce dinero | Mejora felicidad y XP | Nivel 1 |
| Ayuntamiento | $3.000 | No produce dinero | Aumenta capacidad energética | Nivel 3; 25 habitantes |

La producción acumulada se limita a tres pagos por edificio. Por ejemplo, una casa deja de acumular al llegar a $150 y una tienda a $750 hasta que el jugador cobre. Es un límite razonable para incentivar visitas, pero conviene explicarlo en la interfaz.

El nivel 2 requiere 100 XP y el nivel 3 requiere 250 XP. Construir una casa pequeña otorga 20 XP, y cobrar sus ingresos otorga otros 20. Los caminos dan 1 XP por celda. La felicidad modifica la producción entre 75% y 125% del valor base, según la fórmula actual.

Las ampliaciones cuestan $1.500, $3.000, $6.000 y $10.000, respectivamente. Cada compra aumenta el mapa de 20×20 hasta un máximo de 36×36 y desplaza las coordenadas internas sin mover visualmente la ciudad existente.

## 5. Fortalezas

- La lógica está separada en sistemas de ciudad, cámara, cuadrícula, estado, edificios, guardado e interfaz; eso facilita añadir funciones.
- Los datos de costos y requisitos se editan en JSON sin cambiar el código central.
- La colocación revisa espacio, dinero, energía, nivel y conexión a caminos.
- El guardado almacena el estado principal, los edificios y los caminos.
- La demolición libera el espacio y evita quitar el único camino que da acceso directo a un edificio.
- Se han considerado desde ahora controles táctiles, que son importantes para el objetivo de iOS y Android.

## 6. Principales problemas y riesgos

### Prioridad alta

- **Validación pendiente de los últimos cambios:** el botón Menú, la demolición, el reembolso y los controles táctiles deben probarse dentro de Godot. El proyecto incluye `tests/phase1_smoke.gd`, pero esa prueba no cubre las funciones añadidas más recientemente.
- **Pocas metas para el jugador:** no hay tutorial, objetivos, misiones ni hitos visibles. Después de aprender a construir, la motivación puede caer.
- **Caminos lentos de colocar:** cada celda requiere una acción independiente. Trazar calles largas puede sentirse repetitivo, especialmente en móvil.
- **Ampliaciones aún no probadas en ejecución:** conviene comprobar los cuatro niveles, sus costos, la conservación de edificios y caminos, y la restauración correcta al cargar una partida.

### Prioridad media

- **El guardado no incluye la cámara:** al continuar, la ciudad se carga, pero la posición y el zoom vuelven a los valores iniciales.
- **No hay progreso sin conexión:** las construcciones no generan ingresos mientras el juego está cerrado. El archivo guarda el tiempo restante de producción, pero no calcula el tiempo transcurrido fuera del juego.
- **La felicidad tiene poco impacto:** solo modifica la producción y el indicador, por lo que parques y otras mejoras tienen poco significado jugable.
- **Falta diversidad de contenido:** hay pocos edificios y variantes; las dos casas usan el mismo índice de sprite.
- **La interfaz de juego usa coordenadas fijas:** aunque se expande la pantalla, las barras y las tarjetas no se reorganizan para distintas proporciones. Debe verificarse en teléfonos y tabletas.
- **El formato de guardado no tiene migraciones:** solo acepta la versión actual del archivo. Cambiar la estructura más adelante puede invalidar partidas previas.

### Pendiente para publicación móvil

El proyecto aún necesita exportación y validación en dispositivos reales: plantillas de exportación, identificadores de aplicación, firma de Android, certificados y perfiles de iOS, iconos, área segura, suspensión/reanudación y revisión de rendimiento y memoria.

## 7. Ruta de mejora propuesta

### Fase A — estabilidad y claridad

1. Probar inicio → nueva ciudad → construir → guardar → volver al menú → continuar.
2. Comprobar demolición de casas, parques y caminos, incluidos los caminos que conectan edificios.
3. Revisar el juego en ventana pequeña, pantalla panorámica, tableta y un teléfono real.
4. Comprobar las cuatro ampliaciones y que la cuadrícula solo aparezca durante la construcción.
5. Añadir instrucciones iniciales: colocar un camino, construir dos casas y cobrar el primer ingreso.
6. Mostrar cuánto falta para el siguiente pago y avisar cuando el almacén de ingresos esté lleno.

### Fase B — más decisiones y mejor ritmo

1. Permitir arrastrar para trazar varios tramos de camino en una sola acción.
2. Añadir encargos cortos con recompensas: alcanzar población, construir un parque o ganar cierta cantidad.
3. Dar un efecto visible a la felicidad, por ejemplo bonificación de ingresos claramente explicada o animaciones de vecinos.
4. Revisar costos, tiempos y energía tras observar partidas de 10–15 minutos; evitar que el jugador quede sin dinero sin una opción de recuperación.
5. Guardar/restaurar la cámara y hacer el guardado más resistente a interrupciones o cierres móviles.

### Fase C — contenido y acabado

1. Crear más viviendas, comercios, servicios y decoraciones con arte visual distinto.
2. Añadir animaciones pequeñas, efectos de construcción y cobro, sonido y personajes que den vida al mapa.
3. Añadir expansión del terreno y objetivos de mediano plazo.
4. Preparar exportaciones de prueba y revisar controles, lectura de texto, consumo y pausas en Android/iOS.

## 8. Cómo medir si mejora

En pruebas breves con jugadores, conviene registrar:

- tiempo hasta que entienden cómo colocar el primer edificio;
- tiempo hasta construir la primera casa y cobrar el primer ingreso;
- número de intentos de colocación inválidos;
- cantidad de toques/clics para trazar una calle de diez celdas;
- si entienden por qué un edificio está bloqueado o sin camino;
- saldo de dinero a los 5, 10 y 15 minutos;
- si regresan al juego después de desbloquear el nivel 2.

La mejora más exitosa debería reducir acciones repetitivas y hacer más claro el siguiente objetivo sin quitar la libertad de diseñar la ciudad.

## 9. Conclusión

Villa Verde ya tiene los sistemas esenciales de un constructor de ciudades pequeño y una estructura útil para crecer. La siguiente etapa debería centrarse en terminar de validar la experiencia, guiar los primeros minutos y facilitar la construcción de calles. Después conviene ampliar el catálogo, dar más uso a la felicidad y preparar los procesos reales de exportación móvil.
