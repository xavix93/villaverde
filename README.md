# Villa Verde

City builder isométrico original desarrollado en Godot 4.7.2. El proyecto conserva el arte propio del terreno y edificios, con una estructura preparada para añadir más contenido sin concentrar toda la lógica en una escena.

## Ejecutar

Abre `project.godot` en Godot y ejecuta la escena principal con **F5**. Aparecerá el menú de inicio; elige **Continuar** para cargar tu partida o **Nueva ciudad** para empezar desde cero. Para probar directamente el mapa con **F6**, abre `ciudad.tscn`.

## Controles

- **WASD** o flechas: mover la cámara.
- **Rueda del mouse**: acercar o alejar.
- **Arrastrar con botón central**: mover la cámara.
- Selecciona una construcción en el menú inferior y haz clic en el mapa para colocarla.
- Las casas, comercios y servicios necesitan tocar un camino.
- **Clic derecho** o **Esc**: cancelar la construcción seleccionada.
- Haz clic en un edificio con monedas para recoger sus ingresos.
- Usa **Guardar** y **Cargar** en la barra superior.
- Abre **Herramientas → Demoler** para retirar un edificio o camino y recuperar el 50% del costo.
- La cuadrícula aparece al elegir una construcción. En **Terreno** puedes pagar para expandir dos celdas por cada lado; hay cuatro ampliaciones y el precio sube en cada compra.
- Pulsa **Menú** arriba a la derecha para guardar y volver a la pantalla inicial.

La partida empieza con $5.000 en un mapa isométrico de 20×20 celdas. Puedes comprar hasta cuatro ampliaciones del terreno, que llegan a 36×36 celdas. Los ingresos se producen cada cierto tiempo y el guardado automático ocurre cada 30 segundos.

## Estructura

- `scripts/city_controller.gd`: conecta la interfaz, la escena y los sistemas.
- `scripts/main_menu.gd`: menú de inicio y acceso a partidas guardadas.
- `scripts/city_world.gd`: dibuja el mapa, los edificios, caminos y vista previa.
- `scripts/grid_manager.gd`: cuadrícula, huellas, ocupación y acceso a caminos.
- `scripts/building_manager.gd`: colocación, producción y cobro.
- `scripts/game_state.gd`: dinero, población, energía, felicidad, experiencia y niveles.
- `scripts/camera_controller.gd`: desplazamiento, zoom y límites de cámara.
- `scripts/ui_manager.gd`: HUD, catálogo inferior y acciones de partida.
- `scripts/save_manager.gd`: guardado JSON local en `user://villa_verde_save.json`.
- `data/buildings.json`: datos económicos, tamaños, requisitos y sprites de los edificios.
- `assets/edificios/`: terreno y sprites isométricos originales.
- `archive/ciudad_prototype.gd`: conserva el prototipo monolítico anterior como referencia; no está conectado a la escena principal.

## Preparación móvil

La ventana base es apaisada (1280×720), el contenido se expande para diferentes proporciones y los controles principales aceptan entrada táctil. Para exportar a Android/iOS todavía hará falta configurar las plantillas y firmas correspondientes desde Godot.

En pantallas táctiles, arrastra con un dedo para mover el mapa y usa dos dedos para acercar o alejar.

## Verificación

`tests/phase1_smoke.gd` comprueba construcción y colisiones, conexión a caminos, economía, producción y cobro, población, energía, XP, desbloqueos, cámara, guardado/carga e interacción con la tienda y el mapa.
