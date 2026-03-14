# HUMITA - Documento de Desarrollo Completo en Godot

---

## Indice

1. Estructura General del Proyecto
2. Escenas y Nodos: Interfaz de Inicio
3. Escenas y Nodos: HUD y UI en Juego
4. Escenas y Nodos: Personaje (Uma)
5. Escenas y Nodos: Fondo y Tilemaps
6. Escenas y Nodos: Sistema de Dialogo
7. Escenas y Nodos: Sistema de Clima
8. Escenas y Nodos: Inventario
9. Mapas: Descripcion y Composicion
10. Programacion: Scripts Completos
11. Dialogos Completos por Zona
12. Notas de Implementacion Final

---

## 1. Estructura General del Proyecto

### Arbol de Carpetas

```
res://
├── scenes/
│   ├── ui/
│   │   ├── MainMenu.tscn
│   │   ├── HUD.tscn
│   │   ├── DialogBox.tscn
│   │   ├── InventoryPanel.tscn
│   │   ├── Notebook.tscn
│   │   └── PauseMenu.tscn
│   ├── world/
│   │   ├── Casa.tscn
│   │   ├── Barrio.tscn
│   │   ├── Plaza.tscn
│   │   ├── ZonaIntermedia.tscn
│   │   ├── Antesala.tscn
│   │   └── Climax.tscn
│   ├── characters/
│   │   ├── Uma.tscn
│   │   ├── Pablo.tscn
│   │   └── npcs/
│   │       ├── NPC_Base.tscn
│   │       ├── NPC_Hermano.tscn
│   │       ├── NPC_Madre.tscn
│   │       ├── NPC_Kiosquero.tscn
│   │       ├── NPC_Lectora.tscn
│   │       └── NPC_Anciano.tscn
│   ├── objects/
│   │   ├── PickupItem.tscn
│   │   ├── Radio.tscn
│   │   ├── DiscSlot.tscn
│   │   ├── Bench.tscn
│   │   └── Door.tscn
│   └── systems/
│       ├── WeatherSystem.tscn
│       ├── MusicManager.tscn
│       └── GameManager.tscn
├── scripts/
│   ├── ui/
│   │   ├── main_menu.gd
│   │   ├── hud.gd
│   │   ├── dialog_box.gd
│   │   ├── inventory_panel.gd
│   │   ├── notebook.gd
│   │   └── pause_menu.gd
│   ├── characters/
│   │   ├── uma.gd
│   │   ├── pablo.gd
│   │   └── npc_base.gd
│   ├── world/
│   │   ├── casa.gd
│   │   ├── barrio.gd
│   │   ├── plaza.gd
│   │   ├── zona_intermedia.gd
│   │   ├── antesala.gd
│   │   └── climax.gd
│   ├── objects/
│   │   ├── pickup_item.gd
│   │   ├── radio.gd
│   │   └── disk_slot.gd
│   └── systems/
│       ├── weather_system.gd
│       ├── music_manager.gd
│       ├── game_manager.gd
│       └── dialog_data.gd
├── assets/
│   ├── sprites/
│   │   ├── uma/
│   │   ├── pablo/
│   │   ├── npcs/
│   │   ├── objects/
│   │   ├── ui/
│   │   └── tilesets/
│   ├── audio/
│   │   ├── music/
│   │   └── sfx/
│   └── fonts/
└── data/
    ├── dialogos.json
    └── items.json
```

### Autoloads (Singletons)

En Proyecto > Configuracion del Proyecto > Autoload registrar:

| Nombre | Ruta |
|--------|------|
| GameManager | res://scripts/systems/game_manager.gd |
| MusicManager | res://scripts/systems/music_manager.gd |
| DialogData | res://scripts/systems/dialog_data.gd |

---

## 2. Escenas y Nodos: Interfaz de Inicio

### MainMenu.tscn

```
MainMenu (Node2D)
├── Background (TextureRect)
│   └── [textura: fondo difuso, paleta fria-calida]
├── TitleContainer (VBoxContainer)
│   ├── TitleLabel (Label)
│   │   └── text: "HUMITA"
│   │   └── [fuente personalizada, tamaño grande, centrado]
│   ├── SubtitleLabel (Label)
│   │   └── text: "una historia pequeña"
│   │   └── [fuente pequeña, italic, centrado]
│   └── Spacer (Control)
│       └── custom_minimum_size.y = 40
├── ButtonContainer (VBoxContainer)
│   ├── BtnJugar (Button)
│   │   └── text: "Jugar"
│   ├── BtnContinuar (Button)
│   │   └── text: "Continuar"
│   │   └── [visible = false si no hay save]
│   ├── BtnOpciones (Button)
│   │   └── text: "Opciones"
│   └── BtnSalir (Button)
│       └── text: "Salir"
├── VersionLabel (Label)
│   └── text: "v0.1"
│   └── [esquina inferior derecha, tamaño minimo]
└── TransitionOverlay (ColorRect)
    └── color: Color(0, 0, 0, 0)
    └── [cubre toda la pantalla, alpha 0 por defecto]
```

### OpcionesMenu.tscn

```
OpcionesMenu (Control)
├── Panel (PanelContainer)
│   └── VBoxContainer
│       ├── TitleLabel (Label) — text: "Opciones"
│       ├── MusicaSlider (HSlider) — label: "Musica"
│       ├── SFXSlider (HSlider) — label: "Efectos"
│       ├── TextVelocidadSlider (HSlider) — label: "Velocidad de texto"
│       ├── IdiomaOptions (OptionButton)
│       └── BtnVolver (Button) — text: "Volver"
└── Overlay (ColorRect)
    └── [fondo semitransparente detras del panel]
```

---

## 3. Escenas y Nodos: HUD y UI en Juego

### HUD.tscn

El HUD es minimalista. No muestra stats ni barras. Solo los elementos necesarios.

```
HUD (CanvasLayer)
├── TopBar (HBoxContainer)
│   └── [anclado: top-left, margen 16px]
│   (vacio en gameplay normal, puede mostrar zona actual)
├── InteractLabel (Label)
│   └── text: "[E] Hablar / Recoger"
│   └── [anclado: bottom-center]
│   └── visible = false por defecto
├── InventoryHint (HBoxContainer)
│   └── [anclado: bottom-right]
│   └── [muestra iconos de los 6 slots, grises si vacios]
│   ├── Slot1 (TextureRect)
│   ├── Slot2 (TextureRect)
│   ├── Slot3 (TextureRect)
│   ├── Slot4 (TextureRect)
│   ├── Slot5 (TextureRect)
│   └── Slot6 (TextureRect)
├── ClimateIndicator (Control)
│   └── [icono discreto: sol / nube / lluvia]
│   └── [anclado: top-right, tamaño 24x24px]
└── ScreenOverlay (ColorRect)
    └── [para tints de clima: azul suave en lluvia, naranja en sol]
    └── color variable segun estado
    └── mouse_filter = MOUSE_FILTER_IGNORE
```

### PauseMenu.tscn

```
PauseMenu (CanvasLayer)
├── Overlay (ColorRect)
│   └── color: Color(0, 0, 0, 0.5)
├── Panel (PanelContainer)
│   └── VBoxContainer
│       ├── TitleLabel — text: "Pausa"
│       ├── BtnCuaderno (Button) — text: "Cuaderno de bocetos"
│       ├── BtnInventario (Button) — text: "Objetos"
│       ├── BtnOpciones (Button) — text: "Opciones"
│       ├── BtnMenu (Button) — text: "Menu principal"
│       └── BtnContinuar (Button) — text: "Continuar"
└── [el cuaderno y el inventario se abren como sub-paneles]
```

---

## 4. Escenas y Nodos: Personaje Uma

### Uma.tscn

```
Uma (CharacterBody2D)
├── Sprite2D
│   └── texture: spritesheet_uma.png
│   └── hframes: 8, vframes: 4
├── AnimationPlayer
│   └── [animaciones: idle_down, idle_up, idle_side,
│          walk_down, walk_up, walk_side,
│          sit, draw, think]
├── CollisionShape2D
│   └── CapsuleShape2D (ancho 10, alto 20)
├── InteractArea (Area2D)
│   └── [detecta objetos y NPCs en rango]
│   └── CollisionShape2D — CircleShape2D radio 32
├── ShadowSprite (Sprite2D)
│   └── [elipse oscura bajo los pies, alpha 0.3]
├── HatSprite (Sprite2D)
│   └── [sombrero, posicion relativa a la cabeza]
│   └── visible = true por defecto
└── InternalThoughtLabel (Label)
    └── [texto de pensamientos internos de Uma]
    └── [aparece flotando sobre el personaje]
    └── visible = false por defecto
```

### Sprites de Uma: Animaciones Necesarias

Cada direccion (down, up, side) necesita los siguientes frames:

- idle: 2 frames (respiracion sutil)
- walk: 4 frames
- sit: 1 frame estatico
- draw: 3 frames (mano en cuaderno)
- think: 2 frames (Uma mirando al costado)

La animacion side se espeja horizontalmente para left/right.

---

## 5. Escenas y Nodos: Fondo y Tilemaps

### Estructura de Mapa Base

Cada escena de mundo sigue esta estructura de capas:

```
[NombreEscena] (Node2D)
├── WorldEnvironment
│   └── Environment (recurso)
├── TileMapLayers (Node2D)
│   ├── LayerSuelo (TileMapLayer)
│   │   └── [capa 0: piso, vereda, tierra, cesped]
│   ├── LayerDecoFondo (TileMapLayer)
│   │   └── [capa 1: elementos detras del personaje]
│   ├── LayerFrente (TileMapLayer)
│   │   └── [capa 2: elementos delante del personaje]
│   │   └── y_sort_enabled = true
│   └── LayerTecho (TileMapLayer)
│       └── [capa 3: techos, follaje alto]
├── Characters (Node2D)
│   └── y_sort_enabled = true
│   └── [Uma y NPCs van aqui]
├── Objects (Node2D)
│   └── y_sort_enabled = true
│   └── [items, radios, bancos interactuables]
├── Triggers (Node2D)
│   └── [Area2D invisibles para eventos narrativos]
├── WeatherLayer (CanvasLayer)
│   └── [particulas de lluvia, overlay de tint]
└── HUD (CanvasLayer)
    └── [instancia de HUD.tscn]
```

### Tilesets Necesarios

#### Tileset: Exterior Urbano

Tiles requeridos (16x16 px recomendado, escala x2 en engine):

- Vereda lisa (variantes: limpia, con grieta, con mancha)
- Calle (variantes: con y sin lineas)
- Cesped (variantes: corto, largo, con flores)
- Tierra
- Baldosa de plaza (variantes: entera, rota)
- Agua (fuente seca, charco)

#### Tileset: Fachadas y Estructuras

- Pared de edificio (variantes: revoques, ladrillo, azulejo)
- Ventana (cerrada, con persiana baja, iluminada)
- Puerta (cerrada, entreabierta)
- Rejas
- Alero / cornisa
- Techo plano, techo inclinado

#### Tileset: Vegetacion

- Arbol (base, copa baja, copa alta)
- Arbusto
- Planta en maceta
- Enredadera en pared

#### Tileset: Mobiliario Urbano

- Banco de plaza (vacio, con persona)
- Farola (apagada, encendida)
- Poste de luz
- Kiosco (estructura)
- Parada de colectivo

#### Tileset: Interior (Casa)

- Piso de madera
- Piso de ceramica
- Pared interior
- Muebles: mesa, silla, estante, cama, heladera
- Alfombra
- Puerta interior

### Dimensiones de Mapas

| Escena | Ancho (tiles) | Alto (tiles) | Notas |
|--------|--------------|-------------|-------|
| Casa | 20 x 14 | Interior | Planta baja con 4 habitaciones |
| Barrio | 40 x 20 | Exterior | 3 cuadras lineales con cruces |
| Plaza | 30 x 30 | Exterior | Espacio abierto con bordes |
| Zona Intermedia | 20 x 40 | Pasaje | Lineal vertical, mas estrecho |
| Antesala | 18 x 18 | Patio | Cerrado con cielo visible |
| Climax | 16 x 14 | Interior/semi | Espacio final contenido |

---

## 6. Escenas y Nodos: Sistema de Dialogo

### DialogBox.tscn

```
DialogBox (CanvasLayer)
├── Panel (PanelContainer)
│   └── [anclado: bottom, margen 24px todos los lados]
│   └── custom_minimum_size = Vector2(0, 120)
│   ├── MarginContainer
│   │   └── VBoxContainer
│   │       ├── NameLabel (Label)
│   │       │   └── [nombre del hablante, puede estar vacio]
│   │       │   └── [fuente bold, tamaño 14]
│   │       ├── TextLabel (RichTextLabel)
│   │       │   └── [texto del dialogo, aparece letra a letra]
│   │       │   └── bbcode_enabled = true
│   │       └── ChoicesContainer (VBoxContainer)
│   │           └── [opciones de respuesta, visible solo si hay choices]
│   │           ├── Choice1 (Button)
│   │           ├── Choice2 (Button)
│   │           └── Choice3 (Button)
└── ContinueIndicator (TextureRect)
    └── [pequeño triangulo o icono que parpadea]
    └── [indica que hay que presionar para continuar]
    └── [anclado: bottom-right del panel]
```

### Estructura de un Dialogo en JSON

```json
{
  "id": "npc_kiosquero_01",
  "speaker": "Kiosquero",
  "lines": [
    {
      "text": "Buenas.",
      "next": "npc_kiosquero_02"
    }
  ]
}
```

```json
{
  "id": "npc_kiosquero_02",
  "speaker": "Kiosquero",
  "lines": [
    {
      "text": "Hoy hace calor, che.",
      "next": null
    }
  ]
}
```

Dialogo con choices:

```json
{
  "id": "uma_choice_banco",
  "speaker": "",
  "lines": [
    {
      "text": "El banco esta libre. Podria sentarme un rato.",
      "choices": [
        {
          "text": "Sentarse",
          "next": "uma_sienta_banco"
        },
        {
          "text": "Seguir caminando",
          "next": null
        }
      ]
    }
  ]
}
```

Dialogo con condicion:

```json
{
  "id": "pablo_final_disco",
  "condition": "has_item:disco_vinilo",
  "speaker": "Pablo",
  "lines": [
    {
      "text": "Sabia que ibas a reconocer esa cancion.",
      "next": "pablo_final_02"
    }
  ]
}
```

---

## 7. Escenas y Nodos: Sistema de Clima

### WeatherSystem.tscn

```
WeatherSystem (Node2D)
├── RainParticles (GPUParticles2D)
│   └── [configuracion de lluvia, deshabilitado por defecto]
├── SunOverlay (ColorRect)
│   └── color: Color(1.0, 0.9, 0.5, 0.08)
│   └── [tint calido-amarillo para estado soleado]
├── CloudOverlay (ColorRect)
│   └── color: Color(0.8, 0.85, 0.95, 0.12)
│   └── [tint frio-gris para estado nublado]
├── RainOverlay (ColorRect)
│   └── color: Color(0.6, 0.7, 0.9, 0.15)
│   └── [tint azul-suave para lluvia]
└── AudioPlayer (AudioStreamPlayer2D)
    └── [sonido ambiental: lluvia, viento suave]
```

### Estados del Clima

```
SUNNY = 0
CLOUDY = 1
RAIN = 2
```

---

## 8. Escenas y Nodos: Inventario

### InventoryPanel.tscn

```
InventoryPanel (Control)
├── Background (PanelContainer)
│   └── [panel con borde suave]
├── TitleLabel (Label) — text: "Objetos de Uma"
├── GridContainer (GridContainer)
│   └── columns = 3
│   ├── ItemSlot1 (PanelContainer)
│   │   ├── ItemIcon (TextureRect)
│   │   └── ItemNameLabel (Label)
│   ├── ItemSlot2 ...
│   ├── ItemSlot3 ...
│   ├── ItemSlot4 ...
│   ├── ItemSlot5 ...
│   └── ItemSlot6 ...
├── DescriptionBox (RichTextLabel)
│   └── [muestra descripcion del item seleccionado]
└── BtnCerrar (Button) — text: "Cerrar"
```

### Notebook.tscn

```
Notebook (Control)
├── Background (TextureRect)
│   └── [textura de pagina de cuaderno]
├── PageContainer (Control)
│   ├── SketchDisplay (TextureRect)
│   │   └── [muestra el boceto de la pagina actual]
│   └── PageText (RichTextLabel)
│       └── [texto o descripcion del boceto]
├── NavigationRow (HBoxContainer)
│   ├── BtnPrev (Button) — text: "<"
│   ├── PageIndicator (Label) — text: "1 / 4"
│   └── BtnNext (Button) — text: ">"
└── BtnCerrar (Button) — text: "Cerrar cuaderno"
```

---

## 9. Mapas: Descripcion y Composicion Detallada

### Mapa 1: Casa

#### Layout (20 x 14 tiles)

```
[Planta de la casa - representacion ASCII]

+--------------------+
|  HABITACION UMA    |
|  cama  estante     |
|  escritorio        |
+--------+-----------+
|BAÑO    |  LIVING   |
|        |  sofa     |
+--------+  tv       |
|COCINA  |           |
|mesa    +-----------+
|        | HABITACION|
|        | HERMANO   |
+--------+-----------+
```

#### Elementos Interactuables

- Ventana de la habitacion (trigger: ver brillo en calle)
- Estante con discos (trigger: inspeccion, Uma lee titulo)
- Escritorio con cuaderno (trigger: abrir cuaderno, ver dibujo)
- Hermano en living (NPC, dialogo)
- Madre en cocina (NPC, dialogo)
- Mesa de cocina (trigger: leer nota del padre)
- Puerta de entrada (trigger: salida al barrio, activa tras evento detonante)

#### Triggers de Zona

- TriggerVentana: Area2D frente a ventana habitacion
- TriggerEstante: Area2D frente al estante
- TriggerCuaderno: Area2D frente al escritorio
- TriggerNota: Area2D frente a mesa de cocina
- TriggerSalida: Area2D en puerta de entrada

---

### Mapa 2: Barrio

#### Layout (40 x 20 tiles)

```
[Vista cenital del barrio - 3 cuadras]

NORTE
+----------------------------------------+
| edificio  edificio  edificio  edificio |
|  |||      |||       |||       |||      |
|  |||      |||       |||       |||      |
|-- vereda --------------------------------
|          [arbol] [arbol] [arbol]       |
|          <- Uma camina aqui ->         |
|          [arbol] [arbol] [arbol]       |
|-- vereda --------------------------------
| casa    TIENDA    KIOSCO   casa        |
| (cerrada)(cerrada) [radio]             |
+----------------------------------------+
SUR (hacia la plaza)
```

#### Elementos Interactuables

- NPC Señora del balcon (trigger: acercarse al edificio)
- Kiosco con radio (trigger: acercarse, escuchar melodia)
- Tienda cerrada (trigger: ver cartel y foto polaroid)
- NPC Dos chicos jugando a las cartas (trigger: acercarse)
- NPC Hombre mayor en banco de vereda (trigger: acercarse)
- Fragmento de tapa de disco en esquina (pickup item)
- Arbol con sombra (trigger: Uma comenta sobre el frescor)
- Salida norte (regresa a casa)
- Salida sur (avanza a plaza)

#### Triggers de Zona

- TriggerSeñora: Area2D bajo balcon
- TriggerKiosco: Area2D frente al kiosco
- TriggerTienda: Area2D frente a persiana
- TriggerFoto: Area2D junto al cartel
- TriggerChicos: Area2D frente a los chicos
- TriggerAnciano: Area2D frente al banco del anciano
- TriggerFragmento: Area2D sobre el fragmento
- TriggerSombra: Area2D bajo los arboles (comentario interno de Uma)

---

### Mapa 3: Plaza

#### Layout (30 x 30 tiles)

```
[Vista cenital de la plaza]

+------------------------------+
| [edificio] [galeria cubierta]|
|  adolescentes bajo galeria   |
|--vereda----------------------|
|[arb]                   [arb] |
|                              |
|     [banco lectora]          |
|                              |
|         [FUENTE]             |
|          (disco)             |
|                              |
|   [banco Uma / opcional]     |
|                              |
|[arb]                   [arb] |
|--vereda----------------------|
| [señor palomas] [flores]     |
+------------------------------+
```

#### Elementos Interactuables

- NPC Lectora en banco bajo arbol (trigger: acercarse)
- NPC Señor de las palomas (trigger: acercarse)
- Adolescentes bajo galeria (trigger: acercarse, escuchar musica)
- Fuente seca con disco de vinilo (pickup item)
- Banco central (trigger: sentarse, accion opcional de dibujo)
- Galeria cubierta (zona de refugio de lluvia)
- Salida norte (regresa al barrio)
- Salida sur (avanza a zona intermedia)

#### Evento Climatico

- Al entrar a la plaza el clima es SUNNY
- Despues de 60 segundos o al interactuar con la lectora, transicion a CLOUDY
- Despues de recoger el disco o de la transicion a CLOUDY + 30 segundos, transicion a RAIN

---

### Mapa 4: Zona Intermedia

#### Layout (20 x 40 tiles, vertical)

```
[Pasaje vertical]

+--------------------+
| [planta sin maceta]|
|                    |
| [radio sin nadie]  |
|                    |
| [banco igual plaza]|
|                    |
+---[puerta entreabierta]---+
|   (voz de la madre)       |
+---------------------------+
|                           |
| [ranura para disco]       |
| [gancho para sombrero]    |
| [marco vacio]             |
|                           |
+--[pasaje final]--+
```

#### Elementos Interactuables

- Planta sin maceta (trigger: Uma comenta sobre el desplazamiento)
- Radio sin nadie (trigger: acercarse, sonido cortado)
- Banco igual al de la plaza (trigger: Uma reconoce el banco)
- Puerta entreabierta (trigger: voz de la madre)
- Ranura para disco (activador, requiere disco de vinilo)
- Gancho para sombrero (activador, requiere tener sombrero)
- Marco vacio (activador, requiere foto polaroid)
- Pasaje final (trigger de avance, se activa tras recorrer la zona)

---

### Mapa 5: Antesala

#### Layout (18 x 18 tiles, patio)

```
[Patio cerrado]

+------------------+
|[pared]   [pared] |
|                  |
|  [farola apagada]|
|                  |
|  [banco + nota]  |
|                  |
|[pared]   [pared] |
+------[puerta]----+
       (al climax)
```

#### Elementos Interactuables

- Banco con nota del padre (trigger: leer nota, pickup opcional)
- Farola apagada (trigger: Uma mira hacia arriba, comentario)
- Puerta al climax (trigger de avance, activacion por decision del jugador)
- Lluvia de fondo (RAIN activo desde entrada)

---

### Mapa 6: Climax y Final

#### Layout (16 x 14 tiles)

```
[Espacio del final]

+----------------+
|[pared]  [pared]|
|                |
| [radio antigua]|
|   (slot disco) |
|                |
|  [banco/silla] |
|                |
|[cortina lluvia]|
+-------+--------+
        | abierto al patio
```

#### Elementos Interactuables

- Radio antigua con slot (activador principal, requiere disco)
- Banco o silla central (Uma puede sentarse)
- Cortina de lluvia / abertura (visual, no funcional)
- Pablo (NPC especial, aparece al activar la radio o al sentarse)

---

## 10. Programacion: Scripts Completos

### game_manager.gd

```gdscript
# res://scripts/systems/game_manager.gd
extends Node

# Estado del juego
var current_scene: String = ""
var weather_state: int = 0  # 0=SUNNY, 1=CLOUDY, 2=RAIN

# Flags de eventos completados
var flags: Dictionary = {
	"prologue_done": false,
	"heard_melody_kiosk": false,
	"met_brother": false,
	"met_mother": false,
	"read_father_note_1": false,
	"read_father_note_2": false,
	"sat_bench_plaza": false,
	"drew_in_notebook": false,
	"door_intermediate_triggered": false,
	"disc_slot_activated": false,
	"hat_hook_activated": false,
	"photo_frame_activated": false,
	"radio_activated": false,
	"pablo_appeared": false,
	"game_complete": false
}

# Inventario
var inventory: Array = []  # Array de item_id strings, max 6

# Paginas del cuaderno
var notebook_pages: Array = []

# Instancia actual de Uma (referencia externa)
var uma_node: CharacterBody2D = null

func _ready() -> void:
	pass

func set_flag(flag_name: String, value: bool) -> void:
	if flags.has(flag_name):
		flags[flag_name] = value
	else:
		push_warning("GameManager: flag desconocido: " + flag_name)

func get_flag(flag_name: String) -> bool:
	if flags.has(flag_name):
		return flags[flag_name]
	push_warning("GameManager: flag desconocido: " + flag_name)
	return false

func add_item(item_id: String) -> bool:
	if inventory.size() >= 6:
		return false
	if inventory.has(item_id):
		return false
	inventory.append(item_id)
	return true

func has_item(item_id: String) -> bool:
	return inventory.has(item_id)

func remove_item(item_id: String) -> void:
	inventory.erase(item_id)

func add_notebook_page(page_data: Dictionary) -> void:
	notebook_pages.append(page_data)

func change_scene(scene_path: String) -> void:
	current_scene = scene_path
	get_tree().change_scene_to_file(scene_path)

func save_game() -> void:
	var save_data: Dictionary = {
		"current_scene": current_scene,
		"weather_state": weather_state,
		"flags": flags,
		"inventory": inventory,
		"notebook_pages": notebook_pages
	}
	var save_file = FileAccess.open("user://save.json", FileAccess.WRITE)
	save_file.store_string(JSON.stringify(save_data))
	save_file.close()

func load_game() -> bool:
	if not FileAccess.file_exists("user://save.json"):
		return false
	var save_file = FileAccess.open("user://save.json", FileAccess.READ)
	var content = save_file.get_as_text()
	save_file.close()
	var data = JSON.parse_string(content)
	if data == null:
		return false
	current_scene = data.get("current_scene", "")
	weather_state = data.get("weather_state", 0)
	flags = data.get("flags", flags)
	inventory = data.get("inventory", [])
	notebook_pages = data.get("notebook_pages", [])
	return true

func has_save() -> bool:
	return FileAccess.file_exists("user://save.json")
```

---

### uma.gd

```gdscript
# res://scripts/characters/uma.gd
extends CharacterBody2D

const SPEED: float = 80.0

@onready var sprite: Sprite2D = $Sprite2D
@onready var anim: AnimationPlayer = $AnimationPlayer
@onready var interact_area: Area2D = $InteractArea
@onready var hat_sprite: Sprite2D = $HatSprite
@onready var thought_label: Label = $InternalThoughtLabel

var direction: Vector2 = Vector2.ZERO
var last_direction: Vector2 = Vector2.DOWN
var can_move: bool = true
var is_sitting: bool = false
var has_hat: bool = true

var nearby_interactables: Array = []

func _ready() -> void:
	interact_area.body_entered.connect(_on_body_entered_range)
	interact_area.body_exited.connect(_on_body_exited_range)
	interact_area.area_entered.connect(_on_area_entered_range)
	interact_area.area_exited.connect(_on_area_exited_range)

func _physics_process(delta: float) -> void:
	if not can_move or is_sitting:
		velocity = Vector2.ZERO
		move_and_slide()
		return

	direction = Input.get_vector("ui_left", "ui_right", "ui_up", "ui_down")

	if direction != Vector2.ZERO:
		last_direction = direction
		velocity = direction * SPEED
		_update_walk_animation()
	else:
		velocity = Vector2.ZERO
		_update_idle_animation()

	move_and_slide()

func _input(event: InputEvent) -> void:
	if event.is_action_pressed("interact"):
		_try_interact()
	if event.is_action_pressed("ui_cancel"):
		_toggle_pause()

func _try_interact() -> void:
	if nearby_interactables.is_empty():
		return
	var closest = _get_closest_interactable()
	if closest and closest.has_method("interact"):
		closest.interact()

func _get_closest_interactable() -> Node:
	var closest: Node = null
	var min_dist: float = INF
	for obj in nearby_interactables:
		if is_instance_valid(obj):
			var dist = global_position.distance_to(obj.global_position)
			if dist < min_dist:
				min_dist = dist
				closest = obj
	return closest

func _update_walk_animation() -> void:
	var anim_name: String
	if abs(direction.x) > abs(direction.y):
		anim_name = "walk_side"
		sprite.flip_h = direction.x < 0
	elif direction.y < 0:
		anim_name = "walk_up"
	else:
		anim_name = "walk_down"
	if anim.current_animation != anim_name:
		anim.play(anim_name)

func _update_idle_animation() -> void:
	var anim_name: String
	if abs(last_direction.x) > abs(last_direction.y):
		anim_name = "idle_side"
	elif last_direction.y < 0:
		anim_name = "idle_up"
	else:
		anim_name = "idle_down"
	if anim.current_animation != anim_name:
		anim.play(anim_name)

func sit_down() -> void:
	is_sitting = true
	anim.play("sit")

func stand_up() -> void:
	is_sitting = false
	_update_idle_animation()

func remove_hat() -> void:
	has_hat = false
	hat_sprite.visible = false

func put_hat_back() -> void:
	has_hat = true
	hat_sprite.visible = true

func show_thought(text: String, duration: float = 3.0) -> void:
	thought_label.text = text
	thought_label.visible = true
	await get_tree().create_timer(duration).timeout
	thought_label.visible = false

func freeze() -> void:
	can_move = false

func unfreeze() -> void:
	can_move = true

func _on_body_entered_range(body: Node2D) -> void:
	if body.has_method("interact"):
		nearby_interactables.append(body)
		_show_interact_hint(true)

func _on_body_exited_range(body: Node2D) -> void:
	nearby_interactables.erase(body)
	if nearby_interactables.is_empty():
		_show_interact_hint(false)

func _on_area_entered_range(area: Area2D) -> void:
	if area.has_method("interact"):
		nearby_interactables.append(area)
		_show_interact_hint(true)

func _on_area_exited_range(area: Area2D) -> void:
	nearby_interactables.erase(area)
	if nearby_interactables.is_empty():
		_show_interact_hint(false)

func _show_interact_hint(visible: bool) -> void:
	var hud = get_tree().get_first_node_in_group("hud")
	if hud:
		hud.set_interact_hint(visible)

func _toggle_pause() -> void:
	var pause_menu = get_tree().get_first_node_in_group("pause_menu")
	if pause_menu:
		pause_menu.toggle()
```

---

### dialog_box.gd

```gdscript
# res://scripts/ui/dialog_box.gd
extends CanvasLayer

signal dialog_finished
signal choice_selected(choice_index: int)

@onready var panel: PanelContainer = $Panel
@onready var name_label: Label = $Panel/MarginContainer/VBoxContainer/NameLabel
@onready var text_label: RichTextLabel = $Panel/MarginContainer/VBoxContainer/TextLabel
@onready var choices_container: VBoxContainer = $Panel/MarginContainer/VBoxContainer/ChoicesContainer
@onready var continue_indicator: TextureRect = $ContinueIndicator

var is_active: bool = false
var is_typing: bool = false
var current_text: String = ""
var char_index: int = 0
var type_speed: float = 0.03
var type_timer: float = 0.0

var current_dialog_id: String = ""
var dialog_queue: Array = []

func _ready() -> void:
	panel.visible = false
	choices_container.visible = false
	continue_indicator.visible = false

func _process(delta: float) -> void:
	if not is_typing:
		return
	type_timer += delta
	if type_timer >= type_speed:
		type_timer = 0.0
		_type_next_char()

func start_dialog(dialog_id: String) -> void:
	var dialog_data = DialogData.get_dialog(dialog_id)
	if dialog_data == null:
		push_warning("DialogBox: dialogo no encontrado: " + dialog_id)
		return

	# Verificar condicion si existe
	if dialog_data.has("condition"):
		if not _check_condition(dialog_data["condition"]):
			# Buscar alternativa
			var alt_id = dialog_data.get("condition_fail", null)
			if alt_id:
				start_dialog(alt_id)
			return

	is_active = true
	panel.visible = true
	GameManager.uma_node.freeze()

	var lines = dialog_data.get("lines", [])
	if lines.is_empty():
		return

	dialog_queue = lines.duplicate()
	_show_next_line()

func _show_next_line() -> void:
	if dialog_queue.is_empty():
		_end_dialog()
		return

	var line = dialog_queue.pop_front()
	var speaker = line.get("speaker", "")
	var text = line.get("text", "")

	name_label.text = speaker
	name_label.visible = speaker != ""

	current_text = text
	text_label.text = ""
	char_index = 0
	is_typing = true
	continue_indicator.visible = false
	choices_container.visible = false

	# Guardar next y choices para cuando termine el tipeo
	_current_line_data = line

var _current_line_data: Dictionary = {}

func _type_next_char() -> void:
	if char_index >= current_text.length():
		is_typing = false
		_on_typing_finished()
		return
	text_label.text += current_text[char_index]
	char_index += 1

func _on_typing_finished() -> void:
	var choices = _current_line_data.get("choices", [])
	if choices.size() > 0:
		_show_choices(choices)
	else:
		continue_indicator.visible = true

func _show_choices(choices: Array) -> void:
	choices_container.visible = true
	continue_indicator.visible = false

	# Limpiar botones anteriores
	for child in choices_container.get_children():
		child.queue_free()

	for i in choices.size():
		var btn = Button.new()
		btn.text = choices[i]["text"]
		var next_id = choices[i].get("next", null)
		btn.pressed.connect(func(): _on_choice_pressed(i, next_id))
		choices_container.add_child(btn)

func _on_choice_pressed(index: int, next_id) -> void:
	choices_container.visible = false
	choice_selected.emit(index)
	if next_id:
		start_dialog(next_id)
	else:
		_end_dialog()

func _input(event: InputEvent) -> void:
	if not is_active:
		return
	if event.is_action_pressed("interact") or event.is_action_pressed("ui_accept"):
		if is_typing:
			# Completar el texto de golpe
			text_label.text = current_text
			char_index = current_text.length()
			is_typing = false
			_on_typing_finished()
		elif choices_container.visible:
			pass  # esperar seleccion de choice
		else:
			var next_id = _current_line_data.get("next", null)
			if dialog_queue.is_empty() and next_id == null:
				_end_dialog()
			elif next_id:
				start_dialog(next_id)
			else:
				_show_next_line()

func _end_dialog() -> void:
	is_active = false
	panel.visible = false
	GameManager.uma_node.unfreeze()
	dialog_finished.emit()

func _check_condition(condition: String) -> bool:
	if condition.begins_with("has_item:"):
		var item_id = condition.split(":")[1]
		return GameManager.has_item(item_id)
	if condition.begins_with("flag:"):
		var flag_name = condition.split(":")[1]
		return GameManager.get_flag(flag_name)
	return true
```

---

### weather_system.gd

```gdscript
# res://scripts/systems/weather_system.gd
extends Node2D

const SUNNY: int = 0
const CLOUDY: int = 1
const RAIN: int = 2

@onready var rain_particles: GPUParticles2D = $RainParticles
@onready var sun_overlay: ColorRect = $SunOverlay
@onready var cloud_overlay: ColorRect = $CloudOverlay
@onready var rain_overlay: ColorRect = $RainOverlay
@onready var audio_player: AudioStreamPlayer2D = $AudioPlayer

@export var rain_sfx: AudioStream
@export var transition_duration: float = 3.0

var current_state: int = SUNNY
var tween: Tween

func _ready() -> void:
	_apply_state_instant(GameManager.weather_state)

func set_weather(new_state: int) -> void:
	if new_state == current_state:
		return
	current_state = new_state
	GameManager.weather_state = new_state
	_transition_to_state(new_state)

func _transition_to_state(state: int) -> void:
	if tween:
		tween.kill()
	tween = create_tween()
	tween.set_parallel(true)

	match state:
		SUNNY:
			tween.tween_property(sun_overlay, "color:a", 0.08, transition_duration)
			tween.tween_property(cloud_overlay, "color:a", 0.0, transition_duration)
			tween.tween_property(rain_overlay, "color:a", 0.0, transition_duration)
			rain_particles.emitting = false
			_stop_rain_sfx()
		CLOUDY:
			tween.tween_property(sun_overlay, "color:a", 0.0, transition_duration)
			tween.tween_property(cloud_overlay, "color:a", 0.12, transition_duration)
			tween.tween_property(rain_overlay, "color:a", 0.0, transition_duration)
			rain_particles.emitting = false
			_stop_rain_sfx()
		RAIN:
			tween.tween_property(sun_overlay, "color:a", 0.0, transition_duration)
			tween.tween_property(cloud_overlay, "color:a", 0.0, transition_duration)
			tween.tween_property(rain_overlay, "color:a", 0.15, transition_duration)
			rain_particles.emitting = true
			_start_rain_sfx()

func _apply_state_instant(state: int) -> void:
	current_state = state
	match state:
		SUNNY:
			sun_overlay.color.a = 0.08
			cloud_overlay.color.a = 0.0
			rain_overlay.color.a = 0.0
			rain_particles.emitting = false
		CLOUDY:
			sun_overlay.color.a = 0.0
			cloud_overlay.color.a = 0.12
			rain_overlay.color.a = 0.0
			rain_particles.emitting = false
		RAIN:
			sun_overlay.color.a = 0.0
			cloud_overlay.color.a = 0.0
			rain_overlay.color.a = 0.15
			rain_particles.emitting = true
			_start_rain_sfx()

func _start_rain_sfx() -> void:
	if rain_sfx and not audio_player.playing:
		audio_player.stream = rain_sfx
		audio_player.play()

func _stop_rain_sfx() -> void:
	if audio_player.playing:
		audio_player.stop()
```

---

### pickup_item.gd

```gdscript
# res://scripts/objects/pickup_item.gd
extends Area2D

@export var item_id: String = ""
@export var item_name: String = ""
@export var item_description: String = ""
@export var item_icon: Texture2D

@onready var sprite: Sprite2D = $Sprite2D
@onready var label: Label = $Label  # nombre flotante opcional

var already_picked: bool = false

func _ready() -> void:
	if GameManager.has_item(item_id):
		queue_free()

func interact() -> void:
	if already_picked:
		return
	_pick_up()

func _pick_up() -> void:
	var success = GameManager.add_item(item_id)
	if not success:
		# Inventario lleno
		var dialog_box = get_tree().get_first_node_in_group("dialog_box")
		if dialog_box:
			dialog_box.start_dialog("uma_inventory_full")
		return

	already_picked = true

	# Feedback visual: pequena animacion de recogida
	var tween = create_tween()
	tween.tween_property(sprite, "scale", Vector2(1.3, 1.3), 0.1)
	tween.tween_property(sprite, "scale", Vector2.ZERO, 0.2)
	await tween.finished
	queue_free()
```

---

### npc_base.gd

```gdscript
# res://scripts/characters/npc_base.gd
extends CharacterBody2D

@export var npc_id: String = ""
@export var dialog_id: String = ""
@export var dialog_id_after_flag: String = ""
@export var flag_condition: String = ""

@onready var sprite: Sprite2D = $Sprite2D
@onready var anim: AnimationPlayer = $AnimationPlayer

var has_been_talked_to: bool = false

func _ready() -> void:
	anim.play("idle")

func interact() -> void:
	var dialog_to_use: String = dialog_id

	if flag_condition != "" and GameManager.get_flag(flag_condition):
		if dialog_id_after_flag != "":
			dialog_to_use = dialog_id_after_flag

	if dialog_to_use == "":
		return

	var dialog_box = get_tree().get_first_node_in_group("dialog_box")
	if dialog_box:
		dialog_box.start_dialog(dialog_to_use)

	has_been_talked_to = true
```

---

### music_manager.gd

```gdscript
# res://scripts/systems/music_manager.gd
extends Node

@export var tracks: Dictionary = {}
# Ejemplo de estructura:
# {
#   "menu": preload("res://assets/audio/music/menu.ogg"),
#   "casa": preload("res://assets/audio/music/casa.ogg"),
#   "barrio": preload("res://assets/audio/music/barrio.ogg"),
#   "plaza_sunny": preload("res://assets/audio/music/plaza_sunny.ogg"),
#   "plaza_rain": preload("res://assets/audio/music/plaza_rain.ogg"),
#   "intermedia": preload("res://assets/audio/music/intermedia.ogg"),
#   "antesala": preload("res://assets/audio/music/antesala.ogg"),
#   "climax": preload("res://assets/audio/music/climax.ogg"),
#   "cancion_completa": preload("res://assets/audio/music/cancion_completa.ogg")
# }

var player_a: AudioStreamPlayer
var player_b: AudioStreamPlayer
var active_player: AudioStreamPlayer

var current_track: String = ""
var crossfade_duration: float = 2.0

func _ready() -> void:
	player_a = AudioStreamPlayer.new()
	player_b = AudioStreamPlayer.new()
	add_child(player_a)
	add_child(player_b)
	player_a.volume_db = 0.0
	player_b.volume_db = -80.0
	active_player = player_a

func play_track(track_id: String, fade: bool = true) -> void:
	if track_id == current_track:
		return
	if not tracks.has(track_id):
		push_warning("MusicManager: track no encontrada: " + track_id)
		return

	current_track = track_id
	var next_player = player_b if active_player == player_a else player_a
	next_player.stream = tracks[track_id]
	next_player.play()

	if fade:
		var tween = create_tween()
		tween.set_parallel(true)
		tween.tween_property(active_player, "volume_db", -80.0, crossfade_duration)
		tween.tween_property(next_player, "volume_db", 0.0, crossfade_duration)
		await tween.finished
		active_player.stop()

	active_player = next_player

func stop_music(fade: bool = true) -> void:
	if fade:
		var tween = create_tween()
		tween.tween_property(active_player, "volume_db", -80.0, crossfade_duration)
		await tween.finished
	active_player.stop()
	current_track = ""

func set_volume(value: float) -> void:
	# value entre 0.0 y 1.0
	active_player.volume_db = linear_to_db(value)
```

---

### dialog_data.gd

```gdscript
# res://scripts/systems/dialog_data.gd
extends Node

var dialogs: Dictionary = {}

func _ready() -> void:
	_load_dialogs()

func _load_dialogs() -> void:
	var file = FileAccess.open("res://data/dialogos.json", FileAccess.READ)
	if file == null:
		push_error("DialogData: no se pudo abrir dialogos.json")
		return
	var content = file.get_as_text()
	file.close()
	var data = JSON.parse_string(content)
	if data == null:
		push_error("DialogData: JSON invalido en dialogos.json")
		return
	for dialog in data:
		dialogs[dialog["id"]] = dialog

func get_dialog(dialog_id: String) -> Dictionary:
	if dialogs.has(dialog_id):
		return dialogs[dialog_id]
	return {}
```

---

### main_menu.gd

```gdscript
# res://scripts/ui/main_menu.gd
extends Node2D

@onready var btn_jugar: Button = $ButtonContainer/BtnJugar
@onready var btn_continuar: Button = $ButtonContainer/BtnContinuar
@onready var btn_opciones: Button = $ButtonContainer/BtnOpciones
@onready var btn_salir: Button = $ButtonContainer/BtnSalir
@onready var transition_overlay: ColorRect = $TransitionOverlay

func _ready() -> void:
	btn_continuar.visible = GameManager.has_save()

	btn_jugar.pressed.connect(_on_jugar)
	btn_continuar.pressed.connect(_on_continuar)
	btn_opciones.pressed.connect(_on_opciones)
	btn_salir.pressed.connect(_on_salir)

	MusicManager.play_track("menu")

func _on_jugar() -> void:
	_fade_and_load("res://scenes/world/Casa.tscn")

func _on_continuar() -> void:
	if GameManager.load_game():
		_fade_and_load(GameManager.current_scene)

func _on_opciones() -> void:
	pass # abrir panel de opciones

func _on_salir() -> void:
	get_tree().quit()

func _fade_and_load(scene_path: String) -> void:
	var tween = create_tween()
	tween.tween_property(transition_overlay, "color:a", 1.0, 0.5)
	await tween.finished
	GameManager.change_scene(scene_path)
```

---

### casa.gd

```gdscript
# res://scripts/world/casa.gd
extends Node2D

@onready var uma: CharacterBody2D = $Characters/Uma
@onready var dialog_box = $HUD/DialogBox
@onready var trigger_ventana: Area2D = $Triggers/TriggerVentana
@onready var trigger_cuaderno: Area2D = $Triggers/TriggerCuaderno
@onready var trigger_nota: Area2D = $Triggers/TriggerNota
@onready var trigger_salida: Area2D = $Triggers/TriggerSalida
@onready var npc_hermano = $Characters/NPC_Hermano
@onready var npc_madre = $Characters/NPC_Madre

var detonante_activado: bool = false
var detonante_tipo: String = ""

func _ready() -> void:
	GameManager.uma_node = uma
	MusicManager.play_track("casa")

	trigger_ventana.body_entered.connect(_on_ventana)
	trigger_cuaderno.body_entered.connect(_on_cuaderno)
	trigger_nota.body_entered.connect(_on_nota)
	trigger_salida.body_entered.connect(_on_salida)

func _on_ventana(body: Node2D) -> void:
	if body != uma:
		return
	if detonante_activado:
		return
	# Uma ve el brillo desde la ventana
	uma.show_thought("Hay algo en la vereda.")
	await get_tree().create_timer(2.0).timeout
	dialog_box.start_dialog("uma_ventana_brillo")
	_activar_detonante("ventana")

func _on_cuaderno(body: Node2D) -> void:
	if body != uma:
		return
	dialog_box.start_dialog("uma_cuaderno_inspeccion")
	if not detonante_activado:
		_activar_detonante("cuaderno")

func _on_nota(body: Node2D) -> void:
	if body != uma:
		return
	dialog_box.start_dialog("uma_nota_padre_1")
	GameManager.set_flag("read_father_note_1", true)
	GameManager.add_item("nota_padre_1")
	if not detonante_activado:
		_activar_detonante("nota")

func _activar_detonante(tipo: String) -> void:
	if detonante_activado:
		return
	detonante_activado = true
	detonante_tipo = tipo
	GameManager.set_flag("prologue_done", true)

	# La puerta se desbloquea
	await get_tree().create_timer(1.0).timeout
	dialog_box.start_dialog("uma_decision_salir")

func _on_salida(body: Node2D) -> void:
	if body != uma:
		return
	if not detonante_activado:
		uma.show_thought("No tengo ganas de salir todavia.")
		return
	_ir_al_barrio()

func _ir_al_barrio() -> void:
	uma.freeze()
	var tween = create_tween()
	# Fade out
	var overlay = $HUD/TransitionOverlay
	tween.tween_property(overlay, "color:a", 1.0, 0.5)
	await tween.finished
	GameManager.save_game()
	GameManager.change_scene("res://scenes/world/Barrio.tscn")
```

---

### plaza.gd

```gdscript
# res://scripts/world/plaza.gd
extends Node2D

@onready var uma: CharacterBody2D = $Characters/Uma
@onready var dialog_box = $HUD/DialogBox
@onready var weather_system: Node2D = $WeatherLayer/WeatherSystem
@onready var trigger_fuente: Area2D = $Triggers/TriggerFuente
@onready var trigger_banco: Area2D = $Triggers/TriggerBanco
@onready var trigger_salida_sur: Area2D = $Triggers/TriggerSalidaSur

var disco_recogido: bool = false
var tiempo_en_plaza: float = 0.0
var fase_clima: int = 0  # 0=sunny, 1=cloudy, 2=rain

func _ready() -> void:
	GameManager.uma_node = uma
	MusicManager.play_track("plaza_sunny")
	weather_system.set_weather(WeatherSystem.SUNNY)

	trigger_fuente.body_entered.connect(_on_fuente)
	trigger_banco.body_entered.connect(_on_banco)
	trigger_salida_sur.body_entered.connect(_on_salida_sur)

func _process(delta: float) -> void:
	tiempo_en_plaza += delta
	_update_clima()

func _update_clima() -> void:
	match fase_clima:
		0:
			if tiempo_en_plaza > 60.0 or GameManager.get_flag("met_lectora"):
				fase_clima = 1
				weather_system.set_weather(WeatherSystem.CLOUDY)
				MusicManager.play_track("plaza_cloudy")
				uma.show_thought("Por fin se nublo.")
		1:
			if tiempo_en_plaza > 90.0 or disco_recogido:
				fase_clima = 2
				weather_system.set_weather(WeatherSystem.RAIN)
				MusicManager.play_track("plaza_rain")
				uma.show_thought("Lluvia suave. Mejor.")

func _on_fuente(body: Node2D) -> void:
	if body != uma:
		return
	if disco_recogido:
		return
	dialog_box.start_dialog("uma_encuentra_disco")
	GameManager.add_item("disco_vinilo")
	disco_recogido = true
	GameManager.set_flag("found_disc", true)

func _on_banco(body: Node2D) -> void:
	if body != uma:
		return
	dialog_box.start_dialog("uma_choice_banco")

func _on_salida_sur(body: Node2D) -> void:
	if body != uma:
		return
	GameManager.save_game()
	GameManager.change_scene("res://scenes/world/ZonaIntermedia.tscn")
```

---

### climax.gd

```gdscript
# res://scripts/world/climax.gd
extends Node2D

@onready var uma: CharacterBody2D = $Characters/Uma
@onready var pablo: CharacterBody2D = $Characters/Pablo
@onready var dialog_box = $HUD/DialogBox
@onready var radio: Node2D = $Objects/Radio
@onready var weather_system: Node2D = $WeatherLayer/WeatherSystem

func _ready() -> void:
	GameManager.uma_node = uma
	weather_system.set_weather(WeatherSystem.RAIN)
	MusicManager.play_track("antesala")
	pablo.visible = false

	radio.radio_activated.connect(_on_radio_activated)

func _on_radio_activated() -> void:
	if GameManager.get_flag("radio_activated"):
		return
	GameManager.set_flag("radio_activated", true)
	MusicManager.play_track("cancion_completa", true)

	# Mostrar frases flotantes de los diAlogos previos
	await get_tree().create_timer(4.0).timeout
	_show_memory_text("A veces, salir un ratito cambia las cosas.")
	await get_tree().create_timer(3.0).timeout
	_show_memory_text("Soñe que ibas a encontrar algo.")
	await get_tree().create_timer(3.0).timeout
	_show_memory_text("Si ves algo que te llama la atencion, segui por ahi.")
	await get_tree().create_timer(3.0).timeout
	_show_memory_text("Hay cosas que no se entienden hasta que las sentis.")

	await get_tree().create_timer(2.0).timeout
	_aparecer_pablo()

func _show_memory_text(text: String) -> void:
	# Crear label flotante en posicion aleatoria de la pantalla
	var label = Label.new()
	label.text = text
	label.add_theme_color_override("font_color", Color(1, 1, 1, 0.7))
	add_child(label)
	label.position = Vector2(
		randf_range(100, 800),
		randf_range(100, 500)
	)
	var tween = create_tween()
	tween.tween_property(label, "modulate:a", 0.0, 4.0)
	await tween.finished
	label.queue_free()

func _aparecer_pablo() -> void:
	pablo.visible = true
	pablo.modulate.a = 0.0
	var tween = create_tween()
	tween.tween_property(pablo, "modulate:a", 1.0, 2.0)
	await tween.finished

	GameManager.set_flag("pablo_appeared", true)
	await get_tree().create_timer(1.5).timeout
	_start_final_dialog()

func _start_final_dialog() -> void:
	var dialog_id = _choose_pablo_dialog()
	dialog_box.start_dialog(dialog_id)
	dialog_box.dialog_finished.connect(_on_final_dialog_done)

func _choose_pablo_dialog() -> String:
	if GameManager.has_item("disco_vinilo") and GameManager.get_flag("radio_activated"):
		return "pablo_final_disco"
	if GameManager.get_flag("sat_bench_plaza"):
		return "pablo_final_banco"
	if GameManager.has_item("nota_padre_2"):
		return "pablo_final_nota"
	return "pablo_final_base"

func _on_final_dialog_done() -> void:
	GameManager.set_flag("game_complete", true)
	GameManager.save_game()
	await get_tree().create_timer(3.0).timeout
	_roll_credits()

func _roll_credits() -> void:
	var overlay = $HUD/TransitionOverlay
	var tween = create_tween()
	tween.tween_property(overlay, "color:a", 1.0, 3.0)
	await tween.finished

	# Mostrar frase final
	var label = Label.new()
	label.text = "A veces salir un ratito cambia las cosas."
	label.add_theme_font_size_override("font_size", 18)
	label.set_anchors_and_offsets_preset(Control.PRESET_CENTER)
	overlay.add_child(label)
	label.modulate.a = 0.0
	var tween2 = create_tween()
	tween2.tween_property(label, "modulate:a", 1.0, 2.0)
	await tween2.finished

	await get_tree().create_timer(5.0).timeout
	GameManager.change_scene("res://scenes/ui/MainMenu.tscn")
```

---

## 11. Dialogos Completos por Zona

Los dialogos se guardan en `res://data/dialogos.json`. A continuacion se presenta la estructura completa de cada dialogo del juego.

---

### Casa: Dialogos del Prologo

```json
[

  {
    "id": "uma_ventana_brillo",
    "lines": [
      {
        "speaker": "",
        "text": "Hay algo en la vereda. No se que es.",
        "next": "uma_ventana_brillo_02"
      }
    ]
  },
  {
    "id": "uma_ventana_brillo_02",
    "lines": [
      {
        "speaker": "",
        "text": "Brilla. Alguien lo dejo ahi o se cayo de alguna bolsa.",
        "next": null
      }
    ]
  },

  {
    "id": "uma_cuaderno_inspeccion",
    "lines": [
      {
        "speaker": "",
        "text": "El cuaderno esta abierto en una pagina que no recuerdo haber usado.",
        "next": "uma_cuaderno_inspeccion_02"
      }
    ]
  },
  {
    "id": "uma_cuaderno_inspeccion_02",
    "lines": [
      {
        "speaker": "",
        "text": "Hay un dibujo. Una forma que podria ser cualquier cosa. Abajo dice, con otra letra: Ya estuviste cerca.",
        "next": null
      }
    ]
  },

  {
    "id": "uma_nota_padre_1",
    "lines": [
      {
        "speaker": "",
        "text": "Una nota en la mesa. La letra de Papa.",
        "next": "uma_nota_padre_1_texto"
      }
    ]
  },
  {
    "id": "uma_nota_padre_1_texto",
    "lines": [
      {
        "speaker": "Nota",
        "text": "Si ves algo que te llama la atencion, segui por ahi.",
        "choices": [
          {
            "text": "Guardar la nota",
            "next": "uma_nota_guardada"
          },
          {
            "text": "Dejarla en la mesa",
            "next": null
          }
        ]
      }
    ]
  },
  {
    "id": "uma_nota_guardada",
    "lines": [
      {
        "speaker": "",
        "text": "La doblo y la guardo.",
        "next": null
      }
    ]
  },

  {
    "id": "uma_decision_salir",
    "lines": [
      {
        "speaker": "",
        "text": "Afuera hay sol. No me gusta el sol.",
        "next": "uma_decision_salir_02"
      }
    ]
  },
  {
    "id": "uma_decision_salir_02",
    "lines": [
      {
        "speaker": "",
        "text": "Pero algo me llama la atencion. Eso no pasa seguido.",
        "next": "uma_decision_salir_03"
      }
    ]
  },
  {
    "id": "uma_decision_salir_03",
    "lines": [
      {
        "speaker": "",
        "text": "Me pongo el sombrero.",
        "next": null
      }
    ]
  },

  {
    "id": "uma_inventory_full",
    "lines": [
      {
        "speaker": "",
        "text": "No tengo donde ponerlo. Ya llevo demasiadas cosas.",
        "next": null
      }
    ]
  },

  {
    "id": "hermano_dialogo_01",
    "lines": [
      {
        "speaker": "Hermano",
        "text": "Soñe que ibas a encontrar algo. No se que era, pero estabas contenta.",
        "next": "hermano_dialogo_02"
      }
    ]
  },
  {
    "id": "hermano_dialogo_02",
    "lines": [
      {
        "speaker": "Hermano",
        "text": "Te muestro el dibujo si queres.",
        "choices": [
          {
            "text": "Ver el dibujo",
            "next": "hermano_muestra_dibujo"
          },
          {
            "text": "Dejarlo dibujar",
            "next": null
          }
        ]
      }
    ]
  },
  {
    "id": "hermano_muestra_dibujo",
    "lines": [
      {
        "speaker": "Hermano",
        "text": "Mira. No se bien que es. Pero lo dibuje igual.",
        "next": "uma_ve_dibujo_hermano"
      }
    ]
  },
  {
    "id": "uma_ve_dibujo_hermano",
    "lines": [
      {
        "speaker": "",
        "text": "Podria ser una radio. O una flor. O nada. No se.",
        "next": null
      }
    ]
  },

  {
    "id": "madre_dialogo_01",
    "lines": [
      {
        "speaker": "Mama",
        "text": "A veces, salir un ratito cambia las cosas.",
        "next": "madre_dialogo_02"
      }
    ]
  },
  {
    "id": "madre_dialogo_02",
    "lines": [
      {
        "speaker": "Mama",
        "text": "No te estoy diciendo que salgas. Solo lo digo.",
        "next": null
      }
    ]
  },

  {
    "id": "estante_discos",
    "lines": [
      {
        "speaker": "",
        "text": "Hay un disco dado vuelta. La tapa mirando para adentro.",
        "next": "estante_discos_02"
      }
    ]
  },
  {
    "id": "estante_discos_02",
    "lines": [
      {
        "speaker": "",
        "text": "No recuerdo haberlo puesto asi. Se lee la mitad del nombre.",
        "next": null
      }
    ]
  },

  {
    "id": "uma_puerta_bloqueada",
    "lines": [
      {
        "speaker": "",
        "text": "No tengo ganas de salir todavia.",
        "next": null
      }
    ]
  },

  {
    "id": "uma_puerta_con_sombrero",
    "lines": [
      {
        "speaker": "",
        "text": "El sombrero esta puesto. Listo.",
        "next": null
      }
    ]
  },

  {
    "id": "uma_ventana_sin_detonante",
    "lines": [
      {
        "speaker": "",
        "text": "La calle esta soleada. Hace calor.",
        "next": null
      }
    ]
  },

  {
    "id": "uma_hermano_segundo_encuentro",
    "lines": [
      {
        "speaker": "Hermano",
        "text": "Todavia aca.",
        "next": null
      }
    ]
  }

]
```

---

### Barrio: Dialogos del Capitulo 1

```json
[

  {
    "id": "barrio_entrada",
    "lines": [
      {
        "speaker": "",
        "text": "El sol esta fuerte. Menos mal que traje el sombrero.",
        "next": "barrio_entrada_02"
      }
    ]
  },
  {
    "id": "barrio_entrada_02",
    "lines": [
      {
        "speaker": "",
        "text": "Hay sombra en la vereda si camino por debajo de los arboles.",
        "next": null
      }
    ]
  },

  {
    "id": "npc_señora_balcon",
    "lines": [
      {
        "speaker": "Señora del balcon",
        "text": "Hola.",
        "next": "npc_señora_balcon_02"
      }
    ]
  },
  {
    "id": "npc_señora_balcon_02",
    "lines": [
      {
        "speaker": "Señora del balcon",
        "text": "La veo seguido mirando desde adentro. Hoy decidio bajar, que bien.",
        "next": null
      }
    ]
  },

  {
    "id": "npc_kiosquero_radio",
    "lines": [
      {
        "speaker": "",
        "text": "La radio del kiosco suena fuerte. Es una cancion que reconozco de alguna parte.",
        "next": "npc_kiosquero_radio_02"
      }
    ]
  },
  {
    "id": "npc_kiosquero_radio_02",
    "lines": [
      {
        "speaker": "",
        "text": "No puedo recordar de donde la conozco.",
        "next": null
      }
    ]
  },

  {
    "id": "npc_kiosquero_radio_cortada",
    "lines": [
      {
        "speaker": "",
        "text": "La radio se apago de golpe cuando llego un cliente.",
        "next": "npc_kiosquero_radio_cortada_02"
      }
    ]
  },
  {
    "id": "npc_kiosquero_radio_cortada_02",
    "lines": [
      {
        "speaker": "",
        "text": "Justo cuando iba a recordar.",
        "next": null
      }
    ]
  },

  {
    "id": "npc_kiosquero_dialogo",
    "lines": [
      {
        "speaker": "Kiosquero",
        "text": "Buenas.",
        "next": null
      }
    ]
  },

  {
    "id": "npc_chicos_cartas",
    "lines": [
      {
        "speaker": "Chico 1",
        "text": "Esta es la carta del que encuentra cosas.",
        "next": "npc_chicos_cartas_02"
      }
    ]
  },
  {
    "id": "npc_chicos_cartas_02",
    "lines": [
      {
        "speaker": "Chico 1",
        "text": "No vale nada en el juego, pero me gusto.",
        "next": "npc_chicos_cartas_03"
      }
    ]
  },
  {
    "id": "npc_chicos_cartas_03",
    "lines": [
      {
        "speaker": "Chico 2",
        "text": "Seguimos jugando.",
        "next": null
      }
    ]
  },

  {
    "id": "npc_anciano_banco",
    "lines": [
      {
        "speaker": "Señor",
        "text": "Vas a encontrar lo que buscas.",
        "next": "npc_anciano_banco_02"
      }
    ]
  },
  {
    "id": "npc_anciano_banco_02",
    "lines": [
      {
        "speaker": "Señor",
        "text": "No se que es. Pero lo vas a encontrar.",
        "next": null
      }
    ]
  },

  {
    "id": "tienda_cartel",
    "lines": [
      {
        "speaker": "",
        "text": "La persiana esta baja. Hay un cartel: Vuelvo en un rato.",
        "next": "tienda_cartel_02"
      }
    ]
  },
  {
    "id": "tienda_cartel_02",
    "lines": [
      {
        "speaker": "",
        "text": "Al costado del cartel hay una foto polaroid.",
        "next": "tienda_foto_decision"
      }
    ]
  },
  {
    "id": "tienda_foto_decision",
    "lines": [
      {
        "speaker": "",
        "text": "Dos personas en un parque. No se ven las caras. Una tiene sombrero.",
        "choices": [
          {
            "text": "Llevarse la foto",
            "next": "tienda_foto_llevada"
          },
          {
            "text": "Dejarla",
            "next": "tienda_foto_dejada"
          }
        ]
      }
    ]
  },
  {
    "id": "tienda_foto_llevada",
    "lines": [
      {
        "speaker": "",
        "text": "No se quienes son. Pero la guardo.",
        "next": null
      }
    ]
  },
  {
    "id": "tienda_foto_dejada",
    "lines": [
      {
        "speaker": "",
        "text": "La miro una ultima vez. La dejo donde estaba.",
        "next": null
      }
    ]
  },

  {
    "id": "uma_fragmento_disco",
    "lines": [
      {
        "speaker": "",
        "text": "En el suelo. Un pedazo de tapa de disco.",
        "next": "uma_fragmento_disco_02"
      }
    ]
  },
  {
    "id": "uma_fragmento_disco_02",
    "lines": [
      {
        "speaker": "",
        "text": "Esta roto. Se ve parte de un dibujo y las primeras letras de un nombre. No se puede leer completo.",
        "next": "uma_fragmento_disco_03"
      }
    ]
  },
  {
    "id": "uma_fragmento_disco_03",
    "lines": [
      {
        "speaker": "",
        "text": "Lo recojo igual.",
        "next": null
      }
    ]
  },

  {
    "id": "uma_sombra_arbol",
    "lines": [
      {
        "speaker": "",
        "text": "Aca hace mas fresco.",
        "next": null
      }
    ]
  },

  {
    "id": "uma_sol_directo",
    "lines": [
      {
        "speaker": "",
        "text": "El sol me molesta incluso con el sombrero puesto.",
        "next": null
      }
    ]
  }

]
```

---

### Plaza: Dialogos del Capitulo 2

```json
[

  {
    "id": "plaza_entrada",
    "lines": [
      {
        "speaker": "",
        "text": "La plaza. Espacio abierto.",
        "next": "plaza_entrada_02"
      }
    ]
  },
  {
    "id": "plaza_entrada_02",
    "lines": [
      {
        "speaker": "",
        "text": "Sin mucha sombra. Me quedo por los bordes por ahora.",
        "next": null
      }
    ]
  },

  {
    "id": "plaza_nublado",
    "lines": [
      {
        "speaker": "",
        "text": "Las nubes llegaron despacio. La luz cambio.",
        "next": "plaza_nublado_02"
      }
    ]
  },
  {
    "id": "plaza_nublado_02",
    "lines": [
      {
        "speaker": "",
        "text": "Mejor.",
        "next": null
      }
    ]
  },

  {
    "id": "plaza_lluvia",
    "lines": [
      {
        "speaker": "",
        "text": "Empieza a llover. No fuerte. Una llovizna.",
        "next": "plaza_lluvia_02"
      }
    ]
  },
  {
    "id": "plaza_lluvia_02",
    "lines": [
      {
        "speaker": "",
        "text": "Me quedo igual.",
        "next": null
      }
    ]
  },

  {
    "id": "npc_lectora_plaza",
    "lines": [
      {
        "speaker": "Lectora",
        "text": "Cuando llueve, todo suena mas lindo.",
        "next": null
      }
    ]
  },

  {
    "id": "npc_señor_palomas",
    "lines": [
      {
        "speaker": "Señor",
        "text": "Se fueron. Pero van a volver.",
        "next": null
      }
    ]
  },

  {
    "id": "npc_adolescentes_musica",
    "lines": [
      {
        "speaker": "",
        "text": "Estan escuchando musica. Esa melodia...",
        "next": "npc_adolescentes_musica_02"
      }
    ]
  },
  {
    "id": "npc_adolescentes_musica_02",
    "lines": [
      {
        "speaker": "",
        "text": "Es la misma del kiosco. O muy parecida. En una version diferente.",
        "next": null
      }
    ]
  },

  {
    "id": "uma_encuentra_disco",
    "lines": [
      {
        "speaker": "",
        "text": "En el borde de la fuente. Un disco de vinilo.",
        "next": "uma_encuentra_disco_02"
      }
    ]
  },
  {
    "id": "uma_encuentra_disco_02",
    "lines": [
      {
        "speaker": "",
        "text": "Esta mojado pero bien. El nombre del artista coincide con el fragmento de tapa que encontre antes.",
        "next": "uma_encuentra_disco_03"
      }
    ]
  },
  {
    "id": "uma_encuentra_disco_03",
    "lines": [
      {
        "speaker": "",
        "text": "Ahora puedo leerlo completo.",
        "next": null
      }
    ]
  },

  {
    "id": "uma_choice_banco",
    "lines": [
      {
        "speaker": "",
        "text": "El banco esta libre.",
        "choices": [
          {
            "text": "Sentarse un rato",
            "next": "uma_se_sienta_banco"
          },
          {
            "text": "Seguir caminando",
            "next": null
          }
        ]
      }
    ]
  },
  {
    "id": "uma_se_sienta_banco",
    "lines": [
      {
        "speaker": "",
        "text": "La lluvia suena bien desde aca.",
        "next": "uma_banco_espera"
      }
    ]
  },
  {
    "id": "uma_banco_espera",
    "lines": [
      {
        "speaker": "",
        "text": "Puedo quedarme un rato.",
        "choices": [
          {
            "text": "Sacar el cuaderno y dibujar",
            "next": "uma_dibuja_plaza"
          },
          {
            "text": "Solo mirar",
            "next": "uma_mira_plaza"
          }
        ]
      }
    ]
  },
  {
    "id": "uma_dibuja_plaza",
    "lines": [
      {
        "speaker": "",
        "text": "Dibujo la plaza. Rapido, sin pensar demasiado.",
        "next": null
      }
    ]
  },
  {
    "id": "uma_mira_plaza",
    "lines": [
      {
        "speaker": "",
        "text": "La plaza en lluvia. Sin sol. Esta bien.",
        "next": null
      }
    ]
  }

]
```

---

### Zona Intermedia: Dialogos del Capitulo 3

```json
[

  {
    "id": "intermedia_entrada",
    "lines": [
      {
        "speaker": "",
        "text": "Menos gente. Mas silencio.",
        "next": null
      }
    ]
  },

  {
    "id": "uma_planta_sin_maceta",
    "lines": [
      {
        "speaker": "",
        "text": "Una planta en el suelo. Sin maceta.",
        "next": "uma_planta_sin_maceta_02"
      }
    ]
  },
  {
    "id": "uma_planta_sin_maceta_02",
    "lines": [
      {
        "speaker": "",
        "text": "Es igual a la de la señora del balcon.",
        "next": null
      }
    ]
  },

  {
    "id": "uma_radio_nadie",
    "lines": [
      {
        "speaker": "",
        "text": "Una radio. Igual a la del kiosco.",
        "next": "uma_radio_nadie_02"
      }
    ]
  },
  {
    "id": "uma_radio_nadie_02",
    "lines": [
      {
        "speaker": "",
        "text": "No hay nadie que la atienda. Apagada.",
        "next": null
      }
    ]
  },

  {
    "id": "uma_banco_repetido",
    "lines": [
      {
        "speaker": "",
        "text": "El mismo banco que el de la plaza.",
        "next": "uma_banco_repetido_02"
      }
    ]
  },
  {
    "id": "uma_banco_repetido_02",
    "lines": [
      {
        "speaker": "",
        "text": "No puede ser el mismo, pero es igual.",
        "next": null
      }
    ]
  },

  {
    "id": "puerta_voz_madre",
    "lines": [
      {
        "speaker": "",
        "text": "Hay una puerta entreabierta. Adentro, la voz de mama.",
        "next": "puerta_voz_madre_frase"
      }
    ]
  },
  {
    "id": "puerta_voz_madre_frase",
    "lines": [
      {
        "speaker": "Voz de Mama",
        "text": "Hay cosas que no se entienden hasta que las sentis.",
        "next": "puerta_voz_madre_cierre"
      }
    ]
  },
  {
    "id": "puerta_voz_madre_cierre",
    "lines": [
      {
        "speaker": "",
        "text": "La puerta se cierra. No hay nadie.",
        "next": null
      }
    ]
  },

  {
    "id": "activador_disco_slot",
    "condition": "has_item:disco_vinilo",
    "lines": [
      {
        "speaker": "",
        "text": "Hay una ranura. Es del tamaño exacto del disco.",
        "choices": [
          {
            "text": "Poner el disco",
            "next": "disco_puesto_intermedia"
          },
          {
            "text": "Dejarlo",
            "next": null
          }
        ]
      }
    ]
  },
  {
    "id": "activador_disco_slot",
    "condition_fail": "activador_disco_slot_sin_disco",
    "lines": []
  },
  {
    "id": "activador_disco_slot_sin_disco",
    "lines": [
      {
        "speaker": "",
        "text": "Una ranura. No tengo nada que encaje ahi.",
        "next": null
      }
    ]
  },
  {
    "id": "disco_puesto_intermedia",
    "lines": [
      {
        "speaker": "",
        "text": "La musica empieza. Solo un fragmento. Pero ahora recuerdo: ya la escuche antes.",
        "next": null
      }
    ]
  },

  {
    "id": "activador_sombrero",
    "condition": "has_hat",
    "lines": [
      {
        "speaker": "",
        "text": "Un gancho en la pared.",
        "choices": [
          {
            "text": "Colgar el sombrero",
            "next": "sombrero_colgado"
          },
          {
            "text": "No",
            "next": null
          }
        ]
      }
    ]
  },
  {
    "id": "sombrero_colgado",
    "lines": [
      {
        "speaker": "",
        "text": "Lo cuelgo. Primera vez que estoy sin el en todo el dia.",
        "next": "sombrero_colgado_02"
      }
    ]
  },
  {
    "id": "sombrero_colgado_02",
    "lines": [
      {
        "speaker": "",
        "text": "La luz no me molesta tanto como esperaba.",
        "next": null
      }
    ]
  },

  {
    "id": "activador_foto_marco",
    "condition": "has_item:foto_polaroid",
    "lines": [
      {
        "speaker": "",
        "text": "Un marco vacio en la pared.",
        "choices": [
          {
            "text": "Poner la foto polaroid",
            "next": "foto_en_marco"
          },
          {
            "text": "Dejarlo vacio",
            "next": null
          }
        ]
      }
    ]
  },
  {
    "id": "activador_foto_marco",
    "condition_fail": "activador_foto_marco_vacio",
    "lines": []
  },
  {
    "id": "activador_foto_marco_vacio",
    "lines": [
      {
        "speaker": "",
        "text": "Un marco vacio. No tengo nada para ponerle.",
        "next": null
      }
    ]
  },
  {
    "id": "foto_en_marco",
    "lines": [
      {
        "speaker": "",
        "text": "La pongo en el marco.",
        "next": "foto_en_marco_02"
      }
    ]
  },
  {
    "id": "foto_en_marco_02",
    "lines": [
      {
        "speaker": "",
        "text": "No se quienes son. Pero se ven contentos.",
        "next": null
      }
    ]
  },

  {
    "id": "pasaje_final_intermedia",
    "lines": [
      {
        "speaker": "",
        "text": "Un pasaje al fondo. Mas ancho que el resto.",
        "next": null
      }
    ]
  }

]
```

---

### Antesala: Dialogos del Capitulo 4

```json
[

  {
    "id": "antesala_entrada",
    "lines": [
      {
        "speaker": "",
        "text": "Un patio. Cielo abierto. La lluvia sigue.",
        "next": null
      }
    ]
  },

  {
    "id": "antesala_farola",
    "lines": [
      {
        "speaker": "",
        "text": "Una farola apagada.",
        "next": "antesala_farola_02"
      }
    ]
  },
  {
    "id": "antesala_farola_02",
    "lines": [
      {
        "speaker": "",
        "text": "Miro hacia arriba. Las nubes son muy bajas.",
        "next": null
      }
    ]
  },

  {
    "id": "antesala_nota_padre",
    "lines": [
      {
        "speaker": "",
        "text": "Hay una nota en el banco. La letra de Papa.",
        "next": "antesala_nota_padre_texto"
      }
    ]
  },
  {
    "id": "antesala_nota_padre_texto",
    "lines": [
      {
        "speaker": "Nota",
        "text": "A veces, llegar no es lo dificil. Es animarse a seguir.",
        "next": "antesala_nota_comparacion"
      }
    ]
  },
  {
    "id": "antesala_nota_comparacion",
    "condition": "has_item:nota_padre_1",
    "lines": [
      {
        "speaker": "",
        "text": "Saco la primera nota. Las comparo. Misma letra. Mismo papel.",
        "next": "antesala_nota_comparacion_02"
      }
    ]
  },
  {
    "id": "antesala_nota_comparacion",
    "condition_fail": "antesala_nota_guardado",
    "lines": []
  },
  {
    "id": "antesala_nota_comparacion_02",
    "lines": [
      {
        "speaker": "",
        "text": "No se si las escribio el mismo dia o en momentos distintos. El resultado es el mismo.",
        "choices": [
          {
            "text": "Guardar esta nota tambien",
            "next": "antesala_nota_guardada"
          },
          {
            "text": "Dejarla en el banco",
            "next": null
          }
        ]
      }
    ]
  },
  {
    "id": "antesala_nota_guardado",
    "lines": [
      {
        "speaker": "",
        "text": "La leo. La guardo.",
        "choices": [
          {
            "text": "Guardar",
            "next": "antesala_nota_guardada"
          },
          {
            "text": "Dejarla",
            "next": null
          }
        ]
      }
    ]
  },
  {
    "id": "antesala_nota_guardada",
    "lines": [
      {
        "speaker": "",
        "text": "La doblo y la guardo junto a la otra.",
        "next": null
      }
    ]
  },

  {
    "id": "antesala_puerta",
    "lines": [
      {
        "speaker": "",
        "text": "La puerta del fondo. Adentro hay algo.",
        "next": null
      }
    ]
  },

  {
    "id": "antesala_duda",
    "lines": [
      {
        "speaker": "",
        "text": "Ya llegue hasta aca.",
        "next": null
      }
    ]
  }

]
```

---

### Climax y Final: Dialogos del Capitulo 5

```json
[

  {
    "id": "climax_entrada",
    "lines": [
      {
        "speaker": "",
        "text": "Un espacio grande. Con techo pero abierto de un lado.",
        "next": "climax_entrada_02"
      }
    ]
  },
  {
    "id": "climax_entrada_02",
    "lines": [
      {
        "speaker": "",
        "text": "La lluvia se escucha pero no llega hasta aca.",
        "next": null
      }
    ]
  },

  {
    "id": "climax_radio_inspeccion",
    "lines": [
      {
        "speaker": "",
        "text": "Una radio antigua. Tiene un lugar para el disco.",
        "choices": [
          {
            "text": "Poner el disco",
            "next": "climax_radio_con_disco"
          },
          {
            "text": "Dejarla",
            "next": "climax_radio_sin_disco"
          }
        ]
      }
    ]
  },
  {
    "id": "climax_radio_inspeccion",
    "condition": "not_has_item:disco_vinilo",
    "condition_fail": "climax_radio_sin_disco_vacio",
    "lines": []
  },
  {
    "id": "climax_radio_sin_disco_vacio",
    "lines": [
      {
        "speaker": "",
        "text": "Una radio. No tengo nada para ponerle.",
        "next": null
      }
    ]
  },
  {
    "id": "climax_radio_con_disco",
    "lines": [
      {
        "speaker": "",
        "text": "El disco entra. La radio lo acepta.",
        "next": "climax_radio_suena"
      }
    ]
  },
  {
    "id": "climax_radio_suena",
    "lines": [
      {
        "speaker": "",
        "text": "La cancion empieza. Es la misma. Completa. Ahora la escucho entera.",
        "next": null
      }
    ]
  },
  {
    "id": "climax_radio_sin_disco",
    "lines": [
      {
        "speaker": "",
        "text": "La dejo.",
        "next": null
      }
    ]
  },

  {
    "id": "pablo_final_disco",
    "lines": [
      {
        "speaker": "Pablo",
        "text": "Sabia que ibas a reconocer esa cancion.",
        "next": "pablo_final_comun"
      }
    ]
  },
  {
    "id": "pablo_final_banco",
    "lines": [
      {
        "speaker": "Pablo",
        "text": "Vi cuando te quedaste en la plaza aunque estaba lloviendo.",
        "next": "pablo_final_comun"
      }
    ]
  },
  {
    "id": "pablo_final_nota",
    "lines": [
      {
        "speaker": "Pablo",
        "text": "Tu viejo tiene razon. No es lo mismo llegar que animarse a seguir.",
        "next": "pablo_final_comun"
      }
    ]
  },
  {
    "id": "pablo_final_base",
    "lines": [
      {
        "speaker": "Pablo",
        "text": "Mucho tiempo esperando que salieras. Me alegra que lo hayas hecho.",
        "next": "pablo_final_comun"
      }
    ]
  },
  {
    "id": "pablo_final_comun",
    "lines": [
      {
        "speaker": "Pablo",
        "text": "No tenia apuro. Solo esperaba el momento.",
        "next": "uma_respuesta_final"
      }
    ]
  },

  {
    "id": "uma_respuesta_final",
    "lines": [
      {
        "speaker": "",
        "text": "Que respondo.",
        "choices": [
          {
            "text": "Tarde mucho, lo se.",
            "next": "uma_respuesta_ironica"
          },
          {
            "text": "Tambien me alegra.",
            "next": "uma_respuesta_directa"
          },
          {
            "text": "[silencio]",
            "next": "uma_respuesta_silencio"
          }
        ]
      }
    ]
  },
  {
    "id": "uma_respuesta_ironica",
    "lines": [
      {
        "speaker": "Uma",
        "text": "Tarde mucho, lo se.",
        "next": "pablo_cierre"
      }
    ]
  },
  {
    "id": "uma_respuesta_directa",
    "lines": [
      {
        "speaker": "Uma",
        "text": "Tambien me alegra.",
        "next": "pablo_cierre"
      }
    ]
  },
  {
    "id": "uma_respuesta_silencio",
    "lines": [
      {
        "speaker": "",
        "text": "Uma no dice nada. Se queda.",
        "next": "pablo_cierre"
      }
    ]
  },
  {
    "id": "pablo_cierre",
    "lines": [
      {
        "speaker": "Pablo",
        "text": "La lluvia sigue.",
        "next": null
      }
    ]
  }

]
```

---

## 12. Notas de Implementacion Final

### Configuracion del Proyecto en Godot

En Proyecto > Configuracion del Proyecto aplicar lo siguiente:

- Resolucion base: 320 x 180 (pixel art, escala x4 a 1280x720)
  - O bien: 480 x 270 con escala x3
- Modo stretch: canvas_items
- Filtrado de texturas: Nearest (para pixel art nítido)
- Ancho de tile recomendado: 16px con escala x2 en el engine
- Fisica 2D: Layer 1 para Uma, Layer 2 para NPCs, Layer 3 para triggers

Acciones de input a registrar en InputMap:

| Accion | Tecla por defecto |
|--------|------------------|
| ui_up | W / Flecha arriba |
| ui_down | S / Flecha abajo |
| ui_left | A / Flecha izquierda |
| ui_right | D / Flecha derecha |
| interact | E / Z / Enter |
| ui_accept | Enter / Espacio |
| ui_cancel | Escape |

### Orden de Desarrollo Recomendado

El orden sugerido para no bloquear el avance del proyecto es el siguiente.

Primero: sistemas base. Implementar GameManager, MusicManager y DialogData como autoloads. Verificar que el sistema de guardado y carga funciona antes de tocar escenas.

Segundo: Uma y controles. Crear Uma.tscn con sprite placeholder, movimiento, collision y area de interaccion. Verificar el movimiento en todas las direcciones.

Tercero: DialogBox. Implementar el sistema de dialogo con tipeo, choices y condiciones. Probarlo con dialogos de prueba antes de cargar el JSON real.

Cuarto: escenas en orden narrativo. Construir Casa primero, luego Barrio, luego Plaza. Cada escena debe funcionar de punta a punta antes de pasar a la siguiente.

Quinto: sistema de clima. Integrarlo en Plaza donde el cambio climatico es narrativamente central.

Sexto: Zona Intermedia, Antesala y Climax. Estas zonas son las mas cortas y las mas simbolicas. Se construyen ultimas.

Septimo: polish. Musica, particulas de lluvia, transiciones, animaciones de Uma, overlays de clima.

### Checklist de Escena por Escena

Cada escena se considera completa cuando cumple lo siguiente:

- TileMaps con capas correctas (suelo, deco fondo, frente, techo)
- Uma instanciada con posicion inicial y spawn point
- Todos los NPCs instanciados con dialog_id asignado
- Todos los pickup items instanciados con item_id asignado
- Todos los triggers conectados a sus funciones
- Transicion de entrada (fade in) y salida (fade out) funcionando
- Clima correcto al inicio de la escena
- Musica correcta al inicio de la escena
- Guardado automatico al salir de la escena

### Paleta de Colores por Estado Climatico

Estado soleado:
- Overlay: rgba(255, 230, 128, 20)
- Ambiente: calido, contrastes altos
- Sombras: marcadas, azuladas

Estado nublado:
- Overlay: rgba(200, 215, 240, 30)
- Ambiente: frio suave, contrastes bajos
- Sombras: difusas

Estado lluvia:
- Overlay: rgba(150, 180, 230, 38)
- Ambiente: frio, azulado
- Particulas: blancas semitransparentes, angulo 10-15 grados

### Estructura de Items en items.json

```json
[
  {
    "id": "fragmento_disco",
    "name": "Fragmento de tapa",
    "description_base": "La mitad de una tapa de disco. Se ve parte de un dibujo y el principio de un nombre.",
    "description_updated": "Ahora que tengo el disco completo puedo ver que es el mismo artista.",
    "icon": "res://assets/sprites/objects/fragmento_disco.png"
  },
  {
    "id": "disco_vinilo",
    "name": "Disco de vinilo",
    "description_base": "Un disco encontrado en la fuente. Esta mojado pero en buen estado.",
    "description_updated": "La cancion que tocaba en el kiosco y en el telefono de los adolescentes. Era este disco.",
    "icon": "res://assets/sprites/objects/disco_vinilo.png"
  },
  {
    "id": "foto_polaroid",
    "name": "Foto polaroid",
    "description_base": "Dos personas en un parque. No se ven las caras. Una tiene sombrero.",
    "description_updated": "La puse en el marco de la zona del pasaje.",
    "icon": "res://assets/sprites/objects/foto_polaroid.png"
  },
  {
    "id": "nota_padre_1",
    "name": "Nota de Papa",
    "description_base": "Si ves algo que te llama la atencion, segui por ahi.",
    "description_updated": "Hay una segunda nota igual. Misma letra, mismo papel.",
    "icon": "res://assets/sprites/objects/nota_papel.png"
  },
  {
    "id": "nota_padre_2",
    "name": "Segunda nota de Papa",
    "description_base": "A veces, llegar no es lo dificil. Es animarse a seguir.",
    "description_updated": "",
    "icon": "res://assets/sprites/objects/nota_papel.png"
  },
  {
    "id": "boceto_plaza",
    "name": "Boceto de la plaza",
    "description_base": "Un dibujo rapido de la plaza bajo la lluvia. Lo hice yo.",
    "description_updated": "",
    "icon": "res://assets/sprites/objects/boceto.png"
  }
]
```

### Musica: Estructura Narrativa

La cancion central del juego aparece en cinco formas a lo largo del recorrido:

- Fragmento 1 (barrio, radio del kiosco): 8 compases, cortado de golpe
- Fragmento 2 (plaza, telefono de adolescentes): misma melodia en version diferente, completa pero lejana
- Fragmento 3 (zona intermedia, ranura del disco): 16 compases con resolucion parcial
- Fragmento 4 (climax, radio antigua): cancion completa de principio a fin

La musica ambiental de cada zona es diferente pero comparte el mismo motivo melodico principal, transformado segun el clima y el estado emocional de Uma.

### Notas sobre Pablo

Pablo no tiene sprites de accion. Solo necesita:

- idle_down: 2 frames
- idle_side: 2 frames

No se mueve en el espacio del climax. Aparece, se queda, escucha. Su presencia es estatica por diseno.

---

*Fin del documento de desarrollo de HUMITA.*
