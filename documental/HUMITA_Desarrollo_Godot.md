# HUMITA — Documento de Desarrollo Completo en Godot
### Versión 2.0 | Integrado con Historia Extendida

---

> *"El recorrido emocional tiene prioridad sobre la dificultad."*

---

## ÍNDICE

1. [Configuración del Proyecto](#1-configuración-del-proyecto)
2. [Estructura General del Proyecto](#2-estructura-general-del-proyecto)
3. [Autoloads y Singletons](#3-autoloads-y-singletons)
4. [Escenas UI: Menú Principal y HUD](#4-escenas-ui-menú-principal-y-hud)
5. [Escenas: Personaje Uma](#5-escenas-personaje-uma)
6. [Escenas: NPCs y Pablo](#6-escenas-npcs-y-pablo)
7. [Escenas: Objetos del Mundo](#7-escenas-objetos-del-mundo)
8. [Sistema de Diálogos](#8-sistema-de-diálogos)
9. [Sistema de Clima](#9-sistema-de-clima)
10. [Sistema de Inventario y Cuaderno](#10-sistema-de-inventario-y-cuaderno)
11. [Sistema de Memoria y Flags](#11-sistema-de-memoria-y-flags)
12. [Sistema de Música y Resonancia](#12-sistema-de-música-y-resonancia)
13. [Mapas: Estructura y Tilesets](#13-mapas-estructura-y-tilesets)
14. [Scripts de Zona: Completos](#14-scripts-de-zona-completos)
15. [Diálogos Completos en JSON](#15-diálogos-completos-en-json)
16. [Data: Items en JSON](#16-data-items-en-json)
17. [Transiciones y Efectos Visuales](#17-transiciones-y-efectos-visuales)
18. [Notas de Implementación y Checklist](#18-notas-de-implementación-y-checklist)

---

## 1. CONFIGURACIÓN DEL PROYECTO

### 1.1 Ajustes de Proyecto en Godot 4.x

Ir a **Proyecto → Configuración del Proyecto** y aplicar los siguientes valores:

#### Display / Window

```
Viewport Width:  320
Viewport Height: 180
Mode:            Windowed
Resizable:       true
Stretch Mode:    canvas_items
Stretch Aspect:  keep
```

> **Nota:** Esta resolución base (320×180) escala limpiamente a 1280×720 (×4) y a 1920×1080 (×6). Para pixel art más detallado usar 480×270 con escala ×4.

#### Rendering

```
Textures / Default Texture Filter: Nearest
2D / Snap 2D Transforms to Pixel:  true
2D / Snap 2D Vertices to Pixel:    true
```

#### Physics / 2D

Crear los siguientes layers en **Physics → 2D → Layer Names**:

| Layer | Número | Uso |
|---|---|---|
| `world` | 1 | Paredes, suelo, colisiones físicas |
| `uma` | 2 | Cuerpo de Uma |
| `npcs` | 3 | Cuerpos de NPCs |
| `items` | 4 | Objetos recogibles |
| `triggers` | 5 | Áreas de evento (invisibles) |
| `interactables` | 6 | Objetos interactuables (no recogibles) |

#### Input Map — Acciones Registradas

Ir a **Proyecto → Configuración del Proyecto → InputMap** y agregar:

| Acción | Tecla Principal | Tecla Alternativa | Joystick |
|---|---|---|---|
| `move_up` | W | Flecha Arriba | Stick Izq ↑ |
| `move_down` | S | Flecha Abajo | Stick Izq ↓ |
| `move_left` | A | Flecha Izquierda | Stick Izq ← |
| `move_right` | D | Flecha Derecha | Stick Izq → |
| `interact` | E | Z | Botón Sur (A/X) |
| `open_notebook` | Q | Tab | Botón Oeste (X/□) |
| `open_inventory` | I | — | Botón Norte (Y/△) |
| `slow_walk` | Shift | — | Gatillo izquierdo |
| `pause` | Escape | P | Start/Options |
| `ui_accept` | Enter | Espacio | Botón Sur |
| `ui_cancel` | Escape | Backspace | Botón Este (B/○) |

---

## 2. ESTRUCTURA GENERAL DEL PROYECTO

### 2.1 Árbol de Carpetas Completo

```
res://
├── scenes/
│   ├── ui/
│   │   ├── MainMenu.tscn
│   │   ├── PauseMenu.tscn
│   │   ├── HUD.tscn
│   │   ├── DialogBox.tscn
│   │   ├── InventoryPanel.tscn
│   │   ├── Notebook.tscn
│   │   ├── OptionsMenu.tscn
│   │   ├── EndSummary.tscn
│   │   └── TransitionOverlay.tscn
│   ├── world/
│   │   ├── Casa.tscn
│   │   ├── Barrio.tscn
│   │   ├── Plaza.tscn
│   │   ├── ZonaInterior.tscn
│   │   ├── Antesala.tscn
│   │   └── EspacioFinal.tscn
│   ├── characters/
│   │   ├── Uma.tscn
│   │   ├── Pablo.tscn
│   │   └── npcs/
│   │       ├── NPC_Base.tscn
│   │       ├── NPC_Nelida.tscn
│   │       ├── NPC_Elena.tscn
│   │       ├── NPC_Tomas.tscn
│   │       ├── NPC_Charly.tscn
│   │       ├── NPC_Mario.tscn
│   │       ├── NPC_Ramiro.tscn
│   │       ├── NPC_Nico.tscn
│   │       ├── NPC_Valentina.tscn
│   │       ├── NPC_Ernesto.tscn
│   │       └── NPC_Adolescentes.tscn
│   ├── objects/
│   │   ├── PickupItem.tscn
│   │   ├── InspectableObject.tscn
│   │   ├── Radio.tscn
│   │   ├── DiscSlot.tscn
│   │   ├── HatHook.tscn
│   │   ├── PhotoFrame.tscn
│   │   ├── Bench.tscn
│   │   ├── Door.tscn
│   │   └── NoteOnSurface.tscn
│   └── systems/
│       ├── WeatherSystem.tscn
│       ├── RainParticles.tscn
│       └── FloatingText.tscn
├── scripts/
│   ├── ui/
│   │   ├── main_menu.gd
│   │   ├── pause_menu.gd
│   │   ├── hud.gd
│   │   ├── dialog_box.gd
│   │   ├── inventory_panel.gd
│   │   ├── notebook.gd
│   │   ├── options_menu.gd
│   │   └── end_summary.gd
│   ├── characters/
│   │   ├── uma.gd
│   │   ├── pablo.gd
│   │   └── npc_base.gd
│   ├── world/
│   │   ├── base_zone.gd
│   │   ├── casa.gd
│   │   ├── barrio.gd
│   │   ├── plaza.gd
│   │   ├── zona_interior.gd
│   │   ├── antesala.gd
│   │   └── espacio_final.gd
│   ├── objects/
│   │   ├── pickup_item.gd
│   │   ├── inspectable_object.gd
│   │   ├── radio.gd
│   │   ├── disc_slot.gd
│   │   ├── hat_hook.gd
│   │   ├── photo_frame.gd
│   │   ├── bench.gd
│   │   └── note_on_surface.gd
│   └── systems/
│       ├── game_manager.gd        ← Autoload
│       ├── flags_manager.gd       ← Autoload
│       ├── music_manager.gd       ← Autoload
│       ├── dialog_data.gd         ← Autoload
│       ├── item_data.gd           ← Autoload
│       ├── weather_system.gd
│       └── floating_text.gd
├── assets/
│   ├── sprites/
│   │   ├── uma/
│   │   │   ├── uma_idle_down.png     (2 frames)
│   │   │   ├── uma_idle_up.png       (2 frames)
│   │   │   ├── uma_idle_side.png     (2 frames)
│   │   │   ├── uma_walk_down.png     (4 frames)
│   │   │   ├── uma_walk_up.png       (4 frames)
│   │   │   ├── uma_walk_side.png     (4 frames)
│   │   │   ├── uma_sit.png           (1 frame)
│   │   │   ├── uma_draw.png          (3 frames)
│   │   │   ├── uma_think.png         (2 frames)
│   │   │   └── uma_hat.png           (separado, overlay)
│   │   ├── pablo/
│   │   │   ├── pablo_idle_down.png
│   │   │   └── pablo_idle_side.png
│   │   ├── npcs/
│   │   │   ├── nelida_idle.png
│   │   │   ├── elena_idle.png
│   │   │   ├── tomas_draw.png
│   │   │   ├── charly_idle.png
│   │   │   ├── mario_idle.png
│   │   │   ├── ramiro_idle.png
│   │   │   ├── nico_idle.png
│   │   │   ├── valentina_idle.png
│   │   │   ├── ernesto_idle.png
│   │   │   └── adolescentes_idle.png
│   │   ├── objects/
│   │   │   ├── fragmento_disco.png
│   │   │   ├── disco_vinilo.png
│   │   │   ├── foto_polaroid.png
│   │   │   ├── nota_papel.png
│   │   │   ├── boceto.png
│   │   │   ├── radio_antigua.png
│   │   │   ├── sombrero.png
│   │   │   └── cuaderno.png
│   │   ├── ui/
│   │   │   ├── dialog_panel.png
│   │   │   ├── inventory_slot.png
│   │   │   ├── notebook_page.png
│   │   │   ├── continue_arrow.png
│   │   │   └── icons/
│   │   │       ├── icon_sol.png
│   │   │       ├── icon_nube.png
│   │   │       └── icon_lluvia.png
│   │   └── tilesets/
│   │       ├── tileset_exterior.png
│   │       ├── tileset_interior.png
│   │       └── tileset_objetos.png
│   ├── audio/
│   │   ├── music/
│   │   │   ├── menu_theme.ogg
│   │   │   ├── casa_ambient.ogg
│   │   │   ├── barrio_sunny.ogg
│   │   │   ├── plaza_sunny.ogg
│   │   │   ├── plaza_cloudy.ogg
│   │   │   ├── plaza_rain.ogg
│   │   │   ├── zona_interior.ogg
│   │   │   ├── antesala.ogg
│   │   │   ├── espacio_final_ambient.ogg
│   │   │   ├── lavuelta_frag1_guitarra.ogg
│   │   │   ├── lavuelta_frag2_kiosco.ogg
│   │   │   ├── lavuelta_frag3_adolescentes.ogg
│   │   │   ├── lavuelta_frag4_intermedia.ogg
│   │   │   └── lavuelta_completa.ogg
│   │   └── sfx/
│   │       ├── rain_loop.ogg
│   │       ├── wind_soft.ogg
│   │       ├── footsteps_vereda.ogg
│   │       ├── footsteps_cesped.ogg
│   │       ├── footsteps_interior.ogg
│   │       ├── item_pickup.ogg
│   │       ├── dialog_blip.ogg
│   │       ├── door_open.ogg
│   │       ├── notebook_open.ogg
│   │       └── radio_static.ogg
│   └── fonts/
│       ├── fuente_principal.ttf       (texto de diálogo)
│       ├── fuente_monologos.ttf       (pensamientos de Uma, cursiva)
│       ├── fuente_titulos.ttf         (UI, títulos)
│       └── fuente_notas.ttf           (notas escritas a mano)
└── data/
    ├── dialogos.json
    ├── items.json
    └── sketches.json
```

---

## 3. AUTOLOADS Y SINGLETONS

Registrar en **Proyecto → Configuración del Proyecto → Autoload**:

| Nombre Global | Ruta del Script | Orden |
|---|---|---|
| `GameManager` | `res://scripts/systems/game_manager.gd` | 1 |
| `FlagsManager` | `res://scripts/systems/flags_manager.gd` | 2 |
| `MusicManager` | `res://scripts/systems/music_manager.gd` | 3 |
| `DialogData` | `res://scripts/systems/dialog_data.gd` | 4 |
| `ItemData` | `res://scripts/systems/item_data.gd` | 5 |

---

### 3.1 `game_manager.gd` — Singleton Principal

```gdscript
# res://scripts/systems/game_manager.gd
extends Node

# ─────────────────────────────────────────────
# ESTADO DE ESCENA
# ─────────────────────────────────────────────
var current_scene_path: String = "res://scenes/world/Casa.tscn"
var uma_node: CharacterBody2D = null

# ─────────────────────────────────────────────
# INVENTARIO
# ─────────────────────────────────────────────
const MAX_INVENTORY: int = 6
var inventory: Array[String] = []
var notebook_sketches: Array[Dictionary] = []

# Estado del sombrero (fuera del inventario, es identitario)
var uma_has_hat: bool = true

# ─────────────────────────────────────────────
# FRAGMENTOS DE MELODÍA ESCUCHADOS
# ─────────────────────────────────────────────
var melody_fragments_heard: Array[String] = []

# ─────────────────────────────────────────────
# TIEMPO DE JUEGO POR ZONA (para estadísticas finales)
# ─────────────────────────────────────────────
var zone_times: Dictionary = {
	"casa": 0.0,
	"barrio": 0.0,
	"plaza": 0.0,
	"zona_interior": 0.0,
	"antesala": 0.0,
	"espacio_final": 0.0
}
var _current_zone_key: String = ""
var _zone_timer: float = 0.0

# ─────────────────────────────────────────────
# SEÑALES
# ─────────────────────────────────────────────
signal inventory_changed(inventory: Array)
signal hat_state_changed(has_hat: bool)
signal melody_fragment_heard(fragment_id: String)
signal scene_changing(from: String, to: String)

# ─────────────────────────────────────────────
# CICLO
# ─────────────────────────────────────────────
func _process(delta: float) -> void:
	if _current_zone_key != "":
		_zone_timer += delta
		zone_times[_current_zone_key] = _zone_timer

# ─────────────────────────────────────────────
# INVENTARIO
# ─────────────────────────────────────────────
func add_item(item_id: String) -> bool:
	if inventory.size() >= MAX_INVENTORY:
		return false
	if inventory.has(item_id):
		return false
	inventory.append(item_id)
	inventory_changed.emit(inventory)
	return true

func has_item(item_id: String) -> bool:
	return inventory.has(item_id)

func remove_item(item_id: String) -> void:
	inventory.erase(item_id)
	inventory_changed.emit(inventory)

func get_inventory_count() -> int:
	return inventory.size()

# ─────────────────────────────────────────────
# SOMBRERO
# ─────────────────────────────────────────────
func set_hat(value: bool) -> void:
	uma_has_hat = value
	hat_state_changed.emit(value)
	if uma_node:
		if value:
			uma_node.put_hat_on()
		else:
			uma_node.take_hat_off()

# ─────────────────────────────────────────────
# FRAGMENTOS DE MELODÍA
# ─────────────────────────────────────────────
func register_melody_fragment(fragment_id: String) -> void:
	if not melody_fragments_heard.has(fragment_id):
		melody_fragments_heard.append(fragment_id)
		melody_fragment_heard.emit(fragment_id)

func heard_fragment(fragment_id: String) -> bool:
	return melody_fragments_heard.has(fragment_id)

func get_melody_completeness() -> float:
	# Retorna 0.0–1.0 según cuántos fragmentos se escucharon
	var total_fragments: int = 4
	return float(melody_fragments_heard.size()) / float(total_fragments)

# ─────────────────────────────────────────────
# CUADERNO
# ─────────────────────────────────────────────
func add_sketch(sketch_data: Dictionary) -> void:
	notebook_sketches.append(sketch_data)

func get_sketches() -> Array[Dictionary]:
	return notebook_sketches

# ─────────────────────────────────────────────
# ZONA Y TIEMPO
# ─────────────────────────────────────────────
func set_current_zone(zone_key: String) -> void:
	_zone_timer = zone_times.get(zone_key, 0.0)
	_current_zone_key = zone_key

# ─────────────────────────────────────────────
# NIVEL DE COMPLETITUD (para determinar tipo de final)
# ─────────────────────────────────────────────
func get_ending_completeness() -> int:
	# Retorna 1 (escueto), 2 (resonante) o 3 (completo)
	var score: int = 0
	var key_flags: Array[String] = [
		"recogió_disco", "tomó_polaroid", "guardó_nota_padre_1",
		"habló_mario", "valentina_nivel_3", "dibujó_plaza",
		"activó_disco_zona_interior", "escuchó_voz_elena",
		"comparó_notas", "leyó_placa_bardi"
	]
	for flag_name in key_flags:
		if FlagsManager.get_flag(flag_name):
			score += 1
	if score >= 7:
		return 3
	elif score >= 4:
		return 2
	return 1

# ─────────────────────────────────────────────
# CAMBIO DE ESCENA
# ─────────────────────────────────────────────
func change_scene(scene_path: String) -> void:
	scene_changing.emit(current_scene_path, scene_path)
	current_scene_path = scene_path
	get_tree().change_scene_to_file(scene_path)

# ─────────────────────────────────────────────
# GUARDADO Y CARGA
# ─────────────────────────────────────────────
func save_game() -> void:
	var save_data: Dictionary = {
		"current_scene": current_scene_path,
		"inventory": inventory,
		"uma_has_hat": uma_has_hat,
		"flags": FlagsManager.flags,
		"melody_fragments": melody_fragments_heard,
		"notebook_sketches": notebook_sketches,
		"zone_times": zone_times
	}
	var file := FileAccess.open("user://humita_save.json", FileAccess.WRITE)
	if file:
		file.store_string(JSON.stringify(save_data, "\t"))
		file.close()

func load_game() -> bool:
	if not FileAccess.file_exists("user://humita_save.json"):
		return false
	var file := FileAccess.open("user://humita_save.json", FileAccess.READ)
	if not file:
		return false
	var content := file.get_as_text()
	file.close()
	var data = JSON.parse_string(content)
	if data == null:
		return false
	current_scene_path = data.get("current_scene", "res://scenes/world/Casa.tscn")
	inventory = Array(data.get("inventory", []), TYPE_STRING, "", null)
	uma_has_hat = data.get("uma_has_hat", true)
	FlagsManager.flags = data.get("flags", FlagsManager.flags)
	melody_fragments_heard = Array(data.get("melody_fragments", []), TYPE_STRING, "", null)
	notebook_sketches = data.get("notebook_sketches", [])
	zone_times = data.get("zone_times", zone_times)
	return true

func has_save() -> bool:
	return FileAccess.file_exists("user://humita_save.json")

func delete_save() -> void:
	if FileAccess.file_exists("user://humita_save.json"):
		DirAccess.remove_absolute("user://humita_save.json")
```

---

### 3.2 `flags_manager.gd` — Sistema de Flags

```gdscript
# res://scripts/systems/flags_manager.gd
extends Node

# ─────────────────────────────────────────────
# TODOS LOS FLAGS DEL JUEGO
# ─────────────────────────────────────────────
var flags: Dictionary = {

	# ── PRÓLOGO / CASA ────────────────────────
	"detonante_brillo":           false,
	"detonante_melodia":          false,
	"detonante_cuaderno":         false,
	"habló_tomas":                false,
	"tomas_nivel":                0,      # 0, 1, 2 (profundidad de conversación)
	"habló_elena":                false,
	"elena_nivel":                0,
	"guardó_nota_padre_1":        false,
	"inspeccionó_estante":        false,
	"inspeccionó_foto_heladera":  false,
	"inspeccionó_cuarto":         false,
	"salió_casa":                 false,

	# ── ACTO I / BARRIO ───────────────────────
	"nelida_nivel":               0,      # 0, 1, 2, 3
	"charly_escuchó_canción":     false,
	"charly_nivel":               0,
	"habló_mario":                false,
	"habló_ramiro_nico":          false,
	"ramiro_nico_nivel":          0,
	"recogió_fragmento":          false,
	"tomó_polaroid":              false,
	"leyó_placa_bardi":           false,
	"timbró_bardi":               false,
	"acarició_gato":              false,
	"escuchó_radio_ventana":      false,

	# ── ACTO II / PLAZA ───────────────────────
	"fue_al_centro_plaza":        false,
	"recogió_disco":              false,
	"valentina_nivel":            0,      # 0, 1, 2, 3
	"valentina_nivel_3":          false,  # flag específico para completitud
	"habló_ernesto":              false,
	"ernesto_nivel":              0,
	"escuchó_adolescentes":       false,
	"dibujó_plaza":               false,
	"se_sentó_banco_plaza":       false,
	"clima_nublado_activado":     false,
	"clima_lluvia_activado":      false,

	# ── ACTO III / ZONA INTERIOR ──────────────
	"escuchó_voz_elena":          false,
	"activó_disco_zona_interior": false,
	"dejó_sombrero_gancho":       false,
	"recuperó_sombrero":          false,
	"colocó_polaroid_marco":      false,
	"inspeccionó_planta_espejo":  false,
	"inspeccionó_radio_espejo":   false,
	"inspeccionó_banco_espejo":   false,

	# ── ACTO IV / ANTESALA ────────────────────
	"leyó_nota_padre_2":          false,
	"guardó_nota_padre_2":        false,
	"comparó_notas":              false,
	"dibujó_antesala":            false,
	"esperó_sola_antesala":       false,

	# ── ACTO V / ESPACIO FINAL ────────────────
	"puso_disco_radio_final":     false,
	"escuchó_canción_completa":   false,
	"pablo_apareció":             false,
	"respuesta_pablo":            "",     # "ironica", "directa", "silencio"
	"juego_completo":             false
}

# ─────────────────────────────────────────────
# SEÑALES
# ─────────────────────────────────────────────
signal flag_changed(flag_name: String, value)

# ─────────────────────────────────────────────
# MÉTODOS
# ─────────────────────────────────────────────
func set_flag(flag_name: String, value) -> void:
	if not flags.has(flag_name):
		push_warning("FlagsManager: Flag desconocido: '%s'" % flag_name)
		return
	flags[flag_name] = value
	flag_changed.emit(flag_name, value)

func get_flag(flag_name: String):
	if not flags.has(flag_name):
		push_warning("FlagsManager: Flag desconocido: '%s'" % flag_name)
		return null
	return flags[flag_name]

func increment_flag(flag_name: String) -> void:
	if not flags.has(flag_name):
		push_warning("FlagsManager: Flag desconocido: '%s'" % flag_name)
		return
	if flags[flag_name] is int:
		flags[flag_name] += 1
		flag_changed.emit(flag_name, flags[flag_name])
	else:
		push_warning("FlagsManager: El flag '%s' no es entero." % flag_name)

func check_flags(required_flags: Dictionary) -> bool:
	# Comprueba múltiples flags al mismo tiempo
	# required_flags = { "nombre_flag": valor_esperado, ... }
	for flag_name in required_flags.keys():
		if get_flag(flag_name) != required_flags[flag_name]:
			return false
	return true
```

---

### 3.3 `dialog_data.gd` — Cargador de Diálogos

```gdscript
# res://scripts/systems/dialog_data.gd
extends Node

var dialogs: Dictionary = {}

func _ready() -> void:
	_load_dialogs()

func _load_dialogs() -> void:
	var file := FileAccess.open("res://data/dialogos.json", FileAccess.READ)
	if file == null:
		push_error("DialogData: No se pudo abrir dialogos.json")
		return
	var content := file.get_as_text()
	file.close()
	var data = JSON.parse_string(content)
	if data == null:
		push_error("DialogData: JSON inválido en dialogos.json")
		return
	for dialog in data:
		if dialog.has("id"):
			dialogs[dialog["id"]] = dialog
	print("DialogData: %d diálogos cargados." % dialogs.size())

func get_dialog(dialog_id: String) -> Dictionary:
	if dialogs.has(dialog_id):
		return dialogs[dialog_id]
	push_warning("DialogData: Diálogo no encontrado: '%s'" % dialog_id)
	return {}

func has_dialog(dialog_id: String) -> bool:
	return dialogs.has(dialog_id)
```

---

### 3.4 `item_data.gd` — Cargador de Items

```gdscript
# res://scripts/systems/item_data.gd
extends Node

var items: Dictionary = {}

func _ready() -> void:
	_load_items()

func _load_items() -> void:
	var file := FileAccess.open("res://data/items.json", FileAccess.READ)
	if file == null:
		push_error("ItemData: No se pudo abrir items.json")
		return
	var content := file.get_as_text()
	file.close()
	var data = JSON.parse_string(content)
	if data == null:
		push_error("ItemData: JSON inválido en items.json")
		return
	for item in data:
		if item.has("id"):
			items[item["id"]] = item
	print("ItemData: %d items cargados." % items.size())

func get_item(item_id: String) -> Dictionary:
	if items.has(item_id):
		return items[item_id]
	return {}

func get_description(item_id: String) -> String:
	var item := get_item(item_id)
	if item.is_empty():
		return ""
	# Retorna descripción actualizada si está disponible
	var updated := item.get("description_updated", "")
	if updated != "" and _should_show_updated(item_id):
		return updated
	return item.get("description_base", "")

func _should_show_updated(item_id: String) -> bool:
	match item_id:
		"fragmento_disco":
			return GameManager.has_item("disco_vinilo")
		"nota_padre_1":
			return GameManager.has_item("nota_padre_2")
		"disco_vinilo":
			return FlagsManager.get_flag("activó_disco_zona_interior")
		_:
			return false
```

---

## 4. ESCENAS UI: MENÚ PRINCIPAL Y HUD

### 4.1 `MainMenu.tscn` — Árbol de Nodos

```
MainMenu (Node2D)
├── Background (TextureRect)
│   ├── texture: [fondo difuso, paleta cálida-fría]
│   └── stretch_mode: STRETCH_COVER
├── AnimationPlayer  ← anima entrada/salida de elementos
├── TitleContainer (VBoxContainer)
│   ├── anchor_preset: CENTER_TOP
│   ├── TitleLabel (Label)
│   │   ├── text: "HUMITA"
│   │   ├── theme_override_font: fuente_titulos.ttf
│   │   ├── theme_override_font_size: 64
│   │   └── horizontal_alignment: CENTER
│   ├── SubtitleLabel (Label)
│   │   ├── text: "una historia pequeña"
│   │   ├── theme_override_font: fuente_monologos.ttf  (italic)
│   │   ├── theme_override_font_size: 18
│   │   └── horizontal_alignment: CENTER
│   └── Spacer (Control)
│       └── custom_minimum_size: Vector2(0, 48)
├── ButtonContainer (VBoxContainer)
│   ├── anchor_preset: CENTER
│   ├── separation: 12
│   ├── BtnJugar (Button)
│   │   └── text: "Jugar"
│   ├── BtnContinuar (Button)
│   │   ├── text: "Continuar"
│   │   └── visible: false  ← se activa si hay save
│   ├── BtnOpciones (Button)
│   │   └── text: "Opciones"
│   └── BtnSalir (Button)
│       └── text: "Salir"
├── VersionLabel (Label)
│   ├── text: "v2.0"
│   ├── anchor_preset: BOTTOM_RIGHT
│   └── theme_override_font_size: 10
└── TransitionOverlay (ColorRect)
    ├── color: Color(0, 0, 0, 1)  ← empieza en negro, fade in
    ├── anchors_preset: FULL_RECT
    └── mouse_filter: MOUSE_FILTER_IGNORE
```

### 4.2 `HUD.tscn` — Árbol de Nodos

```
HUD (CanvasLayer)
├── layer: 10
├── InteractPrompt (Control)
│   ├── anchor_preset: BOTTOM_CENTER
│   ├── offset_bottom: -24
│   ├── visible: false
│   └── HBoxContainer
│       ├── KeyIcon (TextureRect)
│       │   └── texture: icono_tecla_E.png
│       └── ActionLabel (Label)
│           ├── text: "Interactuar"
│           └── theme_override_font_size: 11
├── InventoryBar (HBoxContainer)
│   ├── anchor_preset: BOTTOM_RIGHT
│   ├── offset_right: -8
│   ├── offset_bottom: -8
│   └── [6× ItemSlotUI instancias]
│       └── ItemSlotUI (PanelContainer)
│           ├── custom_minimum_size: Vector2(20, 20)
│           └── SlotIcon (TextureRect)
│               └── stretch_mode: STRETCH_KEEP_ASPECT_CENTERED
├── ClimateIcon (TextureRect)
│   ├── anchor_preset: TOP_RIGHT
│   ├── offset_right: -8
│   ├── offset_top: 8
│   └── custom_minimum_size: Vector2(20, 20)
├── NotebookHint (Label)
│   ├── anchor_preset: BOTTOM_LEFT
│   ├── offset_left: 8
│   ├── offset_bottom: -8
│   ├── text: "[Q] Cuaderno"
│   ├── theme_override_font_size: 10
│   └── modulate: Color(1, 1, 1, 0.5)
└── ScreenTint (ColorRect)
    ├── anchors_preset: FULL_RECT
    ├── color: Color(0, 0, 0, 0)  ← cambia según clima
    └── mouse_filter: MOUSE_FILTER_IGNORE
```

### 4.3 `main_menu.gd`

```gdscript
# res://scripts/ui/main_menu.gd
extends Node2D

@onready var btn_jugar:     Button    = $ButtonContainer/BtnJugar
@onready var btn_continuar: Button    = $ButtonContainer/BtnContinuar
@onready var btn_opciones:  Button    = $ButtonContainer/BtnOpciones
@onready var btn_salir:     Button    = $ButtonContainer/BtnSalir
@onready var overlay:       ColorRect = $TransitionOverlay
@onready var anim:          AnimationPlayer = $AnimationPlayer

func _ready() -> void:
	btn_continuar.visible = GameManager.has_save()
	btn_jugar.pressed.connect(_on_jugar)
	btn_continuar.pressed.connect(_on_continuar)
	btn_opciones.pressed.connect(_on_opciones)
	btn_salir.pressed.connect(_on_salir)
	MusicManager.play_track("menu_theme")
	# Fade in desde negro
	var tween := create_tween()
	tween.tween_property(overlay, "color:a", 0.0, 1.2)

func _on_jugar() -> void:
	# Nueva partida: resetear todo
	GameManager.delete_save()
	FlagsManager.flags = FlagsManager.flags.duplicate(true)  # reset
	_fade_and_load("res://scenes/world/Casa.tscn")

func _on_continuar() -> void:
	if GameManager.load_game():
		_fade_and_load(GameManager.current_scene_path)

func _on_opciones() -> void:
	var options := preload("res://scenes/ui/OptionsMenu.tscn").instantiate()
	add_child(options)

func _on_salir() -> void:
	get_tree().quit()

func _fade_and_load(scene_path: String) -> void:
	btn_jugar.disabled = true
	btn_continuar.disabled = true
	var tween := create_tween()
	tween.tween_property(overlay, "color:a", 1.0, 0.8)
	await tween.finished
	GameManager.change_scene(scene_path)
```

### 4.4 `hud.gd`

```gdscript
# res://scripts/ui/hud.gd
extends CanvasLayer
class_name HUD

@onready var interact_prompt: Control   = $InteractPrompt
@onready var action_label:    Label     = $InteractPrompt/HBoxContainer/ActionLabel
@onready var climate_icon:    TextureRect = $ClimateIcon
@onready var screen_tint:     ColorRect = $ScreenTint
@onready var inventory_bar:   HBoxContainer = $InventoryBar

@export var icon_sol:    Texture2D
@export var icon_nube:   Texture2D
@export var icon_lluvia: Texture2D

const TINT_SUN:    Color = Color(1.0, 0.90, 0.50, 0.08)
const TINT_CLOUD:  Color = Color(0.78, 0.84, 0.94, 0.12)
const TINT_RAIN:   Color = Color(0.59, 0.71, 0.90, 0.15)
const TINT_NONE:   Color = Color(0, 0, 0, 0)

func _ready() -> void:
	add_to_group("hud")
	GameManager.inventory_changed.connect(_on_inventory_changed)

func set_interact_hint(visible: bool, action_text: String = "Interactuar") -> void:
	interact_prompt.visible = visible
	action_label.text = action_text

func set_climate_visual(state: int) -> void:
	# 0=sol, 1=nublado, 2=lluvia
	var tween := create_tween()
	tween.set_parallel(true)
	match state:
		0:
			climate_icon.texture = icon_sol
			tween.tween_property(screen_tint, "color", TINT_SUN, 3.0)
		1:
			climate_icon.texture = icon_nube
			tween.tween_property(screen_tint, "color", TINT_CLOUD, 3.0)
		2:
			climate_icon.texture = icon_lluvia
			tween.tween_property(screen_tint, "color", TINT_RAIN, 3.0)

func set_climate_interior() -> void:
	# Para zonas interiores: sin tint de clima
	var tween := create_tween()
	tween.tween_property(screen_tint, "color", TINT_NONE, 2.0)
	climate_icon.texture = null

func _on_inventory_changed(inv: Array) -> void:
	var slots := inventory_bar.get_children()
	for i in slots.size():
		var slot_icon: TextureRect = slots[i].get_node("SlotIcon")
		if i < inv.size():
			var item := ItemData.get_item(inv[i])
			if item.has("icon_path"):
				slot_icon.texture = load(item["icon_path"])
		else:
			slot_icon.texture = null
```

---

## 5. ESCENAS: PERSONAJE UMA

### 5.1 `Uma.tscn` — Árbol de Nodos Completo

```
Uma (CharacterBody2D)
├── script: uma.gd
├── collision_layer: 2 (uma)
├── collision_mask: 1 (world)
│
├── Sprite2D  ← spritesheet principal
│   ├── texture: uma_idle_down.png
│   ├── hframes: 4
│   ├── vframes: 1
│   └── centered: true
│
├── AnimationPlayer
│   ├── [idle_down]    2fr @6fps  loop
│   ├── [idle_up]      2fr @6fps  loop
│   ├── [idle_side]    2fr @6fps  loop
│   ├── [walk_down]    4fr @10fps loop
│   ├── [walk_up]      4fr @10fps loop
│   ├── [walk_side]    4fr @10fps loop
│   ├── [sit]          1fr static
│   ├── [draw]         3fr @4fps  loop
│   ├── [think]        2fr @4fps  loop
│   └── [hat_on]       2fr @8fps  oneshot
│
├── CollisionShape2D
│   └── CapsuleShape2D
│       ├── radius: 5
│       └── height: 12
│
├── InteractArea (Area2D)
│   ├── collision_layer: 0
│   ├── collision_mask: 4|6  (items + interactuables)
│   └── CollisionShape2D
│       └── CircleShape2D radius: 28
│
├── HatNode (Node2D)
│   └── HatSprite (Sprite2D)
│       ├── texture: uma_hat.png
│       ├── position: Vector2(0, -10)  ← relativo a Uma
│       └── visible: true
│
├── ShadowSprite (Sprite2D)
│   ├── texture: shadow_ellipse.png
│   ├── position: Vector2(0, 6)
│   ├── modulate: Color(0, 0, 0, 0.25)
│   └── z_index: -1
│
├── ThoughtBubble (CanvasLayer)
│   ├── layer: 5
│   └── ThoughtLabel (RichTextLabel)
│       ├── bbcode_enabled: true
│       ├── custom_minimum_size: Vector2(200, 0)
│       ├── visible: false
│       ├── theme_override_font: fuente_monologos.ttf
│       └── theme_override_font_size: 11
│
└── FootstepPlayer (AudioStreamPlayer2D)
    └── [reproduce sfx según superficie]
```

### 5.2 `uma.gd` — Script Completo

```gdscript
# res://scripts/characters/uma.gd
extends CharacterBody2D
class_name Uma

# ─────────────────────────────────────────────
# CONSTANTES
# ─────────────────────────────────────────────
const SPEED_NORMAL: float = 72.0
const SPEED_SLOW:   float = 36.0

# ─────────────────────────────────────────────
# NODOS
# ─────────────────────────────────────────────
@onready var sprite:        Sprite2D       = $Sprite2D
@onready var anim:          AnimationPlayer = $AnimationPlayer
@onready var interact_area: Area2D         = $InteractArea
@onready var hat_sprite:    Sprite2D       = $HatNode/HatSprite
@onready var thought_label: RichTextLabel  = $ThoughtBubble/ThoughtLabel
@onready var foot_player:   AudioStreamPlayer2D = $FootstepPlayer

# ─────────────────────────────────────────────
# ESTADO
# ─────────────────────────────────────────────
var can_move:       bool     = true
var is_sitting:     bool     = false
var is_drawing:     bool     = false
var last_direction: Vector2  = Vector2.DOWN
var nearby_objects: Array    = []
var _thought_tween: Tween

# ─────────────────────────────────────────────
# SEÑALES
# ─────────────────────────────────────────────
signal started_interaction(obj: Node)

# ─────────────────────────────────────────────
# CICLO
# ─────────────────────────────────────────────
func _ready() -> void:
	add_to_group("uma")
	GameManager.uma_node = self
	hat_sprite.visible = GameManager.uma_has_hat
	interact_area.area_entered.connect(_on_enter_range)
	interact_area.area_exited.connect(_on_exit_range)
	interact_area.body_entered.connect(_on_body_enter)
	interact_area.body_exited.connect(_on_body_exit)

func _physics_process(delta: float) -> void:
	if not can_move or is_sitting or is_drawing:
		velocity = Vector2.ZERO
		move_and_slide()
		return

	var dir := Input.get_vector("move_left", "move_right", "move_up", "move_down")
	var speed := SPEED_SLOW if Input.is_action_pressed("slow_walk") else SPEED_NORMAL

	if dir != Vector2.ZERO:
		last_direction = dir
		velocity = dir * speed
		_update_animation_walk(dir)
		_play_footstep(delta)
	else:
		velocity = Vector2.ZERO
		_update_animation_idle()

	move_and_slide()

func _input(event: InputEvent) -> void:
	if event.is_action_pressed("interact"):
		_try_interact()
	if event.is_action_pressed("open_notebook"):
		_open_notebook()
	if event.is_action_pressed("open_inventory"):
		_open_inventory()
	if event.is_action_pressed("pause"):
		_toggle_pause()

# ─────────────────────────────────────────────
# INTERACCIÓN
# ─────────────────────────────────────────────
func _try_interact() -> void:
	if nearby_objects.is_empty():
		return
	var closest := _get_closest()
	if closest and closest.has_method("interact"):
		closest.interact()
		started_interaction.emit(closest)

func _get_closest() -> Node:
	var closest: Node = null
	var min_dist: float = INF
	for obj in nearby_objects:
		if is_instance_valid(obj):
			var d := global_position.distance_to(obj.global_position)
			if d < min_dist:
				min_dist = d
				closest = obj
	return closest

func _on_enter_range(area: Area2D) -> void:
	if area.has_method("interact"):
		nearby_objects.append(area)
		_update_interact_hint()

func _on_exit_range(area: Area2D) -> void:
	nearby_objects.erase(area)
	_update_interact_hint()

func _on_body_enter(body: Node2D) -> void:
	if body.has_method("interact") and body != self:
		nearby_objects.append(body)
		_update_interact_hint()

func _on_body_exit(body: Node2D) -> void:
	nearby_objects.erase(body)
	_update_interact_hint()

func _update_interact_hint() -> void:
	var hud := get_tree().get_first_node_in_group("hud") as HUD
	if hud:
		hud.set_interact_hint(not nearby_objects.is_empty())

# ─────────────────────────────────────────────
# ANIMACIONES
# ─────────────────────────────────────────────
func _update_animation_walk(dir: Vector2) -> void:
	var anim_name: String
	if abs(dir.x) > abs(dir.y):
		anim_name = "walk_side"
		sprite.flip_h = (dir.x < 0)
		hat_sprite.flip_h = (dir.x < 0)
	elif dir.y < 0:
		anim_name = "walk_up"
	else:
		anim_name = "walk_down"
	if anim.current_animation != anim_name:
		anim.play(anim_name)

func _update_animation_idle() -> void:
	var anim_name: String
	if abs(last_direction.x) > abs(last_direction.y):
		anim_name = "idle_side"
	elif last_direction.y < 0:
		anim_name = "idle_up"
	else:
		anim_name = "idle_down"
	if anim.current_animation != anim_name:
		anim.play(anim_name)

# ─────────────────────────────────────────────
# SOMBRERO
# ─────────────────────────────────────────────
func take_hat_off() -> void:
	hat_sprite.visible = false

func put_hat_on() -> void:
	hat_sprite.visible = true
	anim.play("hat_on")
	await anim.animation_finished
	_update_animation_idle()

# ─────────────────────────────────────────────
# PENSAMIENTO INTERNO
# ─────────────────────────────────────────────
func show_thought(text: String, duration: float = 4.0) -> void:
	if _thought_tween and _thought_tween.is_valid():
		_thought_tween.kill()

	thought_label.text = "[i]" + text + "[/i]"
	thought_label.visible = true
	thought_label.modulate.a = 0.0

	_thought_tween = create_tween()
	_thought_tween.tween_property(thought_label, "modulate:a", 1.0, 0.4)
	_thought_tween.tween_interval(duration)
	_thought_tween.tween_property(thought_label, "modulate:a", 0.0, 0.6)
	_thought_tween.tween_callback(func(): thought_label.visible = false)

# ─────────────────────────────────────────────
# ESTADOS ESPECIALES
# ─────────────────────────────────────────────
func sit_down() -> void:
	is_sitting = true
	velocity = Vector2.ZERO
	anim.play("sit")

func stand_up() -> void:
	is_sitting = false
	_update_animation_idle()

func start_drawing() -> void:
	is_drawing = true
	velocity = Vector2.ZERO
	anim.play("draw")

func stop_drawing() -> void:
	is_drawing = false
	_update_animation_idle()

func freeze() -> void:
	can_move = false
	velocity = Vector2.ZERO

func unfreeze() -> void:
	can_move = true

# ─────────────────────────────────────────────
# PASOS
# ─────────────────────────────────────────────
var _footstep_timer: float = 0.0
const FOOTSTEP_INTERVAL: float = 0.4

@export var sfx_vereda:   AudioStream
@export var sfx_cesped:   AudioStream
@export var sfx_interior: AudioStream

var current_surface: String = "vereda"

func _play_footstep(delta: float) -> void:
	_footstep_timer += delta
	if _footstep_timer >= FOOTSTEP_INTERVAL:
		_footstep_timer = 0.0
		match current_surface:
			"vereda":   foot_player.stream = sfx_vereda
			"cesped":   foot_player.stream = sfx_cesped
			"interior": foot_player.stream = sfx_interior
		if foot_player.stream:
			foot_player.play()

# ─────────────────────────────────────────────
# NAVEGACIÓN
# ─────────────────────────────────────────────
func _open_notebook() -> void:
	var nb := get_tree().get_first_node_in_group("notebook")
	if nb and nb.has_method("toggle"):
		nb.toggle()

func _open_inventory() -> void:
	var inv := get_tree().get_first_node_in_group("inventory")
	if inv and inv.has_method("toggle"):
		inv.toggle()

func _toggle_pause() -> void:
	var pm := get_tree().get_first_node_in_group("pause_menu")
	if pm and pm.has_method("toggle"):
		pm.toggle()
```

---

## 6. ESCENAS: NPCS Y PABLO

### 6.1 `NPC_Base.tscn` — Árbol de Nodos

```
NPC_Base (CharacterBody2D)
├── script: npc_base.gd
├── collision_layer: 3 (npcs)
├── collision_mask: 1 (world)
│
├── Sprite2D
│   └── texture: [asignar por instancia]
│
├── AnimationPlayer
│   └── [idle] loop
│
├── CollisionShape2D
│   └── CapsuleShape2D radius:4 height:10
│
├── InteractCollider (Area2D)
│   ├── collision_layer: 6  (interactuables)
│   ├── collision_mask: 0
│   └── CollisionShape2D
│       └── CircleShape2D radius: 24
│
└── NameLabel (Label)   ← visible solo en debug
    ├── text: [nombre NPC]
    └── visible: false
```

### 6.2 `npc_base.gd`

```gdscript
# res://scripts/characters/npc_base.gd
extends CharacterBody2D
class_name NPCBase

# ─────────────────────────────────────────────
# EXPORT: configurar por instancia en el editor
# ─────────────────────────────────────────────
@export var npc_name:           String = ""
@export var dialog_ids:         Array[String] = []
# Múltiples diálogos: se usan por nivel de conversación
# dialog_ids[0] = primer encuentro
# dialog_ids[1] = segundo encuentro (si existe)
# dialog_ids[2] = tercer encuentro (si existe)

@export var flag_on_talk:       String = ""   # flag a setear en true al hablar
@export var flag_increment:     String = ""   # flag numérico a incrementar
@export var requires_flag:      String = ""   # flag requerido para hablar
@export var requires_item:      String = ""   # item requerido para hablar

# ─────────────────────────────────────────────
# ESTADO
# ─────────────────────────────────────────────
var talk_count: int = 0

# ─────────────────────────────────────────────
# MÉTODOS
# ─────────────────────────────────────────────
func interact() -> void:
	# Verificar prerequisitos
	if requires_flag != "" and not FlagsManager.get_flag(requires_flag):
		return
	if requires_item != "" and not GameManager.has_item(requires_item):
		return

	# Determinar qué diálogo mostrar
	var dialog_id := _get_dialog_id()
	if dialog_id == "":
		return

	# Mostrar diálogo
	var dialog_box := get_tree().get_first_node_in_group("dialog_box")
	if dialog_box and dialog_box.has_method("start_dialog"):
		dialog_box.start_dialog(dialog_id)
		dialog_box.dialog_finished.connect(_on_dialog_finished, CONNECT_ONE_SHOT)

func _get_dialog_id() -> String:
	if dialog_ids.is_empty():
		return ""
	var idx := mini(talk_count, dialog_ids.size() - 1)
	return dialog_ids[idx]

func _on_dialog_finished() -> void:
	talk_count += 1
	if flag_on_talk != "":
		FlagsManager.set_flag(flag_on_talk, true)
	if flag_increment != "":
		FlagsManager.increment_flag(flag_increment)
	_on_talked()

func _on_talked() -> void:
	# Sobreescribir en subclases si es necesario
	pass
```

### 6.3 `Pablo.tscn` — Árbol de Nodos

```
Pablo (CharacterBody2D)
├── script: pablo.gd
│
├── Sprite2D
│   └── texture: pablo_idle_side.png
│   └── hframes: 2
│
├── AnimationPlayer
│   └── [idle] 2fr @5fps loop
│
├── CollisionShape2D
│   └── CapsuleShape2D radius:5 height:12
│
├── InteractCollider (Area2D)
│   └── CollisionShape2D CircleShape2D radius: 32
│
└── [visible: false por defecto — se activa en Acto V]
```

### 6.4 `pablo.gd`

```gdscript
# res://scripts/characters/pablo.gd
extends CharacterBody2D

@onready var sprite: Sprite2D = $Sprite2D
@onready var anim:   AnimationPlayer = $AnimationPlayer

func _ready() -> void:
	visible = false
	anim.play("idle")

func appear() -> void:
	visible = true
	modulate.a = 0.0
	var tween := create_tween()
	tween.tween_property(self, "modulate:a", 1.0, 2.5)

func interact() -> void:
	# Llamado desde EspacioFinal cuando es el momento
	pass  # El diálogo se maneja directamente desde espacio_final.gd

func _choose_dialog_id() -> String:
	# Selecciona la variante de diálogo según flags y objetos
	if GameManager.has_item("disco_vinilo") and FlagsManager.get_flag("puso_disco_radio_final"):
		return "pablo_final_disco"
	if FlagsManager.get_flag("se_sentó_banco_plaza"):
		return "pablo_final_banco"
	if GameManager.has_item("nota_padre_2"):
		return "pablo_final_nota"
	if FlagsManager.get_flag("comparó_notas"):
		return "pablo_final_notas_comparadas"
	if not GameManager.uma_has_hat:
		return "pablo_final_sin_sombrero"
	return "pablo_final_base"

func get_dialog_id() -> String:
	return _choose_dialog_id()
```

---

## 7. ESCENAS: OBJETOS DEL MUNDO

### 7.1 `PickupItem.tscn`

```
PickupItem (Area2D)
├── script: pickup_item.gd
├── collision_layer: 4 (items)
├── collision_mask: 0
│
├── Sprite2D
│   └── texture: [asignar por instancia]
│
├── CollisionShape2D
│   └── RectangleShape2D size: Vector2(12, 12)
│
├── GlowEffect (PointLight2D)   ← pulso suave para indicar que es recogible
│   ├── color: Color(1, 0.95, 0.7, 0.6)
│   ├── energy: 0.8
│   ├── texture_scale: 0.4
│   └── [AnimationPlayer anima energy entre 0.5 y 0.8 @1seg loop]
│
└── PickupLabel (Label)
    ├── text: [nombre del item]
    ├── theme_override_font_size: 9
    ├── horizontal_alignment: CENTER
    └── offset_top: -18
```

### 7.2 `pickup_item.gd`

```gdscript
# res://scripts/objects/pickup_item.gd
extends Area2D

@export var item_id:          String = ""
@export var pickup_dialog_id: String = ""   # monólogo al recoger
@export var auto_add_to_inv:  bool   = true

@onready var item_sprite: Sprite2D = $Sprite2D
@onready var glow:        PointLight2D = $GlowEffect

var already_picked: bool = false

func _ready() -> void:
	# Si ya está en el inventario (carga de save), eliminarse
	if GameManager.has_item(item_id):
		queue_free()
		return
	add_to_group("pickup_items")

func interact() -> void:
	if already_picked:
		return
	_pick_up()

func _pick_up() -> void:
	if auto_add_to_inv:
		var ok := GameManager.add_item(item_id)
		if not ok:
			# Inventario lleno
			var db := get_tree().get_first_node_in_group("dialog_box")
			if db:
				db.start_dialog("uma_inventory_full")
			return

	already_picked = true

	# Sonido de recogida
	var sfx := AudioStreamPlayer.new()
	sfx.stream = preload("res://assets/audio/sfx/item_pickup.ogg")
	sfx.autoplay = true
	sfx.process_mode = Node.PROCESS_MODE_ALWAYS
	get_tree().root.add_child(sfx)
	sfx.finished.connect(sfx.queue_free)

	# Animación de recogida
	var tween := create_tween()
	tween.set_parallel(true)
	tween.tween_property(item_sprite, "position:y", item_sprite.position.y - 8, 0.3)
	tween.tween_property(item_sprite, "modulate:a", 0.0, 0.3)
	tween.tween_property(glow, "energy", 0.0, 0.3)
	await tween.finished

	# Mostrar diálogo de recogida
	if pickup_dialog_id != "":
		var db := get_tree().get_first_node_in_group("dialog_box")
		if db:
			db.start_dialog(pickup_dialog_id)

	queue_free()
```

### 7.3 `InspectableObject.tscn` y `inspectable_object.gd`

```
InspectableObject (Area2D)
├── script: inspectable_object.gd
├── collision_layer: 6 (interactuables)
│
├── Sprite2D (opcional, si el objeto es visible)
│
└── CollisionShape2D (forma según objeto)
```

```gdscript
# res://scripts/objects/inspectable_object.gd
extends Area2D

@export var dialog_id:          String = ""
@export var dialog_id_second:   String = ""   # segundo intento de inspección
@export var flag_on_inspect:    String = ""
@export var requires_flag:      String = ""
@export var requires_item:      String = ""
@export var hint_text:          String = "Inspeccionar"

var inspect_count: int = 0

func _ready() -> void:
	add_to_group("interactables")

func interact() -> void:
	if requires_flag != "" and not FlagsManager.get_flag(requires_flag):
		return
	if requires_item != "" and not GameManager.has_item(requires_item):
		return

	var id := dialog_id
	if inspect_count > 0 and dialog_id_second != "":
		id = dialog_id_second

	if id == "":
		return

	var db := get_tree().get_first_node_in_group("dialog_box")
	if db:
		db.start_dialog(id)

	inspect_count += 1
	if flag_on_inspect != "":
		FlagsManager.set_flag(flag_on_inspect, true)
```

### 7.4 `DiscSlot.tscn` y `disc_slot.gd`

```gdscript
# res://scripts/objects/disc_slot.gd
extends Area2D

# Contexto: puede ser la ranura en Zona Interior o la Radio en Espacio Final
@export var is_radio: bool = false  # true = radio del acto final

signal disc_placed

func interact() -> void:
	if not GameManager.has_item("disco_vinilo"):
		var db := get_tree().get_first_node_in_group("dialog_box")
		if db:
			db.start_dialog("disc_slot_empty")
		return

	if is_radio:
		_activate_radio()
	else:
		_activate_slot_intermedia()

func _activate_slot_intermedia() -> void:
	if FlagsManager.get_flag("activó_disco_zona_interior"):
		return
	FlagsManager.set_flag("activó_disco_zona_interior", true)
	GameManager.register_melody_fragment("frag4_intermedia")
	MusicManager.play_fragment("lavuelta_frag4_intermedia")
	var db := get_tree().get_first_node_in_group("dialog_box")
	if db:
		db.start_dialog("disco_puesto_intermedia")
	disc_placed.emit()

func _activate_radio() -> void:
	if FlagsManager.get_flag("puso_disco_radio_final"):
		return
	FlagsManager.set_flag("puso_disco_radio_final", true)
	disc_placed.emit()
	var db := get_tree().get_first_node_in_group("dialog_box")
	if db:
		db.start_dialog("climax_radio_con_disco")
		await db.dialog_finished
	MusicManager.play_track("lavuelta_completa", true)
```

### 7.5 `HatHook.tscn` y `hat_hook.gd`

```gdscript
# res://scripts/objects/hat_hook.gd
extends Area2D

signal hat_hung
signal hat_retrieved

var has_hat: bool = false

func interact() -> void:
	if has_hat:
		_retrieve_hat()
	else:
		_hang_hat()

func _hang_hat() -> void:
	if not GameManager.uma_has_hat:
		return  # Uma no tiene sombrero encima
	FlagsManager.set_flag("dejó_sombrero_gancho", true)
	GameManager.set_hat(false)
	has_hat = true
	hat_hung.emit()
	var db := get_tree().get_first_node_in_group("dialog_box")
	if db:
		db.start_dialog("sombrero_colgado")

func _retrieve_hat() -> void:
	GameManager.set_hat(true)
	FlagsManager.set_flag("recuperó_sombrero", true)
	has_hat = false
	hat_retrieved.emit()
	var db := get_tree().get_first_node_in_group("dialog_box")
	if db:
		db.start_dialog("sombrero_recuperado")
```

### 7.6 `PhotoFrame.tscn` y `photo_frame.gd`

```gdscript
# res://scripts/objects/photo_frame.gd
extends Area2D

@onready var photo_sprite: Sprite2D = $PhotoSprite

func _ready() -> void:
	photo_sprite.visible = false

func interact() -> void:
	if FlagsManager.get_flag("colocó_polaroid_marco"):
		var db := get_tree().get_first_node_in_group("dialog_box")
		if db:
			db.start_dialog("foto_ya_puesta")
		return

	if not GameManager.has_item("foto_polaroid"):
		var db := get_tree().get_first_node_in_group("dialog_box")
		if db:
			db.start_dialog("marco_vacio_sin_foto")
		return

	_place_photo()

func _place_photo() -> void:
	FlagsManager.set_flag("colocó_polaroid_marco", true)
	photo_sprite.visible = true
	var db := get_tree().get_first_node_in_group("dialog_box")
	if db:
		db.start_dialog("foto_en_marco")
```

### 7.7 `Bench.tscn` y `bench.gd`

```gdscript
# res://scripts/objects/bench.gd
extends Area2D

@export var dialog_id:          String = "uma_choice_banco"
@export var draw_trigger:       bool   = false  # activa opción de dibujar
@export var idle_wait_seconds:  float  = 180.0  # tiempo para auto-dibujar

var uma_sitting: bool = false
var _idle_timer:  float = 0.0

func _process(delta: float) -> void:
	if uma_sitting and draw_trigger and not FlagsManager.get_flag("dibujó_plaza"):
		_idle_timer += delta
		if _idle_timer >= idle_wait_seconds:
			_auto_draw()

func interact() -> void:
	var db := get_tree().get_first_node_in_group("dialog_box")
	if db:
		db.start_dialog(dialog_id)
		db.dialog_finished.connect(_on_choice_done, CONNECT_ONE_SHOT)

func sit_uma() -> void:
	uma_sitting = true
	_idle_timer = 0.0
	FlagsManager.set_flag("se_sentó_banco_plaza", true)
	if GameManager.uma_node:
		GameManager.uma_node.sit_down()

func stand_uma() -> void:
	uma_sitting = false
	_idle_timer = 0.0
	if GameManager.uma_node:
		GameManager.uma_node.stand_up()

func _on_choice_done() -> void:
	pass  # La lógica de sentarse/pararse la maneja el diálogo via callbacks

func _auto_draw() -> void:
	_idle_timer = 0.0
	FlagsManager.set_flag("dibujó_plaza", true)
	GameManager.add_item("boceto_plaza")
	GameManager.add_sketch({
		"zone": "plaza",
		"texture": "res://assets/sprites/objects/boceto_plaza.png",
		"description": "La plaza bajo la lluvia. La fuente seca. Los árboles."
	})
	if GameManager.uma_node:
		GameManager.uma_node.start_drawing()
	var db := get_tree().get_first_node_in_group("dialog_box")
	if db:
		db.start_dialog("uma_dibuja_plaza")
		await db.dialog_finished
	if GameManager.uma_node:
		GameManager.uma_node.stop_drawing()
```

---

## 8. SISTEMA DE DIÁLOGOS

### 8.1 `DialogBox.tscn` — Árbol de Nodos

```
DialogBox (CanvasLayer)
├── layer: 20
├── add_to_group: "dialog_box"
│
├── Panel (PanelContainer)
│   ├── anchor_preset: BOTTOM_WIDE
│   ├── offset_top: -130
│   ├── offset_left: 16
│   ├── offset_right: -16
│   ├── offset_bottom: -12
│   │
│   └── MarginContainer
│       ├── margin_left: 12
│       ├── margin_right: 12
│       ├── margin_top: 8
│       ├── margin_bottom: 8
│       └── VBoxContainer
│           ├── separation: 6
│           ├── SpeakerLabel (Label)
│           │   ├── theme_override_font: fuente_titulos.ttf
│           │   ├── theme_override_font_size: 12
│           │   └── theme_override_color: Color(0.8, 0.7, 0.4, 1)
│           ├── DialogText (RichTextLabel)
│           │   ├── bbcode_enabled: true
│           │   ├── scroll_active: false
│           │   ├── custom_minimum_size: Vector2(0, 64)
│           │   ├── theme_override_font: fuente_principal.ttf
│           │   └── theme_override_font_size: 12
│           └── ChoicesContainer (VBoxContainer)
│               ├── separation: 4
│               └── visible: false
│
└── ContinueArrow (TextureRect)
    ├── texture: continue_arrow.png
    ├── anchor_preset: BOTTOM_RIGHT (del panel)
    ├── custom_minimum_size: Vector2(10, 10)
    ├── visible: false
    └── [AnimationPlayer: parpadeo @1seg]
```

### 8.2 `dialog_box.gd` — Completo

```gdscript
# res://scripts/ui/dialog_box.gd
extends CanvasLayer

# ─────────────────────────────────────────────
# SEÑALES
# ─────────────────────────────────────────────
signal dialog_finished
signal choice_made(index: int, choice_data: Dictionary)

# ─────────────────────────────────────────────
# NODOS
# ─────────────────────────────────────────────
@onready var panel:            PanelContainer  = $Panel
@onready var speaker_label:    Label           = $Panel/MarginContainer/VBoxContainer/SpeakerLabel
@onready var dialog_text:      RichTextLabel   = $Panel/MarginContainer/VBoxContainer/DialogText
@onready var choices_box:      VBoxContainer   = $Panel/MarginContainer/VBoxContainer/ChoicesContainer
@onready var continue_arrow:   TextureRect     = $ContinueArrow
@onready var anim:             AnimationPlayer = $ContinueArrow/AnimationPlayer

# ─────────────────────────────────────────────
# ESTADO
# ─────────────────────────────────────────────
var is_active:   bool = false
var is_typing:   bool = false
var full_text:   String = ""
var char_idx:    int = 0
var type_speed:  float = 0.03   # segundos por caracter
var _timer:      float = 0.0
var _line_data:  Dictionary = {}
var _dialog_queue: Array = []

# ─────────────────────────────────────────────
# CICLO
# ─────────────────────────────────────────────
func _ready() -> void:
	add_to_group("dialog_box")
	panel.visible = false

func _process(delta: float) -> void:
	if not is_typing:
		return
	_timer += delta
	if _timer >= type_speed:
		_timer = 0.0
		_advance_type()

func _input(event: InputEvent) -> void:
	if not is_active:
		return
	if event.is_action_pressed("interact") or event.is_action_pressed("ui_accept"):
		_handle_advance()

# ─────────────────────────────────────────────
# INICIO DE DIÁLOGO
# ─────────────────────────────────────────────
func start_dialog(dialog_id: String) -> void:
	var data: Dictionary = DialogData.get_dialog(dialog_id)
	if data.is_empty():
		push_warning("DialogBox: Diálogo no encontrado: '%s'" % dialog_id)
		return

	# Verificar condición
	if data.has("condition"):
		if not _check_condition(data["condition"]):
			var fail_id: String = data.get("condition_fail", "")
			if fail_id != "":
				start_dialog(fail_id)
			return

	# Setear flag al iniciar si está especificado
	if data.has("set_flag_on_start"):
		FlagsManager.set_flag(data["set_flag_on_start"], true)

	is_active = true
	panel.visible = true
	if GameManager.uma_node:
		GameManager.uma_node.freeze()

	_dialog_queue = data.get("lines", []).duplicate()
	_show_next_line()

func _show_next_line() -> void:
	if _dialog_queue.is_empty():
		_end_dialog()
		return

	_line_data = _dialog_queue.pop_front()

	# Speaker
	var spk: String = _line_data.get("speaker", "")
	speaker_label.text = spk
	speaker_label.visible = (spk != "")

	# Texto
	full_text = _line_data.get("text", "")
	dialog_text.text = ""
	char_idx = 0
	is_typing = true
	_timer = 0.0
	continue_arrow.visible = false
	choices_box.visible = false

	# SFX blip
	if _line_data.get("play_blip", true):
		_play_blip()

func _advance_type() -> void:
	if char_idx >= full_text.length():
		is_typing = false
		_on_type_done()
		return
	dialog_text.text += full_text[char_idx]
	char_idx += 1

func _on_type_done() -> void:
	var choices: Array = _line_data.get("choices", [])
	if choices.size() > 0:
		_show_choices(choices)
	else:
		continue_arrow.visible = true
		anim.play("blink")

	# Ejecutar callback de esta línea si existe
	if _line_data.has("set_flag"):
		FlagsManager.set_flag(_line_data["set_flag"], _line_data.get("flag_value", true))
	if _line_data.has("add_item"):
		GameManager.add_item(_line_data["add_item"])
	if _line_data.has("play_melody"):
		GameManager.register_melody_fragment(_line_data["play_melody"])
		MusicManager.play_fragment(_line_data["play_melody"])

func _handle_advance() -> void:
	if is_typing:
		# Completar texto de golpe
		dialog_text.text = full_text
		char_idx = full_text.length()
		is_typing = false
		_on_type_done()
		return
	if choices_box.visible:
		return  # Esperar selección
	# Avanzar a la siguiente línea o dialog_id
	var next_id: String = _line_data.get("next", "")
	if next_id != "":
		start_dialog(next_id)
	elif not _dialog_queue.is_empty():
		_show_next_line()
	else:
		_end_dialog()

# ─────────────────────────────────────────────
# CHOICES
# ─────────────────────────────────────────────
func _show_choices(choices: Array) -> void:
	choices_box.visible = true
	continue_arrow.visible = false
	for child in choices_box.get_children():
		child.queue_free()
	for i in choices.size():
		var ch: Dictionary = choices[i]
		# Verificar condición de choice individual
		if ch.has("requires_flag") and not FlagsManager.get_flag(ch["requires_flag"]):
			continue
		if ch.has("requires_item") and not GameManager.has_item(ch["requires_item"]):
			continue
		var btn := Button.new()
		btn.text = ch.get("text", "...")
		btn.add_theme_font_size_override("font_size", 11)
		btn.pressed.connect(_on_choice_pressed.bind(i, ch))
		choices_box.add_child(btn)

func _on_choice_pressed(index: int, choice_data: Dictionary) -> void:
	choices_box.visible = false
	choice_made.emit(index, choice_data)
	# Setear flags de la choice
	if choice_data.has("set_flag"):
		FlagsManager.set_flag(choice_data["set_flag"], choice_data.get("flag_value", true))
	if choice_data.has("add_item"):
		GameManager.add_item(choice_data["add_item"])
	# Navegar al siguiente diálogo
	var next_id: String = choice_data.get("next", "")
	if next_id != "":
		start_dialog(next_id)
	else:
		_end_dialog()

# ─────────────────────────────────────────────
# FIN
# ─────────────────────────────────────────────
func _end_dialog() -> void:
	is_active = false
	panel.visible = false
	continue_arrow.visible = false
	if GameManager.uma_node:
		GameManager.uma_node.unfreeze()
	dialog_finished.emit()

# ─────────────────────────────────────────────
# CONDICIONES
# ─────────────────────────────────────────────
func _check_condition(condition: String) -> bool:
	var parts: PackedStringArray = condition.split(":")
	if parts.size() < 2:
		return true
	var ctype: String = parts[0]
	var cval:  String = parts[1]
	match ctype:
		"has_item":
			return GameManager.has_item(cval)
		"not_has_item":
			return not GameManager.has_item(cval)
		"flag":
			return FlagsManager.get_flag(cval) == true
		"not_flag":
			return FlagsManager.get_flag(cval) == false
		"flag_gte":
			# flag_gte:nombre_flag:valor_minimo
			if parts.size() < 3:
				return false
			var flag_val = FlagsManager.get_flag(cval)
			return flag_val is int and flag_val >= int(parts[2])
		"has_hat":
			return GameManager.uma_has_hat
		"no_hat":
			return not GameManager.uma_has_hat
		_:
			return true

# ─────────────────────────────────────────────
# SFX
# ─────────────────────────────────────────────
var _blip_stream: AudioStream = preload("res://assets/audio/sfx/dialog_blip.ogg")
var _blip_player: AudioStreamPlayer

func _play_blip() -> void:
	if _blip_player == null:
		_blip_player = AudioStreamPlayer.new()
		_blip_player.stream = _blip_stream
		_blip_player.volume_db = -12.0
		add_child(_blip_player)
	if not _blip_player.playing:
		_blip_player.play()
```

---

## 9. SISTEMA DE CLIMA

### 9.1 `WeatherSystem.tscn`

```
WeatherSystem (Node2D)
├── script: weather_system.gd
│
├── RainParticles (GPUParticles2D)
│   ├── emitting: false
│   ├── amount: 200
│   ├── lifetime: 1.5
│   ├── explosiveness: 0.0
│   ├── randomness: 0.2
│   ├── local_coords: false
│   ├── draw_order: DRAW_ORDER_INDEX
│   └── ProcessMaterial (ParticleProcessMaterial)
│       ├── direction: Vector3(0.15, 1, 0)   ← ángulo de lluvia
│       ├── spread: 5.0
│       ├── gravity: Vector3(0, 400, 0)
│       ├── initial_velocity_min: 180.0
│       ├── initial_velocity_max: 220.0
│       ├── scale_min: 0.8
│       ├── scale_max: 1.2
│       └── color: Color(0.85, 0.92, 1.0, 0.6)
│
├── SunOverlay (ColorRect)
│   ├── anchors_preset: FULL_RECT
│   ├── color: Color(1.0, 0.90, 0.50, 0.0)
│   └── mouse_filter: MOUSE_FILTER_IGNORE
│
├── CloudOverlay (ColorRect)
│   ├── anchors_preset: FULL_RECT
│   ├── color: Color(0.78, 0.84, 0.94, 0.0)
│   └── mouse_filter: MOUSE_FILTER_IGNORE
│
├── RainOverlay (ColorRect)
│   ├── anchors_preset: FULL_RECT
│   ├── color: Color(0.59, 0.71, 0.90, 0.0)
│   └── mouse_filter: MOUSE_FILTER_IGNORE
│
└── RainAudio (AudioStreamPlayer)
    ├── stream: rain_loop.ogg
    ├── volume_db: -80.0
    └── autoplay: false
```

### 9.2 `weather_system.gd` — Completo

```gdscript
# res://scripts/systems/weather_system.gd
extends Node2D
class_name WeatherSystem

# ─────────────────────────────────────────────
# ESTADOS
# ─────────────────────────────────────────────
enum State { SUNNY, CLOUDY, RAIN, INTERIOR }

const TARGET_ALPHA: Dictionary = {
	State.SUNNY:    { "sun": 0.08, "cloud": 0.00, "rain_overlay": 0.00, "rain_particles": false, "rain_audio": -80.0 },
	State.CLOUDY:   { "sun": 0.00, "cloud": 0.12, "rain_overlay": 0.00, "rain_particles": false, "rain_audio": -80.0 },
	State.RAIN:     { "sun": 0.00, "cloud": 0.00, "rain_overlay": 0.15, "rain_particles": true,  "rain_audio": -10.0 },
	State.INTERIOR: { "sun": 0.00, "cloud": 0.00, "rain_overlay": 0.00, "rain_particles": false, "rain_audio": -80.0 }
}

# ─────────────────────────────────────────────
# NODOS
# ─────────────────────────────────────────────
@onready var rain_particles: GPUParticles2D = $RainParticles
@onready var sun_overlay:    ColorRect      = $SunOverlay
@onready var cloud_overlay:  ColorRect      = $CloudOverlay
@onready var rain_overlay:   ColorRect      = $RainOverlay
@onready var rain_audio:     AudioStreamPlayer = $RainAudio

# ─────────────────────────────────────────────
# ESTADO
# ─────────────────────────────────────────────
var current_state: State = State.SUNNY
@export var transition_duration: float = 4.0

signal weather_changed(new_state: State)

# ─────────────────────────────────────────────
# MÉTODOS PÚBLICOS
# ─────────────────────────────────────────────
func _ready() -> void:
	_apply_instant(current_state)

func set_weather(new_state: State, instant: bool = false) -> void:
	if new_state == current_state:
		return
	current_state = new_state
	var hud := get_tree().get_first_node_in_group("hud") as HUD
	if hud:
		if new_state == State.INTERIOR:
			hud.set_climate_interior()
		else:
			hud.set_climate_visual(new_state as int)
	if instant:
		_apply_instant(new_state)
	else:
		_transition_to(new_state)
	weather_changed.emit(new_state)

func get_state() -> State:
	return current_state

# ─────────────────────────────────────────────
# TRANSICIÓN
# ─────────────────────────────────────────────
func _transition_to(state: State) -> void:
	var target: Dictionary = TARGET_ALPHA[state]
	var tween := create_tween()
	tween.set_parallel(true)

	tween.tween_property(sun_overlay,   "color:a", target["sun"],          transition_duration)
	tween.tween_property(cloud_overlay, "color:a", target["cloud"],        transition_duration)
	tween.tween_property(rain_overlay,  "color:a", target["rain_overlay"], transition_duration)
	tween.tween_property(rain_audio,    "volume_db", target["rain_audio"], transition_duration)

	if target["rain_particles"]:
		rain_particles.emitting = true
		rain_particles.modulate.a = 0.0
		tween.tween_property(rain_particles, "modulate:a", 1.0, transition_duration)
	else:
		tween.tween_property(rain_particles, "modulate:a", 0.0, transition_duration)
		await tween.finished
		rain_particles.emitting = false

	if target["rain_audio"] > -79.0 and not rain_audio.playing:
		rain_audio.play()

func _apply_instant(state: State) -> void:
	var target: Dictionary = TARGET_ALPHA[state]
	sun_overlay.color.a   = target["sun"]
	cloud_overlay.color.a = target["cloud"]
	rain_overlay.color.a  = target["rain_overlay"]
	rain_audio.volume_db  = target["rain_audio"]
	rain_particles.emitting = target["rain_particles"]
	rain_particles.modulate.a = 1.0 if target["rain_particles"] else 0.0
	if target["rain_audio"] > -79.0:
		rain_audio.play()
```

---

## 10. SISTEMA DE INVENTARIO Y CUADERNO

### 10.1 `InventoryPanel.tscn`

```
InventoryPanel (CanvasLayer)
├── layer: 25
├── add_to_group: "inventory"
├── visible: false
│
├── DimOverlay (ColorRect)
│   ├── anchors_preset: FULL_RECT
│   ├── color: Color(0,0,0,0.5)
│   └── mouse_filter: MOUSE_FILTER_STOP
│
└── Panel (PanelContainer)
    ├── anchor_preset: CENTER
    ├── custom_minimum_size: Vector2(220, 280)
    └── MarginContainer (margins: 12px)
        └── VBoxContainer
            ├── TitleLabel (Label)
            │   ├── text: "Objetos de Uma"
            │   └── horizontal_alignment: CENTER
            ├── Separator (HSeparator)
            ├── SlotsGrid (GridContainer)
            │   ├── columns: 3
            │   ├── separation: 8
            │   └── [6× Slot instancias]
            │       └── ItemSlotUI (PanelContainer)
            │           ├── custom_minimum_size: Vector2(56, 56)
            │           ├── SlotIcon (TextureRect) stretch: KEEP_ASPECT_CENTERED
            │           └── [Button invisible sobre todo el slot]
            ├── Separator (HSeparator)
            ├── DescriptionLabel (RichTextLabel)
            │   ├── bbcode_enabled: true
            │   ├── custom_minimum_size: Vector2(0, 60)
            │   └── scroll_active: false
            └── BtnClose (Button)
                └── text: "Cerrar"
```

### 10.2 `inventory_panel.gd`

```gdscript
# res://scripts/ui/inventory_panel.gd
extends CanvasLayer

@onready var slots_grid:       GridContainer  = $Panel/MarginContainer/VBoxContainer/SlotsGrid
@onready var description_label: RichTextLabel = $Panel/MarginContainer/VBoxContainer/DescriptionLabel
@onready var btn_close:        Button         = $Panel/MarginContainer/VBoxContainer/BtnClose

var selected_slot: int = -1

func _ready() -> void:
	add_to_group("inventory")
	btn_close.pressed.connect(func(): toggle())
	GameManager.inventory_changed.connect(_refresh)
	visible = false

func toggle() -> void:
	visible = not visible
	if visible:
		_refresh(GameManager.inventory)
		if GameManager.uma_node:
			GameManager.uma_node.freeze()
	else:
		if GameManager.uma_node:
			GameManager.uma_node.unfreeze()

func _refresh(inv: Array) -> void:
	var slots := slots_grid.get_children()
	for i in slots.size():
		var slot := slots[i] as PanelContainer
		var icon: TextureRect = slot.get_node("SlotIcon")
		var btn:  Button      = slot.get_node("SelectButton")

		if i < inv.size():
			var item := ItemData.get_item(inv[i])
			if item.has("icon_path"):
				icon.texture = load(item["icon_path"])
			btn.pressed.connect(func(): _select_slot(i), CONNECT_ONE_SHOT)
		else:
			icon.texture = null

func _select_slot(idx: int) -> void:
	selected_slot = idx
	var item_id: String = GameManager.inventory[idx]
	var desc: String = ItemData.get_description(item_id)
	var item: Dictionary = ItemData.get_item(item_id)
	description_label.text = "[b]%s[/b]\n%s" % [item.get("name", ""), desc]
```

### 10.3 `Notebook.tscn` y `notebook.gd`

```gdscript
# res://scripts/ui/notebook.gd
extends CanvasLayer

@onready var sketch_display:  TextureRect  = $Panel/PageContainer/SketchDisplay
@onready var page_text:       RichTextLabel = $Panel/PageContainer/PageText
@onready var page_indicator:  Label        = $Panel/NavigationRow/PageIndicator
@onready var btn_prev:        Button       = $Panel/NavigationRow/BtnPrev
@onready var btn_next:        Button       = $Panel/NavigationRow/BtnNext
@onready var btn_close:       Button       = $Panel/BtnClose

# Páginas base (siempre presentes)
const BASE_PAGES: Array[Dictionary] = [
	{
		"texture": "res://assets/sprites/objects/boceto_banco.png",
		"text": "Un banco vacío. Lo dibujé mirando por la ventana."
	},
	{
		"texture": "res://assets/sprites/objects/boceto_arbol.png",
		"text": "Un árbol con mucha sombra. Me gustaba ese árbol."
	},
	{
		"texture": "res://assets/sprites/objects/boceto_figura.png",
		"text": "Una figura con sombrero. De espaldas. No sé si soy yo."
	}
]

var all_pages: Array[Dictionary] = []
var current_page: int = 0
var sfx_open: AudioStream = preload("res://assets/audio/sfx/notebook_open.ogg")

func _ready() -> void:
	add_to_group("notebook")
	btn_prev.pressed.connect(_prev_page)
	btn_next.pressed.connect(_next_page)
	btn_close.pressed.connect(func(): toggle())
	visible = false

func toggle() -> void:
	visible = not visible
	if visible:
		_rebuild_pages()
		_show_page(current_page)
		var sfx := AudioStreamPlayer.new()
		sfx.stream = sfx_open
		sfx.autoplay = true
		add_child(sfx)
		sfx.finished.connect(sfx.queue_free)
		if GameManager.uma_node:
			GameManager.uma_node.freeze()
	else:
		if GameManager.uma_node:
			GameManager.uma_node.unfreeze()

func _rebuild_pages() -> void:
	all_pages.clear()
	all_pages.append_array(BASE_PAGES)
	for sketch in GameManager.get_sketches():
		all_pages.append(sketch)

func _show_page(idx: int) -> void:
	if all_pages.is_empty():
		return
	idx = clampi(idx, 0, all_pages.size() - 1)
	current_page = idx
	var page: Dictionary = all_pages[idx]
	if page.has("texture"):
		sketch_display.texture = load(page["texture"])
	else:
		sketch_display.texture = null
	page_text.text = page.get("text", page.get("description", ""))
	page_indicator.text = "%d / %d" % [idx + 1, all_pages.size()]
	btn_prev.disabled = (idx == 0)
	btn_next.disabled = (idx == all_pages.size() - 1)

func _prev_page() -> void:
	_show_page(current_page - 1)

func _next_page() -> void:
	_show_page(current_page + 1)
```

---

## 11. SISTEMA DE MEMORIA Y FLAGS

*(El sistema de flags está en `flags_manager.gd` — sección 3.2)*

### 11.1 Tabla de Flags → Efecto en Diálogos y Final

| Flag | Valor | Efecto |
|---|---|---|
| `recogió_disco` | true | Pablo: "Sabía que ibas a reconocer esa canción." |
| `se_sentó_banco_plaza` | true | Pablo: "Vi cuando te quedaste en la plaza aunque llovía." |
| `comparó_notas` | true | Pablo: "¿Comparaste las dos notas?" |
| `dibujó_plaza` | true | Pablo: "Te vi dibujar en el banco." |
| `dejó_sombrero_gancho` + `recuperó_sombrero=false` | — | Pablo: "¿Dónde dejaste el sombrero?" |
| `nelida_nivel` >= 3 | — | Pablo: "¿Hablaste con la señora del balcón?" |
| `leyó_placa_bardi` | true | Activa línea extra en diálogo con Valentina y Nélida |
| `escuchó_voz_elena` | true | Texto flotante en Acto V: frase de Elena |
| `charly_escuchó_canción` | true | Activa fragmento de melodía frag2 |
| `escuchó_adolescentes` | true | Activa fragmento de melodía frag3 |

### 11.2 Sistema de Textos Flotantes — `floating_text.gd`

```gdscript
# res://scripts/systems/floating_text.gd
extends Node2D
class_name FloatingText

@onready var label: Label = $Label

func _ready() -> void:
	label.modulate.a = 0.0

func show_text(text: String, duration: float = 5.0) -> void:
	label.text = text
	var tween := create_tween()
	tween.tween_property(label, "modulate:a", 1.0, 1.0)
	tween.tween_interval(duration - 2.0)
	tween.tween_property(label, "modulate:a", 0.0, 1.0)
	await tween.finished
	queue_free()
```

**Uso en `espacio_final.gd`:**

```gdscript
func _spawn_floating_texts() -> void:
	var texts_to_show: Array[Dictionary] = []

	# Construir lista según flags
	if FlagsManager.get_flag("habló_tomas"):
		texts_to_show.append({ "text": "Soñé que ibas a encontrar algo.", "pos": Vector2(40, 80) })
	if FlagsManager.get_flag("habló_elena"):
		texts_to_show.append({ "text": "A veces salir un ratito cambia las cosas.", "pos": Vector2(180, 120) })
	if FlagsManager.get_flag("habló_mario"):
		texts_to_show.append({ "text": "Vas a encontrar lo que buscás.", "pos": Vector2(60, 160) })
	if FlagsManager.get_flag("habló_ernesto"):
		texts_to_show.append({ "text": "Se fueron. Pero van a volver.", "pos": Vector2(200, 60) })
	if FlagsManager.get_flag("escuchó_voz_elena"):
		texts_to_show.append({ "text": "Hay cosas que no se entienden hasta que las sentís.", "pos": Vector2(80, 200) })
	if FlagsManager.get_flag("guardó_nota_padre_1"):
		texts_to_show.append({ "text": "Si ves algo que te llama la atención, seguí por ahí.", "pos": Vector2(150, 180) })
	if FlagsManager.get_flag("se_sentó_banco_plaza"):
		texts_to_show.append({ "text": "Cuando llueve todo suena más lindo.", "pos": Vector2(220, 140) })

	# Mostrar con delays
	var delay: float = 0.0
	for entry in texts_to_show:
		await get_tree().create_timer(delay).timeout
		var ft: FloatingText = preload("res://scenes/systems/FloatingText.tscn").instantiate()
		ft.position = entry["pos"]
		add_child(ft)
		ft.show_text(entry["text"], 5.0)
		delay += 3.5
```

---

## 12. SISTEMA DE MÚSICA Y RESONANCIA

### 12.1 `music_manager.gd` — Completo

```gdscript
# res://scripts/systems/music_manager.gd
extends Node

# ─────────────────────────────────────────────
# TRACKS REGISTRADOS
# ─────────────────────────────────────────────
const TRACKS: Dictionary = {
	"menu_theme":               "res://assets/audio/music/menu_theme.ogg",
	"casa_ambient":             "res://assets/audio/music/casa_ambient.ogg",
	"barrio_sunny":             "res://assets/audio/music/barrio_sunny.ogg",
	"plaza_sunny":              "res://assets/audio/music/plaza_sunny.ogg",
	"plaza_cloudy":             "res://assets/audio/music/plaza_cloudy.ogg",
	"plaza_rain":               "res://assets/audio/music/plaza_rain.ogg",
	"zona_interior":            "res://assets/audio/music/zona_interior.ogg",
	"antesala":                 "res://assets/audio/music/antesala.ogg",
	"espacio_final_ambient":    "res://assets/audio/music/espacio_final_ambient.ogg",
	"lavuelta_completa":        "res://assets/audio/music/lavuelta_completa.ogg"
}

const FRAGMENTS: Dictionary = {
	"frag1_cocina":            "res://assets/audio/music/lavuelta_frag1_guitarra.ogg",
	"frag2_kiosco":            "res://assets/audio/music/lavuelta_frag2_kiosco.ogg",
	"frag3_adolescentes":      "res://assets/audio/music/lavuelta_frag3_adolescentes.ogg",
	"frag4_intermedia":        "res://assets/audio/music/lavuelta_frag4_intermedia.ogg"
}

# ─────────────────────────────────────────────
# PLAYERS (crossfade A/B)
# ─────────────────────────────────────────────
var player_a: AudioStreamPlayer
var player_b: AudioStreamPlayer
var active_player: AudioStreamPlayer
var current_track_id: String = ""

# Player dedicado para fragmentos (por encima de la música)
var fragment_player: AudioStreamPlayer

@export var crossfade_duration: float = 2.5
@export var master_volume: float = 1.0

signal track_changed(track_id: String)
signal fragment_played(fragment_id: String)

# ─────────────────────────────────────────────
# SETUP
# ─────────────────────────────────────────────
func _ready() -> void:
	player_a = AudioStreamPlayer.new()
	player_b = AudioStreamPlayer.new()
	fragment_player = AudioStreamPlayer.new()
	player_a.bus = "Music"
	player_b.bus = "Music"
	fragment_player.bus = "Music"
	player_a.volume_db = -80.0
	player_b.volume_db = -80.0
	fragment_player.volume_db = -80.0
	add_child(player_a)
	add_child(player_b)
	add_child(fragment_player)
	active_player = player_a

# ─────────────────────────────────────────────
# PLAY TRACK
# ─────────────────────────────────────────────
func play_track(track_id: String, fade: bool = true) -> void:
	if track_id == current_track_id:
		return
	if not TRACKS.has(track_id):
		push_warning("MusicManager: Track no encontrada: '%s'" % track_id)
		return

	current_track_id = track_id
	var next_player: AudioStreamPlayer = player_b if active_player == player_a else player_a

	var stream: AudioStream = load(TRACKS[track_id])
	next_player.stream = stream
	next_player.play()

	var tween := create_tween()
	tween.set_parallel(true)
	if fade:
		tween.tween_property(active_player, "volume_db", -80.0, crossfade_duration)
		tween.tween_property(next_player,   "volume_db", 0.0,   crossfade_duration)
	else:
		active_player.volume_db = -80.0
		active_player.stop()
		next_player.volume_db = 0.0

	await tween.finished
	active_player.stop()
	active_player = next_player
	track_changed.emit(track_id)

func stop_music(fade: bool = true) -> void:
	current_track_id = ""
	if fade:
		var tween := create_tween()
		tween.tween_property(active_player, "volume_db", -80.0, crossfade_duration)
		await tween.finished
	active_player.stop()

# ─────────────────────────────────────────────
# PLAY FRAGMENT (no interrumpe la música base)
# ─────────────────────────────────────────────
func play_fragment(fragment_id: String) -> void:
	if not FRAGMENTS.has(fragment_id):
		push_warning("MusicManager: Fragmento no encontrado: '%s'" % fragment_id)
		return
	fragment_player.stream = load(FRAGMENTS[fragment_id])
	fragment_player.volume_db = 0.0
	fragment_player.play()
	GameManager.register_melody_fragment(fragment_id)
	fragment_played.emit(fragment_id)

# ─────────────────────────────────────────────
# VOLUMEN
# ─────────────────────────────────────────────
func set_music_volume(value: float) -> void:
	# value: 0.0–1.0
	master_volume = value
	AudioServer.set_bus_volume_db(AudioServer.get_bus_index("Music"), linear_to_db(value))
```

### 12.2 Tabla de Tracks por Zona y Estado

| Zona | Estado Clima | Track | Fragmento |
|---|---|---|---|
| Menú Principal | — | `menu_theme` | — |
| Casa | — | `casa_ambient` | `frag1_cocina` (si detonante 2) |
| Barrio | Soleado | `barrio_sunny` | `frag2_kiosco` (si para en Charly) |
| Plaza | Soleado | `plaza_sunny` | — |
| Plaza | Nublado | `plaza_cloudy` | `frag3_adolescentes` (si se acerca) |
| Plaza | Lluvia | `plaza_rain` | — |
| Zona Interior | — | `zona_interior` | `frag4_intermedia` (al activar ranura) |
| Antesala | — | `antesala` | — |
| Espacio Final | — | `espacio_final_ambient` → `lavuelta_completa` | — |

---

## 13. MAPAS: ESTRUCTURA Y TILESETS

### 13.1 Estructura Base de Toda Escena de Mundo

```gdscript
# res://scripts/world/base_zone.gd
# Script base para todas las zonas — extender en cada zona
extends Node2D
class_name BaseZone

@export var zone_key: String = ""
@export var initial_weather: int = 0  # WeatherSystem.State

@onready var weather_system: WeatherSystem = $WeatherLayer/WeatherSystem
@onready var hud: HUD = $HUD
@onready var dialog_box = $HUD/DialogBox
@onready var transition_overlay: ColorRect = $HUD/TransitionOverlay

func _ready() -> void:
	GameManager.set_current_zone(zone_key)
	weather_system.set_weather(initial_weather as WeatherSystem.State, true)
	_setup_music()
	_connect_triggers()
	_fade_in()

func _setup_music() -> void:
	pass  # Sobreescribir en cada zona

func _connect_triggers() -> void:
	pass  # Sobreescribir en cada zona

func _fade_in() -> void:
	transition_overlay.color.a = 1.0
	var tween := create_tween()
	tween.tween_property(transition_overlay, "color:a", 0.0, 0.8)

func _fade_out_and_load(scene_path: String) -> void:
	if GameManager.uma_node:
		GameManager.uma_node.freeze()
	var tween := create_tween()
	tween.tween_property(transition_overlay, "color:a", 1.0, 0.8)
	await tween.finished
	GameManager.save_game()
	GameManager.change_scene(scene_path)
```

### 13.2 Estructura de Nodos Común en Todas las Zonas

```
[NombreZona] (Node2D)
├── extends base_zone.gd
│
├── TileMapLayers (Node2D)
│   ├── LayerSuelo (TileMapLayer)       z_index: -10
│   ├── LayerDecoFondo (TileMapLayer)   z_index: -5, y_sort_enabled: false
│   ├── LayerFrente (TileMapLayer)      z_index: 0,  y_sort_enabled: true
│   └── LayerTecho (TileMapLayer)       z_index: 10
│
├── Characters (Node2D)
│   └── y_sort_enabled: true
│   └── [Uma + NPCs instanciados aquí]
│
├── Objects (Node2D)
│   └── y_sort_enabled: true
│   └── [PickupItems + Interactables aquí]
│
├── Triggers (Node2D)
│   └── [Area2D invisibles para eventos]
│
├── WeatherLayer (CanvasLayer)
│   ├── layer: 8
│   └── WeatherSystem (instancia de WeatherSystem.tscn)
│
└── HUD (CanvasLayer)
    ├── layer: 10
    ├── instancia de HUD.tscn
    ├── instancia de DialogBox.tscn
    ├── instancia de InventoryPanel.tscn
    ├── instancia de Notebook.tscn
    ├── instancia de PauseMenu.tscn
    └── TransitionOverlay (ColorRect)
        ├── anchors_preset: FULL_RECT
        ├── color: Color(0,0,0,1)
        └── mouse_filter: MOUSE_FILTER_IGNORE
```

### 13.3 Dimensiones de Mapas

| Zona | Ancho (tiles) | Alto (tiles) | Tile Size | Notas |
|---|---|---|---|---|
| Casa | 20 | 14 | 16×16 | 4 habitaciones, scrolling limitado |
| Barrio | 80 | 24 | 16×16 | Horizontal, 4 cuadras completas |
| Plaza | 36 | 36 | 16×16 | Cuadrada, open space |
| Zona Interior | 24 | 56 | 16×16 | Vertical, un solo camino con ramales |
| Antesala | 20 | 20 | 16×16 | Patio cuadrado cerrado |
| Espacio Final | 18 | 16 | 16×16 | Sala con abertura lateral |

### 13.4 Tilesets — Descripción de Tiles Necesarios

#### `tileset_exterior.png` — Grilla 8×8 tiles (16×16px cada uno)

```
Fila 0: Veredas
  [00] vereda_lisa          [01] vereda_grieta         [02] vereda_mancha
  [03] vereda_esquina_ext   [04] vereda_esquina_int     [05] cordón_calle
  [06] adoquín              [07] adoquín_roto

Fila 1: Calles
  [08] calle_asfalto        [09] calle_linea_central    [10] calle_bache
  [11] calle_charco_lluvia  [12] cruce_peatonal         [13] calle_marca_vieja

Fila 2: Césped y tierra
  [16] cesped_corto         [17] cesped_largo            [18] cesped_con_flor
  [19] tierra               [20] tierra_mojada           [21] cesped_borde

Fila 3: Baldosas de plaza
  [24] baldosa_entera       [25] baldosa_rota            [26] baldosa_musgo
  [27] baldosa_humeda       [28] fuente_borde            [29] fuente_interior_seco

Fila 4: Fachadas bajas
  [32] pared_revoque_claro  [33] pared_revoque_oscuro    [34] pared_ladrillo
  [35] pared_azulejo_blanco [36] pared_con_grieta        [37] pared_humedad

Fila 5: Ventanas y puertas
  [40] ventana_cerrada      [41] ventana_persiana_baja   [42] ventana_abierta
  [43] ventana_luz_on       [44] puerta_madera_cerrada   [45] puerta_entreabierta
  [46] reja_baja            [47] alero_cornisa

Fila 6: Vegetación
  [48] arbol_base_grueso    [49] arbol_copa_baja_A       [50] arbol_copa_baja_B
  [51] arbol_copa_alta_A    [52] arbol_copa_alta_B       [53] arbusto_pequeño
  [54] planta_maceta        [55] enredadera_pared

Fila 7: Mobiliario urbano
  [56] banco_vacio          [57] banco_con_persona       [58] farola_apagada
  [59] farola_encendida     [60] poste_luz               [61] buzón_viejo
  [62] parada_colectivo     [63] kiosco_estructura
```

#### `tileset_interior.png`

```
Fila 0: Pisos
  piso_madera_A    piso_madera_B    piso_ceramica_blanca
  piso_ceramica_color  alfombra_borde  alfombra_centro

Fila 1: Paredes interiores
  pared_blanca     pared_empapelada  pared_con_cuadro
  pared_esquina    zócalo_madera

Fila 2: Muebles (1 tile)
  mesa_cocina      silla_madera  heladera_cerrada
  estante_vacio    estante_libros  cama_simple

Fila 3: Muebles (2 tiles ancho)
  sillón_izq  sillón_der   escritorio_izq  escritorio_der
  tv_apagada_izq  tv_apagada_der

Fila 4: Decoración
  maceta_interior  cuadro_pared  espejo_pared
  lámpara_mesa     cortina_lateral  cortina_centro
```

---

## 14. SCRIPTS DE ZONA: COMPLETOS

### 14.1 `casa.gd`

```gdscript
# res://scripts/world/casa.gd
extends BaseZone

@onready var uma:           Uma = $Characters/Uma
@onready var npc_tomas:     NPCBase = $Characters/NPC_Tomas
@onready var npc_elena:     NPCBase = $Characters/NPC_Elena

# Triggers
@onready var trigger_ventana:  Area2D = $Triggers/TriggerVentana
@onready var trigger_cuaderno: Area2D = $Triggers/TriggerCuaderno
@onready var trigger_nota:     Area2D = $Triggers/TriggerNota
@onready var trigger_salida:   Area2D = $Triggers/TriggerSalida
@onready var trigger_patio:    Area2D = $Triggers/TriggerPatio

# Objetos
@onready var estante_insp:  InspectableObject = $Objects/EstanteDiscos
@onready var foto_heladera: InspectableObject = $Objects/FotoHeladera
@onready var foto_mesita:   InspectableObject = $Objects/FotoMesita

var detonante_activado: bool = false

func _setup_music() -> void:
	MusicManager.play_track("casa_ambient")

func _connect_triggers() -> void:
	trigger_ventana.body_entered.connect(_on_ventana)
	trigger_cuaderno.body_entered.connect(_on_cuaderno)
	trigger_nota.body_entered.connect(_on_nota)
	trigger_salida.body_entered.connect(_on_salida)
	trigger_patio.body_entered.connect(_on_patio)

func _on_ventana(body: Node2D) -> void:
	if body != uma or detonante_activado:
		return
	FlagsManager.set_flag("detonante_brillo", true)
	GameManager.register_melody_fragment("frag1_cocina")
	uma.show_thought("Hay algo que brilla en la vereda. No puedo distinguir qué es.")
	await get_tree().create_timer(2.5).timeout
	dialog_box.start_dialog("uma_ventana_brillo")
	_activar_detonante()

func _on_cuaderno(body: Node2D) -> void:
	if body != uma:
		return
	FlagsManager.set_flag("inspeccionó_cuarto", true)
	if not detonante_activado:
		FlagsManager.set_flag("detonante_cuaderno", true)
		dialog_box.start_dialog("uma_cuaderno_pagina_misteriosa")
		await dialog_box.dialog_finished
		_activar_detonante()
	else:
		dialog_box.start_dialog("uma_cuaderno_ya_visto")

func _on_nota(body: Node2D) -> void:
	if body != uma:
		return
	dialog_box.start_dialog("uma_nota_padre_decision")
	# El diálogo maneja el add_item via "choices" + "add_item" field

func _on_patio(body: Node2D) -> void:
	if body != uma:
		return
	# Detonante alternativo: si ya vio el cuaderno en patio
	if not detonante_activado and FlagsManager.get_flag("inspeccionó_cuarto"):
		FlagsManager.set_flag("detonante_cuaderno", true)
		dialog_box.start_dialog("uma_patio_cuaderno")
		await dialog_box.dialog_finished
		_activar_detonante()
	else:
		uma.show_thought("El patio está quieto. Hay una planta que está floreciendo.")

func _activar_detonante() -> void:
	if detonante_activado:
		return
	detonante_activado = true
	FlagsManager.set_flag("salió_casa", false)  # se setea en true al cruzar puerta
	await get_tree().create_timer(0.8).timeout
	dialog_box.start_dialog("uma_decision_salir")

func _on_salida(body: Node2D) -> void:
	if body != uma:
		return
	if not detonante_activado:
		dialog_box.start_dialog("uma_puerta_bloqueada")
		return
	FlagsManager.set_flag("salió_casa", true)
	_fade_out_and_load("res://scenes/world/Barrio.tscn")
```

---

### 14.2 `barrio.gd`

```gdscript
# res://scripts/world/barrio.gd
extends BaseZone

@onready var uma:              Uma     = $Characters/Uma
@onready var npc_nelida:       NPCBase = $Characters/NPC_Nelida
@onready var npc_charly:       NPCBase = $Characters/NPC_Charly
@onready var npc_mario:        NPCBase = $Characters/NPC_Mario
@onready var npc_ramiro:       NPCBase = $Characters/NPC_Ramiro
@onready var npc_nico:         NPCBase = $Characters/NPC_Nico

@onready var item_fragmento:   PickupItem = $Objects/FragmentoDiscoPiso
@onready var tienda_obj:       InspectableObject = $Objects/TiendaCerrada
@onready var foto_tienda:      PickupItem = $Objects/FotoPolaroid
@onready var placa_bardi:      InspectableObject = $Objects/PlacaBardi
@onready var timbre_bardi:     InspectableObject = $Objects/TimbreBardi
@onready var gato_obj:         InspectableObject = $Objects/Gato

@onready var trigger_kiosco_zona: Area2D = $Triggers/TriggerKioscoZona
@onready var trigger_sombra:      Area2D = $Triggers/TriggerSombra
@onready var trigger_salida_sur:  Area2D = $Triggers/TriggerSalidaSur
@onready var trigger_salida_norte: Area2D = $Triggers/TriggerSalidaNorte

var kiosco_melodia_played: bool = false

func _setup_music() -> void:
	MusicManager.play_track("barrio_sunny")

func _connect_triggers() -> void:
	trigger_kiosco_zona.body_entered.connect(_on_kiosco_zona)
	trigger_sombra.body_entered.connect(_on_sombra)
	trigger_salida_sur.body_entered.connect(_on_salida_sur)
	trigger_salida_norte.body_entered.connect(_on_salida_norte)

	# Configurar NPC Nélida
	npc_nelida.dialog_ids = ["npc_nelida_01", "npc_nelida_02", "npc_nelida_03"]
	npc_nelida.flag_increment = "nelida_nivel"

	# Configurar NPC Charly
	npc_charly.dialog_ids = ["npc_charly_01", "npc_charly_02"]
	npc_charly.flag_on_talk = "charly_nivel"

	# Configurar NPC Mario
	npc_mario.dialog_ids = ["npc_mario_01", "npc_mario_02"]
	npc_mario.flag_on_talk = "habló_mario"

	# Configurar Ramiro y Nico
	npc_ramiro.dialog_ids = ["npc_ramiro_01", "npc_ramiro_02"]
	npc_ramiro.flag_increment = "ramiro_nico_nivel"
	npc_ramiro.flag_on_talk = "habló_ramiro_nico"

	# Foto de la tienda: solo recogible si inspeccionó el cartel antes
	foto_tienda.requires_flag = ""  # se gestiona desde tienda_obj

	# Placa de Bardi
	placa_bardi.dialog_id = "bardi_placa_inspeccion"
	placa_bardi.flag_on_inspect = "leyó_placa_bardi"

	# Timbre de Bardi
	timbre_bardi.dialog_id = "bardi_timbre"
	timbre_bardi.flag_on_inspect = "timbró_bardi"

	# Gato
	gato_obj.dialog_id = "barrio_gato"
	gato_obj.flag_on_inspect = "acarició_gato"

func _on_kiosco_zona(body: Node2D) -> void:
	if body != uma or kiosco_melodia_played:
		return
	kiosco_melodia_played = true
	FlagsManager.set_flag("charly_escuchó_canción", true)
	GameManager.register_melody_fragment("frag2_kiosco")
	MusicManager.play_fragment("frag2_kiosco")
	uma.show_thought("Una canción en la radio del kiosco. La conozco de algún lado.")
	await get_tree().create_timer(4.0).timeout
	uma.show_thought("Se cortó justo.")

func _on_sombra(body: Node2D) -> void:
	if body != uma:
		return
	uma.show_thought("Acá hace más fresco. El arbolado viejo es lo mejor que tiene este barrio.")

func _on_salida_sur(body: Node2D) -> void:
	if body != uma:
		return
	_fade_out_and_load("res://scenes/world/Plaza.tscn")

func _on_salida_norte(body: Node2D) -> void:
	if body != uma:
		return
	_fade_out_and_load("res://scenes/world/Casa.tscn")
```

---

### 14.3 `plaza.gd`

```gdscript
# res://scripts/world/plaza.gd
extends BaseZone

@onready var uma:          Uma            = $Characters/Uma
@onready var npc_valentina: NPCBase       = $Characters/NPC_Valentina
@onready var npc_ernesto:  NPCBase        = $Characters/NPC_Ernesto
@onready var weather_sys:  WeatherSystem  = $WeatherLayer/WeatherSystem

@onready var item_disco:   PickupItem    = $Objects/DiscoPlazo
@onready var bench_uma:    Bench         = $Objects/BancoUma

@onready var trigger_centro:     Area2D = $Triggers/TriggerCentro
@onready var trigger_adolescentes: Area2D = $Triggers/TriggerAdolescentes
@onready var trigger_salida_sur: Area2D = $Triggers/TriggerSalidaSur
@onready var trigger_salida_norte: Area2D = $Triggers/TriggerSalidaNorte

var tiempo_en_plaza: float = 0.0
var fase_clima: int = 0  # 0=sunny 1=cloudy 2=rain
var adolescentes_escuchados: bool = false

func _ready() -> void:
	super()
	bench_uma.draw_trigger = true  # activa lógica de auto-dibujar
	_setup_npcs()

func _setup_music() -> void:
	MusicManager.play_track("plaza_sunny")

func _connect_triggers() -> void:
	trigger_centro.body_entered.connect(_on_centro)
	trigger_adolescentes.body_entered.connect(_on_adolescentes)
	trigger_salida_sur.body_entered.connect(_on_salida_sur)
	trigger_salida_norte.body_entered.connect(_on_salida_norte)

func _setup_npcs() -> void:
	npc_valentina.dialog_ids = ["npc_valentina_01", "npc_valentina_02", "npc_valentina_03"]
	npc_valentina.flag_increment = "valentina_nivel"
	npc_ernesto.dialog_ids = ["npc_ernesto_01", "npc_ernesto_02"]
	npc_ernesto.flag_on_talk = "habló_ernesto"

func _process(delta: float) -> void:
	tiempo_en_plaza += delta
	_update_clima()

func _update_clima() -> void:
	match fase_clima:
		0:  # SUNNY → CLOUDY
			var trigger_nublado: bool = (
				tiempo_en_plaza > 60.0 or
				FlagsManager.get_flag("valentina_nivel") >= 1
			)
			if trigger_nublado:
				fase_clima = 1
				FlagsManager.set_flag("clima_nublado_activado", true)
				weather_sys.set_weather(WeatherSystem.State.CLOUDY)
				MusicManager.play_track("plaza_cloudy")
				uma.show_thought("Las nubes llegaron despacio. Mejor.")
		1:  # CLOUDY → RAIN
			var trigger_lluvia: bool = (
				tiempo_en_plaza > 100.0 or
				FlagsManager.get_flag("recogió_disco")
			)
			if trigger_lluvia:
				fase_clima = 2
				FlagsManager.set_flag("clima_lluvia_activado", true)
				weather_sys.set_weather(WeatherSystem.State.RAIN)
				MusicManager.play_track("plaza_rain")
				uma.show_thought("Lluvia suave. Me quedo.")

func _on_centro(body: Node2D) -> void:
	if body != uma:
		return
	FlagsManager.set_flag("fue_al_centro_plaza", true)
	uma.show_thought("Sin sombra en el centro. Esto es lo que se siente estar expuesta.")

func _on_adolescentes(body: Node2D) -> void:
	if body != uma or adolescentes_escuchados:
		return
	adolescentes_escuchados = true
	FlagsManager.set_flag("escuchó_adolescentes", true)
	GameManager.register_melody_fragment("frag3_adolescentes")
	MusicManager.play_fragment("frag3_adolescentes")
	dialog_box.start_dialog("uma_adolescentes_musica")

func _on_salida_sur(body: Node2D) -> void:
	if body != uma:
		return
	_fade_out_and_load("res://scenes/world/ZonaInterior.tscn")

func _on_salida_norte(body: Node2D) -> void:
	if body != uma:
		return
	_fade_out_and_load("res://scenes/world/Barrio.tscn")
```

---

### 14.4 `zona_interior.gd`

```gdscript
# res://scripts/world/zona_interior.gd
extends BaseZone

@onready var uma: Uma = $Characters/Uma

# Objetos-espejo
@onready var planta_espejo: InspectableObject = $Objects/PlantaSinMaceta
@onready var radio_espejo:  InspectableObject = $Objects/RadioSinNadie
@onready var banco_espejo:  InspectableObject = $Objects/BancoReplicado

# Activadores
@onready var disc_slot:  DiscSlot  = $Objects/RanuraDiscoIntermedia
@onready var hat_hook:   HatHook   = $Objects/GanchoSombrero
@onready var photo_frame: PhotoFrame = $Objects/MarcoVacio

# Triggers
@onready var trigger_puerta_voz:  Area2D = $Triggers/TriggerPuertaVoz
@onready var trigger_pasaje_final: Area2D = $Triggers/TriggerPasajeFinal

var puerta_activada: bool = false

func _setup_music() -> void:
	MusicManager.play_track("zona_interior")

func _connect_triggers() -> void:
	trigger_puerta_voz.body_entered.connect(_on_puerta_voz)
	trigger_pasaje_final.body_entered.connect(_on_pasaje_final)

	planta_espejo.dialog_id = "uma_planta_sin_maceta"
	planta_espejo.flag_on_inspect = "inspeccionó_planta_espejo"
	radio_espejo.dialog_id  = "uma_radio_nadie"
	radio_espejo.flag_on_inspect = "inspeccionó_radio_espejo"
	banco_espejo.dialog_id  = "uma_banco_repetido"
	banco_espejo.flag_on_inspect = "inspeccionó_banco_espejo"

func _on_puerta_voz(body: Node2D) -> void:
	if body != uma or puerta_activada:
		return
	puerta_activada = true
	FlagsManager.set_flag("escuchó_voz_elena", true)
	uma.freeze()
	dialog_box.start_dialog("puerta_voz_madre")
	await dialog_box.dialog_finished
	uma.unfreeze()

func _on_pasaje_final(body: Node2D) -> void:
	if body != uma:
		return
	_fade_out_and_load("res://scenes/world/Antesala.tscn")
```

---

### 14.5 `antesala.gd`

```gdscript
# res://scripts/world/antesala.gd
extends BaseZone

@onready var uma: Uma = $Characters/Uma
@onready var nota_banco: NoteOnSurface = $Objects/NotaBanco
@onready var farola_obj: InspectableObject = $Objects/FarolaApagada
@onready var trigger_puerta: Area2D = $Triggers/TriggerPuertaFinal

var _idle_wait_timer: float = 0.0
var _idle_triggered: bool = false
const IDLE_TO_ADVANCE: float = 120.0  # 2 min → Uma avanza sola

func _setup_music() -> void:
	MusicManager.play_track("antesala")

func _connect_triggers() -> void:
	trigger_puerta.body_entered.connect(_on_puerta_final)
	farola_obj.dialog_id = "antesala_farola"
	nota_banco.dialog_id_nota = "antesala_nota_padre_2"

func _process(delta: float) -> void:
	if _idle_triggered:
		return
	if GameManager.uma_node and not GameManager.uma_node.can_move:
		return
	# Verificar si Uma está quieta cerca de la puerta
	if trigger_puerta.overlaps_body(uma):
		_idle_wait_timer += delta
		if _idle_wait_timer >= IDLE_TO_ADVANCE:
			_idle_triggered = true
			_uma_avanza_sola()

func _uma_avanza_sola() -> void:
	FlagsManager.set_flag("esperó_sola_antesala", true)
	dialog_box.start_dialog("antesala_duda_sola")
	await dialog_box.dialog_finished
	_fade_out_and_load("res://scenes/world/EspacioFinal.tscn")

func _on_puerta_final(body: Node2D) -> void:
	if body != uma:
		return
	_idle_triggered = true
	_fade_out_and_load("res://scenes/world/EspacioFinal.tscn")
```

---

### 14.6 `espacio_final.gd` — Completo

```gdscript
# res://scripts/world/espacio_final.gd
extends BaseZone

@onready var uma:         Uma   = $Characters/Uma
@onready var pablo:       CharacterBody2D = $Characters/Pablo
@onready var radio_obj:   DiscSlot = $Objects/RadioAntiguaSlot
@onready var bench_final: Bench    = $Objects/SillaCentral
@onready var weather_sys: WeatherSystem = $WeatherLayer/WeatherSystem

# Estado
var cancion_sonando: bool = false
var pablo_visible:   bool = false
var secuencia_final: bool = false

func _ready() -> void:
	super()
	pablo.visible = false
	radio_obj.is_radio = true
	radio_obj.disc_placed.connect(_on_radio_activada)

func _setup_music() -> void:
	MusicManager.play_track("espacio_final_ambient")

func _connect_triggers() -> void:
	weather_sys.set_weather(WeatherSystem.State.RAIN, true)

# ─────────────────────────────────────────────
# RADIO ACTIVADA
# ─────────────────────────────────────────────
func _on_radio_activada() -> void:
	if cancion_sonando:
		return
	cancion_sonando = true
	FlagsManager.set_flag("puso_disco_radio_final", true)

	# La música la pone DiscSlot vía MusicManager.play_track("lavuelta_completa")
	# Aquí manejamos lo que pasa durante la canción

	uma.sit_down()
	await get_tree().create_timer(2.0).timeout
	_spawn_floating_texts()
	await get_tree().create_timer(30.0).timeout  # ~mitad de la canción
	FlagsManager.set_flag("escuchó_canción_completa", true)
	await get_tree().create_timer(15.0).timeout  # hacia el final
	_aparecer_pablo()

# ─────────────────────────────────────────────
# TEXTOS FLOTANTES
# ─────────────────────────────────────────────
func _spawn_floating_texts() -> void:
	var texts: Array[Dictionary] = []

	if FlagsManager.get_flag("habló_tomas"):
		texts.append({ "text": "Soñé que ibas a encontrar algo.", "pos": Vector2(35, 70) })
	if FlagsManager.get_flag("habló_elena"):
		texts.append({ "text": "A veces salir un ratito cambia las cosas.", "pos": Vector2(190, 110) })
	if FlagsManager.get_flag("habló_mario"):
		texts.append({ "text": "Vas a encontrar lo que buscás.", "pos": Vector2(55, 155) })
	if FlagsManager.get_flag("habló_ernesto"):
		texts.append({ "text": "Se fueron. Pero van a volver.", "pos": Vector2(200, 55) })
	if FlagsManager.get_flag("escuchó_voz_elena"):
		texts.append({ "text": "Hay cosas que no se entienden hasta que las sentís.", "pos": Vector2(70, 195) })
	if FlagsManager.get_flag("guardó_nota_padre_1"):
		texts.append({ "text": "Si ves algo que te llama la atención, seguí por ahí.", "pos": Vector2(145, 175) })
	if FlagsManager.get_flag("se_sentó_banco_plaza"):
		texts.append({ "text": "Cuando llueve todo suena más lindo.", "pos": Vector2(220, 135) })
	if FlagsManager.get_flag("timbró_bardi"):
		texts.append({ "text": "Alguien que vivió acá grabó esto.", "pos": Vector2(30, 140) })
	if not GameManager.uma_has_hat:
		texts.append({ "text": "Sin el sombrero todo está un poco más cerca.", "pos": Vector2(180, 175) })

	var delay: float = 0.0
	for entry in texts:
		await get_tree().create_timer(delay).timeout
		var ft: Node = preload("res://scenes/systems/FloatingText.tscn").instantiate()
		ft.position = entry["pos"]
		add_child(ft)
		ft.show_text(entry["text"], 6.0)
		delay += 3.8

# ─────────────────────────────────────────────
# APARICIÓN DE PABLO
# ─────────────────────────────────────────────
func _aparecer_pablo() -> void:
	if pablo_visible:
		return
	pablo_visible = true

	uma.stand_up()
	await get_tree().create_timer(1.5).timeout

	FlagsManager.set_flag("pablo_apareció", true)
	pablo.appear()
	await get_tree().create_timer(2.5).timeout

	_iniciar_dialogo_pablo()

# ─────────────────────────────────────────────
# DIÁLOGO FINAL
# ─────────────────────────────────────────────
func _iniciar_dialogo_pablo() -> void:
	if secuencia_final:
		return
	secuencia_final = true

	var dialog_id := pablo.get_dialog_id()
	dialog_box.start_dialog(dialog_id)
	await dialog_box.dialog_finished

	# Después del diálogo de Pablo, viene la respuesta de Uma
	dialog_box.start_dialog("uma_respuesta_final")
	dialog_box.choice_made.connect(_on_respuesta_uma, CONNECT_ONE_SHOT)
	await dialog_box.dialog_finished

	_iniciar_cierre()

func _on_respuesta_uma(index: int, choice_data: Dictionary) -> void:
	var respuesta: String = choice_data.get("respuesta_id", "silencio")
	FlagsManager.set_flag("respuesta_pablo", respuesta)

# ─────────────────────────────────────────────
# CIERRE
# ─────────────────────────────────────────────
func _iniciar_cierre() -> void:
	FlagsManager.set_flag("juego_completo", true)
	GameManager.save_game()

	# Pausa antes del fade
	await get_tree().create_timer(6.0).timeout

	# Fade a negro
	var tween := create_tween()
	tween.tween_property(transition_overlay, "color:a", 1.0, 5.0)
	await tween.finished

	# Frase final
	_mostrar_frase_final()

func _mostrar_frase_final() -> void:
	var label := Label.new()
	label.text = "A veces salir un ratito cambia las cosas."
	label.add_theme_font_override("font", preload("res://assets/fonts/fuente_monologos.ttf"))
	label.add_theme_font_size_override("font_size", 14)
	label.add_theme_color_override("font_color", Color(1, 1, 1, 0.0))
	label.set_anchors_and_offsets_preset(Control.PRESET_CENTER)
	transition_overlay.add_child(label)

	var tween := create_tween()
	tween.tween_property(label, "theme_override_colors/font_color", Color(1, 1, 1, 1.0), 2.5)
	await tween.finished
	await get_tree().create_timer(5.0).timeout

	# Ir al resumen final
	GameManager.change_scene("res://scenes/ui/EndSummary.tscn")
```

---

## 15. DIÁLOGOS COMPLETOS EN JSON

### `res://data/dialogos.json`

```json
[

  // ═══════════════════════════════════════════
  // PRÓLOGO — CASA
  // ═══════════════════════════════════════════

  {
    "id": "uma_ventana_brillo",
    "lines": [
      { "speaker": "", "text": "Hay algo que brilla en la vereda. No puedo distinguir qué es.", "next": "uma_ventana_brillo_02" }
    ]
  },
  {
    "id": "uma_ventana_brillo_02",
    "lines": [
      { "speaker": "", "text": "Probablemente nada. Un vidrio, una moneda. Pero brilla.", "next": null }
    ]
  },

  {
    "id": "uma_cuaderno_pagina_misteriosa",
    "lines": [
      { "speaker": "", "text": "El cuaderno está abierto en una página que no recuerdo haber usado.", "next": "uma_cuaderno_pagina_02" }
    ]
  },
  {
    "id": "uma_cuaderno_pagina_02",
    "lines": [
      { "speaker": "", "text": "Hay un dibujo. Una forma abstracta. Podría ser cualquier cosa.", "next": "uma_cuaderno_pagina_03" }
    ]
  },
  {
    "id": "uma_cuaderno_pagina_03",
    "lines": [
      { "speaker": "", "text": "Abajo dice, con otra letra: 'Ya estuviste cerca.'", "next": "uma_cuaderno_pagina_04" }
    ]
  },
  {
    "id": "uma_cuaderno_pagina_04",
    "lines": [
      { "speaker": "", "text": "No es mi letra. No es la de Tomás. No sé cuándo entró esto acá.", "next": null }
    ]
  },

  {
    "id": "uma_cuaderno_ya_visto",
    "lines": [
      { "speaker": "", "text": "La página sigue ahí. La misma forma. La misma letra.", "next": null }
    ]
  },

  {
    "id": "uma_patio_cuaderno",
    "lines": [
      { "speaker": "", "text": "Saco el cuaderno. La página que encontré adentro.", "next": "uma_patio_cuaderno_02" }
    ]
  },
  {
    "id": "uma_patio_cuaderno_02",
    "lines": [
      { "speaker": "", "text": "Afuera se ve distinto. Más real. Más urgente.", "next": null }
    ]
  },

  {
    "id": "uma_nota_padre_decision",
    "lines": [
      { "speaker": "", "text": "Una nota sobre la mesa. La letra de papá.", "next": "uma_nota_padre_texto" }
    ]
  },
  {
    "id": "uma_nota_padre_texto",
    "lines": [
      {
        "speaker": "Nota — Papá",
        "text": "Si ves algo que te llama la atención, seguí por ahí. Eso es todo lo que sé sobre encontrar cosas que valen. Vuelvo el viernes. Te quiero.",
        "choices": [
          { "text": "Guardar la nota", "next": "uma_nota_guardada", "set_flag": "guardó_nota_padre_1", "add_item": "nota_padre_1" },
          { "text": "Dejarla en la mesa", "next": "uma_nota_dejada" }
        ]
      }
    ]
  },
  {
    "id": "uma_nota_guardada",
    "lines": [
      { "speaker": "", "text": "La doblo y la guardo.", "next": null }
    ]
  },
  {
    "id": "uma_nota_dejada",
    "lines": [
      { "speaker": "", "text": "La dejo. Ya sé lo que dice.", "next": null }
    ]
  },

  {
    "id": "uma_decision_salir",
    "lines": [
      { "speaker": "", "text": "Afuera hay sol. No me gusta el sol.", "next": "uma_decision_salir_02" }
    ]
  },
  {
    "id": "uma_decision_salir_02",
    "lines": [
      { "speaker": "", "text": "Pero algo me llama la atención. Eso no pasa seguido.", "next": "uma_decision_salir_03" }
    ]
  },
  {
    "id": "uma_decision_salir_03",
    "lines": [
      { "speaker": "", "text": "Me pongo el sombrero.", "next": null }
    ]
  },

  {
    "id": "uma_puerta_bloqueada",
    "lines": [
      { "speaker": "", "text": "Todavía no. Hay algo que no termino de ver.", "next": null }
    ]
  },

  {
    "id": "uma_inventory_full",
    "lines": [
      { "speaker": "", "text": "No tengo más lugar. Llevo demasiadas cosas.", "next": null }
    ]
  },

  {
    "id": "estante_discos_inspeccion",
    "lines": [
      { "speaker": "", "text": "Hay un disco dado vuelta. La tapa mirando hacia adentro.", "next": "estante_discos_02" }
    ]
  },
  {
    "id": "estante_discos_02",
    "lines": [
      { "speaker": "", "text": "No recuerdo haberlo puesto así. Se leen las letras del medio: '...ardi'. El nombre incompleto.", "next": null, "set_flag": "inspeccionó_estante" }
    ]
  },

  {
    "id": "foto_mesita_inspeccion",
    "lines": [
      { "speaker": "", "text": "Papá. Me parezco a él en los ojos, dice mamá.", "next": "foto_mesita_02" }
    ]
  },
  {
    "id": "foto_mesita_02",
    "lines": [
      { "speaker": "", "text": "En el temperamento también. No sé si eso es un cumplido o una descripción.", "next": null }
    ]
  },

  {
    "id": "foto_heladera_inspeccion",
    "lines": [
      { "speaker": "", "text": "Los cuatro. Foto de un cumpleaños hace... cinco años. Tomás era chico.", "next": "foto_heladera_02" }
    ]
  },
  {
    "id": "foto_heladera_02",
    "lines": [
      { "speaker": "", "text": "En la foto estoy sonriendo. Eso pasa a veces.", "next": null, "set_flag": "inspeccionó_foto_heladera" }
    ]
  },

  // ─── NPCs Casa ───────────────────────────────

  {
    "id": "hermano_tomas_01",
    "lines": [
      { "speaker": "Tomás", "text": "Soñé que ibas a encontrar algo. No sé qué era, pero estabas contenta.", "next": "hermano_tomas_02" }
    ]
  },
  {
    "id": "hermano_tomas_02",
    "lines": [
      { "speaker": "Tomás", "text": "Esa cara que hacés cuando algo te gusta pero no querés que se note.", "next": "hermano_tomas_choice" }
    ]
  },
  {
    "id": "hermano_tomas_choice",
    "lines": [
      {
        "speaker": "Tomás",
        "text": "¿Querés ver el dibujo?",
        "choices": [
          { "text": "Ver el dibujo", "next": "hermano_muestra_dibujo" },
          { "text": "Dejarlo dibujar", "next": "hermano_oke", "set_flag": "habló_tomas" }
        ]
      }
    ]
  },
  {
    "id": "hermano_muestra_dibujo",
    "lines": [
      { "speaker": "Tomás", "text": "Mirá. No sé qué es. Pero lo dibujé igual.", "next": "uma_ve_dibujo" }
    ]
  },
  {
    "id": "uma_ve_dibujo",
    "lines": [
      { "speaker": "", "text": "Podría ser una radio. O una flor. O ninguna de las dos cosas.", "next": "hermano_cierre", "set_flag": "habló_tomas" }
    ]
  },
  {
    "id": "hermano_cierre",
    "lines": [
      { "speaker": "Tomás", "text": "No sé qué es. Pero sé que es eso.", "next": null }
    ]
  },
  {
    "id": "hermano_oke",
    "lines": [
      { "speaker": "Tomás", "text": "Bueno.", "next": null }
    ]
  },

  {
    "id": "madre_elena_01",
    "lines": [
      { "speaker": "Mamá", "text": "¿Querés mate?", "next": "madre_elena_choice_mate" }
    ]
  },
  {
    "id": "madre_elena_choice_mate",
    "lines": [
      {
        "speaker": "",
        "text": "",
        "choices": [
          { "text": "Sí", "next": "madre_mate_si" },
          { "text": "No, gracias", "next": "madre_mate_no" }
        ]
      }
    ]
  },
  {
    "id": "madre_mate_si",
    "lines": [
      { "speaker": "Mamá", "text": "Tomás tuvo un sueño raro. No quiso contármelo. Dice que los sueños hay que contárselos a alguien específico.", "next": "madre_frase_central" }
    ]
  },
  {
    "id": "madre_mate_no",
    "lines": [
      { "speaker": "Mamá", "text": "Tomás tuvo un sueño raro esta mañana.", "next": "madre_frase_central" }
    ]
  },
  {
    "id": "madre_frase_central",
    "lines": [
      { "speaker": "Mamá", "text": "A veces salir un ratito cambia las cosas. No siempre. Pero a veces.", "next": null, "set_flag": "habló_elena" }
    ]
  },

  // ═══════════════════════════════════════════
  // ACTO I — BARRIO
  // ═══════════════════════════════════════════

  {
    "id": "barrio_entrada_monolog",
    "lines": [
      { "speaker": "", "text": "El sol está fuerte. Menos mal que traje el sombrero.", "next": "barrio_entrada_02" }
    ]
  },
  {
    "id": "barrio_entrada_02",
    "lines": [
      { "speaker": "", "text": "El arbolado viejo da sombra si me quedo en la vereda del norte. Bien.", "next": null }
    ]
  },

  {
    "id": "barrio_gato",
    "lines": [
      { "speaker": "", "text": "Me miró. Decidió que no era interesante. Volvió a dormir.", "next": "barrio_gato_02" }
    ]
  },
  {
    "id": "barrio_gato_02",
    "lines": [
      { "speaker": "", "text": "Correcto.", "next": null }
    ]
  },

  // NPC Nélida — 3 niveles
  {
    "id": "npc_nelida_01",
    "lines": [
      { "speaker": "Nélida", "text": "¡Uma! ¡Qué bueno verte por acá! Hace rato que no bajabas.", "next": "npc_nelida_01b" }
    ]
  },
  {
    "id": "npc_nelida_01b",
    "lines": [
      { "speaker": "Nélida", "text": "No, no te digo por nada. Es que te veía desde acá, en la ventana. Pensaba: hoy no, mañana quizás.", "next": "npc_nelida_choice_01" }
    ]
  },
  {
    "id": "npc_nelida_choice_01",
    "lines": [
      {
        "speaker": "Nélida", "text": "¿A dónde vas?",
        "choices": [
          { "text": "No sé todavía.", "next": "npc_nelida_resp_a" },
          { "text": "A dar una vuelta.", "next": "npc_nelida_resp_b" },
          { "text": "(silencio)", "next": "npc_nelida_resp_c" }
        ]
      }
    ]
  },
  { "id": "npc_nelida_resp_a", "lines": [{ "speaker": "Nélida", "text": "Mejor así. Sin destino se encuentra más.", "next": null }] },
  { "id": "npc_nelida_resp_b", "lines": [{ "speaker": "Nélida", "text": "Qué lujo. Yo ya no puedo mucho. Pero vos podés.", "next": null }] },
  { "id": "npc_nelida_resp_c", "lines": [{ "speaker": "Nélida", "text": "No importa. Igual es bueno que hayas bajado.", "next": null }] },

  {
    "id": "npc_nelida_02",
    "lines": [
      { "speaker": "Nélida", "text": "¿Vos sabés que en esa esquina vivió un músico? Celso Bardi.", "next": "npc_nelida_02b" }
    ]
  },
  {
    "id": "npc_nelida_02b",
    "lines": [
      { "speaker": "Nélida", "text": "Grabó un disco acá, en la época que grababan en los departamentos porque no había plata para estudio.", "next": "npc_nelida_02c" }
    ]
  },
  {
    "id": "npc_nelida_02c",
    "lines": [
      { "speaker": "Nélida", "text": "Se escuchaba la música desde la calle. Una canción en particular. No me acuerdo la letra, pero la melodía la tengo acá todavía.", "next": null }
    ]
  },

  {
    "id": "npc_nelida_03",
    "condition": "has_item:disco_vinilo",
    "lines": [
      { "speaker": "Nélida", "text": "Ese es él. ¿De dónde lo sacaste?", "next": "npc_nelida_03b" }
    ]
  },
  {
    "id": "npc_nelida_03",
    "condition_fail": "npc_nelida_03_sin_disco",
    "lines": []
  },
  {
    "id": "npc_nelida_03_sin_disco",
    "lines": [
      { "speaker": "Nélida", "text": "Mi marido lo escuchaba. Cuando murió, yo estuve mucho tiempo sin poder escuchar música.", "next": "npc_nelida_03_cierre" }
    ]
  },
  {
    "id": "npc_nelida_03b",
    "lines": [
      { "speaker": "Nélida", "text": "Mi marido lo escuchaba. Cuando murió, yo estuve mucho tiempo sin poder escuchar música.", "next": "npc_nelida_03_cierre" }
    ]
  },
  {
    "id": "npc_nelida_03_cierre",
    "lines": [
      { "speaker": "Nélida", "text": "Después un día puse uno de sus discos sin querer y me di cuenta de que ya podía. Que no dolía igual.", "next": "npc_nelida_03_final" }
    ]
  },
  {
    "id": "npc_nelida_03_final",
    "lines": [
      { "speaker": "Nélida", "text": "Esas cosas se mueven solas, ¿sabés? Vos no las empujás. Se mueven.", "next": null }
    ]
  },

  // NPC Charly
  {
    "id": "npc_charly_01",
    "lines": [
      { "speaker": "Charly", "text": "Buenas.", "next": "npc_charly_01b" }
    ]
  },
  {
    "id": "npc_charly_01b",
    "lines": [
      {
        "speaker": "",
        "text": "La radio suena fuerte. Esa canción...",
        "choices": [
          { "text": "Quedarse a escuchar.", "next": "npc_charly_escucha" },
          { "text": "Seguir de largo.", "next": null }
        ]
      }
    ]
  },
  {
    "id": "npc_charly_escucha",
    "lines": [
      { "speaker": "Charly", "text": "¿Le doy algo?", "next": "npc_charly_escucha_b", "play_melody": "frag2_kiosco" }
    ]
  },
  {
    "id": "npc_charly_escucha_b",
    "lines": [
      { "speaker": "", "text": "Llega un cliente. Charly apaga la radio de golpe.", "next": "npc_charly_cortada" }
    ]
  },
  {
    "id": "npc_charly_cortada",
    "lines": [
      { "speaker": "", "text": "Justo ahí.", "next": null }
    ]
  },
  {
    "id": "npc_charly_02",
    "lines": [
      { "speaker": "Charly", "text": "¿Volvió?", "next": "npc_charly_02b" }
    ]
  },
  {
    "id": "npc_charly_02b",
    "lines": [
      { "speaker": "Charly", "text": "Esa canción que sonaba... algo viejo. Un tipo del barrio, me parece. Mi vieja lo conoció. Dice que era raro pero buena gente.", "next": "npc_charly_02c" }
    ]
  },
  {
    "id": "npc_charly_02c",
    "lines": [
      { "speaker": "Charly", "text": "Lo de raro no lo digo en sentido malo.", "next": null }
    ]
  },

  // NPC Don Mario
  {
    "id": "npc_mario_01",
    "lines": [
      { "speaker": "Don Mario", "text": "Buen día.", "next": "npc_mario_01b" }
    ]
  },
  {
    "id": "npc_mario_01b",
    "lines": [
      { "speaker": "Don Mario", "text": "No la vi antes por acá. ¿Es del barrio?", "next": "npc_mario_choice" }
    ]
  },
  {
    "id": "npc_mario_choice",
    "lines": [
      {
        "speaker": "",
        "text": "",
        "choices": [
          { "text": "Vivo a dos cuadras.", "next": "npc_mario_resp_a" },
          { "text": "(silencio)", "next": "npc_mario_resp_b" }
        ]
      }
    ]
  },
  { "id": "npc_mario_resp_a", "lines": [{ "speaker": "Don Mario", "text": "Los que viven cerca a veces tardan más en llegar que los que vienen de lejos. Yo tardé veinte años en conocer bien esta cuadra.", "next": "npc_mario_cierre" }] },
  { "id": "npc_mario_resp_b", "lines": [{ "speaker": "Don Mario", "text": "No importa. Se nota que es de acá.", "next": "npc_mario_cierre" }] },
  {
    "id": "npc_mario_cierre",
    "lines": [
      { "speaker": "Don Mario", "text": "Usted va a encontrar lo que está buscando. No sé qué es. Pero lo va a encontrar.", "next": null, "set_flag": "habló_mario" }
    ]
  },
  {
    "id": "npc_mario_02",
    "lines": [
      { "speaker": "Don Mario", "text": "¿Encontró algo?", "next": "npc_mario_02b" }
    ]
  },
  {
    "id": "npc_mario_02b",
    "lines": [
      { "speaker": "Don Mario", "text": "¿Sabe cuál es el error? Creer que buscar y encontrar son dos momentos distintos. Son el mismo.", "next": null }
    ]
  },

  // NPC Ramiro y Nico
  {
    "id": "npc_ramiro_01",
    "lines": [
      { "speaker": "Ramiro", "text": "Señora, ¿quiere que le tiremos las cartas?", "next": "npc_ramiro_choice" }
    ]
  },
  {
    "id": "npc_ramiro_choice",
    "lines": [
      {
        "speaker": "",
        "text": "",
        "choices": [
          { "text": "No soy señora.", "next": "npc_ramiro_resp_a" },
          { "text": "No creo en eso.", "next": "npc_ramiro_resp_b" },
          { "text": "¿Cuánto cuesta?", "next": "npc_ramiro_resp_c" }
        ]
      }
    ]
  },
  { "id": "npc_ramiro_resp_a", "lines": [{ "speaker": "Ramiro", "text": "Perdón. ¿Quiere que le tiremos las cartas, joven?", "next": "npc_ramiro_tira" }] },
  { "id": "npc_ramiro_resp_b", "lines": [{ "speaker": "Ramiro", "text": "Yo tampoco. Pero es entretenido.", "next": "npc_ramiro_tira" }] },
  { "id": "npc_ramiro_resp_c", "lines": [{ "speaker": "Ramiro", "text": "Nada. Estamos aburridos.", "next": "npc_ramiro_tira" }] },
  {
    "id": "npc_ramiro_tira",
    "lines": [
      { "speaker": "Ramiro", "text": "Esta es la carta del que encuentra cosas.", "next": "npc_ramiro_nico_comment" }
    ]
  },
  {
    "id": "npc_ramiro_nico_comment",
    "lines": [
      { "speaker": "Nico", "text": "Eso lo dice con todas.", "next": "npc_ramiro_nico_resp" }
    ]
  },
  {
    "id": "npc_ramiro_nico_resp",
    "lines": [
      { "speaker": "Ramiro", "text": "Con vos no lo digo porque ya sabés lo que buscás y no lo tenés.", "next": "uma_pregunta_nico" }
    ]
  },
  {
    "id": "uma_pregunta_nico",
    "lines": [
      {
        "speaker": "",
        "text": "",
        "choices": [
          { "text": "¿Qué está buscando él?", "next": "ramiro_responde_nico" },
          { "text": "Mejor no preguntar.", "next": null, "set_flag": "habló_ramiro_nico" }
        ]
      }
    ]
  },
  {
    "id": "ramiro_responde_nico",
    "lines": [
      { "speaker": "Ramiro", "text": "Una chica.", "next": "nico_callate" }
    ]
  },
  {
    "id": "nico_callate",
    "lines": [
      { "speaker": "Nico", "text": "Callate, Rami.", "next": null, "set_flag": "habló_ramiro_nico" }
    ]
  },
  {
    "id": "npc_ramiro_02",
    "lines": [
      { "speaker": "Ramiro", "text": "¿Encontró algo?", "next": null }
    ]
  },

  // Tienda cerrada y Foto
  {
    "id": "tienda_cartel_insp",
    "lines": [
      { "speaker": "", "text": "La persiana está baja. Cartel manuscrito: 'Vuelvo en un rato.'", "next": "tienda_cartel_02" }
    ]
  },
  {
    "id": "tienda_cartel_02",
    "lines": [
      { "speaker": "", "text": "Al costado hay una foto polaroid pegada.", "next": null }
    ]
  },

  {
    "id": "barrio_foto_polaroid_pickup",
    "lines": [
      { "speaker": "", "text": "Dos personas en un parque. No se ven las caras. Una tiene sombrero.", "next": "barrio_foto_choice" }
    ]
  },
  {
    "id": "barrio_foto_choice",
    "lines": [
      {
        "speaker": "",
        "text": "No sé quiénes son.",
        "choices": [
          { "text": "Llevarla.", "next": "barrio_foto_llevada", "set_flag": "tomó_polaroid", "add_item": "foto_polaroid" },
          { "text": "Dejarla.", "next": "barrio_foto_dejada" }
        ]
      }
    ]
  },
  { "id": "barrio_foto_llevada", "lines": [{ "speaker": "", "text": "La doblo con cuidado y la guardo.", "next": null }] },
  { "id": "barrio_foto_dejada", "lines": [{ "speaker": "", "text": "La miro una última vez. La dejo donde estaba.", "next": null }] },

  // Placa Bardi
  {
    "id": "bardi_placa_inspeccion",
    "lines": [
      { "speaker": "", "text": "Una placa de bronce, pequeña. Dice: 'En este edificio vivió y grabó el músico Celso Bardi (1944–?).'", "next": "bardi_placa_02" }
    ]
  },
  {
    "id": "bardi_placa_02",
    "lines": [
      { "speaker": "", "text": "No sé quién es. O no sabía.", "next": null }
    ]
  },

  {
    "id": "bardi_timbre",
    "lines": [
      { "speaker": "", "text": "El timbre todavía dice BARDI. Nadie de ese apellido vive hace décadas.", "next": "bardi_timbre_02" }
    ]
  },
  {
    "id": "bardi_timbre_02",
    "lines": [
      { "speaker": "", "text": "Lo toco igual. No responde nadie. Alguien tendría que actualizar eso. O quizás es mejor así.", "next": null }
    ]
  },

  // Fragmento de disco
  {
    "id": "barrio_fragmento_pickup",
    "lines": [
      { "speaker": "", "text": "En el suelo, contra el cordón. La mitad de algo.", "next": "barrio_fragmento_02" }
    ]
  },
  {
    "id": "barrio_fragmento_02",
    "lines": [
      { "speaker": "", "text": "Se ve parte de un dibujo. Las primeras letras de un nombre: CEL... No alcanza para leer. Lo agarro igual.", "next": null }
    ]
  },

  // ═══════════════════════════════════════════
  // ACTO II — PLAZA
  // ═══════════════════════════════════════════

  {
    "id": "plaza_entrada_monolog",
    "lines": [
      { "speaker": "", "text": "La plaza de siempre. Sin mucha sombra todavía. Me quedo por los bordes.", "next": null }
    ]
  },

  {
    "id": "uma_adolescentes_musica",
    "lines": [
      { "speaker": "", "text": "Están escuchando algo. Esa melodía...", "next": "uma_adolescentes_02" }
    ]
  },
  {
    "id": "uma_adolescentes_02",
    "lines": [
      { "speaker": "", "text": "Es la misma. O muy parecida. En una versión electrónica. Alguien la sampleó.", "next": null }
    ]
  },

  // Disco en la fuente
  {
    "id": "plaza_disco_fuente_pickup",
    "condition": "has_item:fragmento_disco",
    "lines": [
      { "speaker": "", "text": "En el borde de la fuente. Un disco de vinilo mojado.", "next": "plaza_disco_fuente_02_con_frag" }
    ]
  },
  {
    "id": "plaza_disco_fuente_pickup",
    "condition_fail": "plaza_disco_fuente_sin_frag",
    "lines": []
  },
  {
    "id": "plaza_disco_fuente_02_con_frag",
    "lines": [
      { "speaker": "", "text": "El mismo artista que el fragmento que encontré antes. Ahora puedo leerlo completo.", "next": "plaza_disco_fuente_03" }
    ]
  },
  {
    "id": "plaza_disco_fuente_sin_frag",
    "lines": [
      { "speaker": "", "text": "Un vinilo en el borde de una fuente. Quién lo deja acá.", "next": "plaza_disco_fuente_03" }
    ]
  },
  {
    "id": "plaza_disco_fuente_03",
    "lines": [
      { "speaker": "", "text": "Celso Bardi. El disco se llama 'Adentro'. Paradoja no buscada. Lo guardo.", "next": null }
    ]
  },

  // NPC Valentina
  {
    "id": "npc_valentina_01",
    "lines": [
      { "speaker": "Valentina", "text": "Hola. ¿Te sentás?", "next": "npc_valentina_choice_01" }
    ]
  },
  {
    "id": "npc_valentina_choice_01",
    "lines": [
      {
        "speaker": "",
        "text": "",
        "choices": [
          { "text": "Me quedo un momento.", "next": "npc_valentina_resp_si" },
          { "text": "No, gracias.", "next": null }
        ]
      }
    ]
  },
  {
    "id": "npc_valentina_resp_si",
    "lines": [
      { "speaker": "Valentina", "text": "Cuando llueve todo suena más lindo, ¿no te parece? No sé si es el agua o que los otros ruidos bajan.", "next": null }
    ]
  },
  {
    "id": "npc_valentina_02",
    "lines": [
      { "speaker": "Valentina", "text": "¿Estás esperando algo?", "next": "uma_resp_valentina_02" }
    ]
  },
  {
    "id": "uma_resp_valentina_02",
    "lines": [
      {
        "speaker": "",
        "text": "No sé.",
        "choices": [
          { "text": "¿Qué estás leyendo?", "next": "valentina_libro" },
          { "text": "Sí, algo así.", "next": "valentina_espera" }
        ]
      }
    ]
  },
  { "id": "valentina_espera", "lines": [{ "speaker": "Valentina", "text": "A veces esperar algo que no sabés qué es es más honesto que esperar algo que sí sabés.", "next": null }] },
  {
    "id": "valentina_libro",
    "lines": [
      { "speaker": "Valentina", "text": "Una novela donde nada pasa. Pero pasa todo.", "next": "valentina_libro_02" }
    ]
  },
  {
    "id": "valentina_libro_02",
    "lines": [
      { "speaker": "Valentina", "text": "¿Leés?", "next": "uma_resp_leer" }
    ]
  },
  {
    "id": "uma_resp_leer",
    "lines": [
      { "speaker": "", "text": "Leía. Dejé.", "next": "valentina_libros_esperan" }
    ]
  },
  {
    "id": "valentina_libros_esperan",
    "lines": [
      { "speaker": "Valentina", "text": "Los libros esperan. Son buenos en eso.", "next": null }
    ]
  },
  {
    "id": "npc_valentina_03",
    "condition": "has_item:disco_vinilo",
    "lines": [
      { "speaker": "Valentina", "text": "¿Eso es de Bardi?", "next": "valentina_bardi_02" }
    ]
  },
  {
    "id": "npc_valentina_03",
    "condition_fail": "npc_valentina_03_sin_disco",
    "lines": []
  },
  {
    "id": "npc_valentina_03_sin_disco",
    "lines": [
      { "speaker": "Valentina", "text": "¿Seguís por acá?", "next": null }
    ]
  },
  {
    "id": "valentina_bardi_02",
    "lines": [
      { "speaker": "Valentina", "text": "Mi viejo lo escuchaba. ¿Dónde lo conseguiste?", "next": "valentina_bardi_03" }
    ]
  },
  {
    "id": "valentina_bardi_03",
    "lines": [
      { "speaker": "Valentina", "text": "En la plaza, ¿en serio? Hay objetos que vuelven a las personas que los necesitan. No lo digo en sentido místico.", "next": "valentina_bardi_04", "set_flag": "valentina_nivel_3" }
    ]
  },
  {
    "id": "valentina_bardi_04",
    "lines": [
      { "speaker": "Valentina", "text": "Lo digo en el sentido de que alguien lo dejó ahí para alguien, y ese alguien eras vos.", "next": null }
    ]
  },

  // NPC Ernesto
  {
    "id": "npc_ernesto_01",
    "lines": [
      { "speaker": "Ernesto", "text": "Se fueron con la lluvia.", "next": "npc_ernesto_01b" }
    ]
  },
  {
    "id": "npc_ernesto_01b",
    "lines": [
      { "speaker": "Ernesto", "text": "Pero van a volver. Siempre vuelven.", "next": "npc_ernesto_choice" }
    ]
  },
  {
    "id": "npc_ernesto_choice",
    "lines": [
      {
        "speaker": "",
        "text": "",
        "choices": [
          { "text": "¿Cómo se llaman?", "next": "npc_ernesto_gardel" },
          { "text": "(dejar que hable)", "next": "npc_ernesto_esperar" }
        ]
      }
    ]
  },
  {
    "id": "npc_ernesto_esperar",
    "lines": [
      { "speaker": "Ernesto", "text": "Uno aprende a esperar. Lo difícil es aprender que esperar no es lo mismo que quedarse quieto.", "next": null, "set_flag": "habló_ernesto" }
    ]
  },
  {
    "id": "npc_ernesto_gardel",
    "lines": [
      { "speaker": "Ernesto", "text": "Al de la pata torcida lo llamo Gardel, aunque mi hija dice que es una hembra.", "next": "npc_ernesto_gardel_02" }
    ]
  },
  {
    "id": "npc_ernesto_gardel_02",
    "lines": [
      { "speaker": "Ernesto", "text": "Le digo que no importa, que Gardel era un nombre para una actitud, no para un sexo.", "next": null, "set_flag": "habló_ernesto" }
    ]
  },
  {
    "id": "npc_ernesto_02",
    "lines": [
      { "speaker": "Ernesto", "text": "¿Volvieron? Yo todavía estoy esperando.", "next": null }
    ]
  },

  // Banco Uma
  {
    "id": "uma_choice_banco_plaza",
    "lines": [
      {
        "speaker": "",
        "text": "El banco está libre. Hay lugar.",
        "choices": [
          { "text": "Sentarse un rato.", "next": "uma_se_sienta_plaza", "set_flag": "se_sentó_banco_plaza" },
          { "text": "Seguir caminando.", "next": null }
        ]
      }
    ]
  },
  {
    "id": "uma_se_sienta_plaza",
    "lines": [
      { "speaker": "", "text": "La lluvia suena bien desde acá. Me quedo un rato.", "next": null }
    ]
  },
  {
    "id": "uma_dibuja_plaza",
    "lines": [
      { "speaker": "", "text": "Dibujo la plaza. Rápido, sin pensar demasiado. La fuente, los árboles, la lluvia.", "next": "uma_dibuja_02" }
    ]
  },
  {
    "id": "uma_dibuja_02",
    "lines": [
      { "speaker": "", "text": "Nunca dibujo afuera. Adentro sí. Afuera hay demasiado. Hoy hay exactamente suficiente.", "next": null }
    ]
  },

  // ═══════════════════════════════════════════
  // ACTO III — ZONA INTERIOR
  // ═══════════════════════════════════════════

  {
    "id": "zona_interior_entrada",
    "lines": [
      { "speaker": "", "text": "Menos gente. Más silencio. La escala cambió.", "next": null }
    ]
  },

  {
    "id": "uma_planta_sin_maceta",
    "lines": [
      { "speaker": "", "text": "Una planta en el suelo. Sin maceta.", "next": "uma_planta_02" }
    ]
  },
  {
    "id": "uma_planta_02",
    "lines": [
      { "speaker": "", "text": "Es igual a la de la señora del balcón. Pero acá abajo, sin sostén.", "next": null }
    ]
  },

  {
    "id": "uma_radio_nadie",
    "lines": [
      { "speaker": "", "text": "Una radio. Sin nadie. Igual a la del kiosco.", "next": "uma_radio_nadie_02" }
    ]
  },
  {
    "id": "uma_radio_nadie_02",
    "lines": [
      { "speaker": "", "text": "No está enchufada. Pero si me quedo quieta, parece que pudiera sonar.", "next": null }
    ]
  },

  {
    "id": "uma_banco_repetido",
    "lines": [
      { "speaker": "", "text": "El mismo banco que el de la plaza.", "next": "uma_banco_repetido_02" }
    ]
  },
  {
    "id": "uma_banco_repetido_02",
    "lines": [
      { "speaker": "", "text": "No puede ser el mismo. Pero es igual. Tiene la madera igual de gastada. La pintura en el mismo lugar.", "next": null }
    ]
  },

  {
    "id": "puerta_voz_madre",
    "lines": [
      { "speaker": "", "text": "Una puerta entreabierta. Adentro, la voz de mamá.", "next": "puerta_voz_elena_frase" }
    ]
  },
  {
    "id": "puerta_voz_elena_frase",
    "lines": [
      { "speaker": "Voz de Mamá", "text": "... no porque sea fácil. Es que hay cosas que no se entienden hasta que las sentís. Por eso no sirve que te las expliquen. Tenés que ir.", "next": "puerta_voz_pausa" }
    ]
  },
  {
    "id": "puerta_voz_pausa",
    "lines": [
      { "speaker": "Voz de Mamá", "text": "No, no da miedo. O sí da miedo, pero de un tipo distinto. El miedo que da antes es diferente al que da después. El de antes es más grande.", "next": "puerta_se_cierra" }
    ]
  },
  {
    "id": "puerta_se_cierra",
    "lines": [
      { "speaker": "", "text": "La puerta se cierra sola. No hay nadie.", "next": null }
    ]
  },

  // Activadores
  {
    "id": "disc_slot_empty",
    "lines": [
      { "speaker": "", "text": "Una ranura del tamaño de un disco. No tengo nada que encaje.", "next": null }
    ]
  },

  {
    "id": "disco_puesto_intermedia",
    "lines": [
      { "speaker": "", "text": "La ranura acepta el disco. Empieza a sonar.", "next": "disco_puesto_02" }
    ]
  },
  {
    "id": "disco_puesto_02",
    "lines": [
      { "speaker": "", "text": "Un fragmento. Más completo que los otros. Ahora recuerdo: la escuché en el kiosco. En el teléfono de los adolescentes. Es la misma. Hay más.", "next": null }
    ]
  },

  {
    "id": "sombrero_colgado",
    "lines": [
      { "speaker": "", "text": "Lo cuelgo. Primera vez que estoy sin él en todo el día.", "next": "sombrero_colgado_02" }
    ]
  },
  {
    "id": "sombrero_colgado_02",
    "lines": [
      { "speaker": "", "text": "La luz no me molesta tanto como esperaba. Todo está un poco más cerca.", "next": null }
    ]
  },

  {
    "id": "sombrero_recuperado",
    "lines": [
      { "speaker": "", "text": "Lo vuelvo a poner. Está bien. Era mío.", "next": null }
    ]
  },

  {
    "id": "marco_vacio_sin_foto",
    "lines": [
      { "speaker": "", "text": "Un marco vacío. No tengo nada para ponerle.", "next": null }
    ]
  },

  {
    "id": "foto_en_marco",
    "lines": [
      { "speaker": "", "text": "La pongo en el marco.", "next": "foto_en_marco_02" }
    ]
  },
  {
    "id": "foto_en_marco_02",
    "lines": [
      { "speaker": "", "text": "No sé quiénes son. Pero se ven contentos. La clase de contentos que no le están mostrando a nadie.", "next": null }
    ]
  },

  {
    "id": "foto_ya_puesta",
    "lines": [
      { "speaker": "", "text": "La foto está ahí. Se ve bien en ese marco.", "next": null }
    ]
  },

  // ═══════════════════════════════════════════
  // ACTO IV — ANTESALA
  // ═══════════════════════════════════════════

  {
    "id": "antesala_entrada",
    "lines": [
      { "speaker": "", "text": "Un patio. Cielo abierto. La lluvia sigue cayendo.", "next": null }
    ]
  },

  {
    "id": "antesala_farola",
    "lines": [
      { "speaker": "", "text": "Una farola apagada. Miro hacia arriba. Las nubes son muy bajas.", "next": "antesala_farola_02" }
    ]
  },
  {
    "id": "antesala_farola_02",
    "lines": [
      { "speaker": "", "text": "Si se encendiera ahora, me gustaría eso.", "next": null }
    ]
  },

  {
    "id": "antesala_nota_padre_2",
    "lines": [
      { "speaker": "", "text": "Una nota en el banco. La letra de papá.", "next": "antesala_nota_texto" }
    ]
  },
  {
    "id": "antesala_nota_texto",
    "lines": [
      { "speaker": "Nota — H", "text": "A veces llegar no es lo difícil. Es animarse a seguir cuando ya llegaste. Eso lo aprendí tarde. Vos lo estás aprendiendo bien.", "next": "antesala_nota_comparacion_check" }
    ]
  },
  {
    "id": "antesala_nota_comparacion_check",
    "condition": "has_item:nota_padre_1",
    "lines": [
      { "speaker": "", "text": "Saco la primera nota. Las comparo. Misma letra. Mismo papel.", "next": "antesala_comparacion_02", "set_flag": "comparó_notas" }
    ]
  },
  {
    "id": "antesala_nota_comparacion_check",
    "condition_fail": "antesala_nota_guardar_choice",
    "lines": []
  },
  {
    "id": "antesala_comparacion_02",
    "lines": [
      { "speaker": "", "text": "No sé si las escribió el mismo día o en momentos distintos. El resultado es el mismo.", "next": "antesala_nota_guardar_choice" }
    ]
  },
  {
    "id": "antesala_nota_guardar_choice",
    "lines": [
      {
        "speaker": "",
        "text": "",
        "choices": [
          { "text": "Guardar también esta nota.", "next": "antesala_nota_guardada", "add_item": "nota_padre_2", "set_flag": "guardó_nota_padre_2" },
          { "text": "Dejarla en el banco.", "next": null }
        ]
      }
    ]
  },
  { "id": "antesala_nota_guardada", "lines": [{ "speaker": "", "text": "La doblo junto a la otra.", "next": null }] },

  {
    "id": "antesala_duda_sola",
    "lines": [
      { "speaker": "", "text": "Ya llegué hasta acá.", "next": null }
    ]
  },

  // ═══════════════════════════════════════════
  // ACTO V — ESPACIO FINAL
  // ═══════════════════════════════════════════

  {
    "id": "climax_entrada",
    "lines": [
      { "speaker": "", "text": "Un espacio grande. Con techo pero abierto de un lado.", "next": "climax_entrada_02" }
    ]
  },
  {
    "id": "climax_entrada_02",
    "lines": [
      { "speaker": "", "text": "La lluvia se escucha pero no llega hasta acá.", "next": null }
    ]
  },

  {
    "id": "climax_radio_inspeccion_sin_disco",
    "lines": [
      { "speaker": "", "text": "Una radio antigua. Tiene un lugar para algo.", "next": "climax_radio_02" }
    ]
  },
  {
    "id": "climax_radio_02",
    "lines": [
      { "speaker": "", "text": "No tengo nada que encaje. O quizás sí.", "next": null }
    ]
  },

  {
    "id": "climax_radio_con_disco",
    "lines": [
      { "speaker": "", "text": "El disco entra. La radio lo acepta.", "next": "climax_radio_suena" }
    ]
  },
  {
    "id": "climax_radio_suena",
    "lines": [
      { "speaker": "", "text": "La canción empieza. Es la misma. Entera. Ahora la escucho de principio a fin.", "next": null }
    ]
  },

  // Diálogos de Pablo — variantes
  {
    "id": "pablo_final_disco",
    "lines": [
      { "speaker": "Pablo", "text": "Sabía que ibas a reconocer esa canción.", "next": "pablo_final_comun" }
    ]
  },
  {
    "id": "pablo_final_banco",
    "lines": [
      { "speaker": "Pablo", "text": "Vi cuando te quedaste en la plaza aunque estaba lloviendo.", "next": "pablo_final_comun" }
    ]
  },
  {
    "id": "pablo_final_nota",
    "lines": [
      { "speaker": "Pablo", "text": "Tu viejo tiene razón. No es lo mismo llegar que animarse a seguir.", "next": "pablo_final_comun" }
    ]
  },
  {
    "id": "pablo_final_notas_comparadas",
    "lines": [
      { "speaker": "Pablo", "text": "¿Comparaste las dos notas?", "next": "uma_resp_pablo_notas" }
    ]
  },
  {
    "id": "uma_resp_pablo_notas",
    "lines": [
      { "speaker": "", "text": "¿Cómo sabés que hay dos?", "next": "pablo_responde_notas" }
    ]
  },
  {
    "id": "pablo_responde_notas",
    "lines": [
      { "speaker": "Pablo", "text": "No sé. Lo adiviné.", "next": "pablo_final_comun" }
    ]
  },
  {
    "id": "pablo_final_sin_sombrero",
    "lines": [
      { "speaker": "Pablo", "text": "¿Dónde dejaste el sombrero?", "next": "uma_resp_sin_sombrero" }
    ]
  },
  {
    "id": "uma_resp_sin_sombrero",
    "lines": [
      { "speaker": "", "text": "En algún lado.", "next": "pablo_resp_igual_aca" }
    ]
  },
  {
    "id": "pablo_resp_igual_aca",
    "lines": [
      { "speaker": "Pablo", "text": "Y igual estás acá.", "next": "pablo_final_comun" }
    ]
  },
  {
    "id": "pablo_final_base",
    "lines": [
      { "speaker": "Pablo", "text": "Mucho tiempo esperando que salieras. Me alegra que lo hayas hecho.", "next": "pablo_final_comun" }
    ]
  },
  {
    "id": "pablo_final_comun",
    "lines": [
      { "speaker": "Pablo", "text": "No tenía apuro. Solo esperaba el momento.", "next": "uma_respuesta_final" }
    ]
  },

  // Respuesta de Uma
  {
    "id": "uma_respuesta_final",
    "lines": [
      {
        "speaker": "",
        "text": "",
        "choices": [
          { "text": "Tarde mucho, lo sé.", "next": "uma_resp_ironica", "respuesta_id": "ironica" },
          { "text": "También me alegra.", "next": "uma_resp_directa", "respuesta_id": "directa" },
          { "text": "(silencio)", "next": "uma_resp_silencio", "respuesta_id": "silencio" }
        ]
      }
    ]
  },
  {
    "id": "uma_resp_ironica",
    "lines": [
      { "speaker": "Uma", "text": "Tarde mucho, lo sé.", "next": "pablo_cierre_resp" }
    ]
  },
  {
    "id": "pablo_cierre_resp",
    "lines": [
      { "speaker": "Pablo", "text": "No importa el cuánto. Importa que estás acá.", "next": "pablo_cierre_final" }
    ]
  },
  {
    "id": "uma_resp_directa",
    "lines": [
      { "speaker": "Uma", "text": "También me alegra.", "next": "pablo_cierre_final" }
    ]
  },
  {
    "id": "uma_resp_silencio",
    "lines": [
      { "speaker": "", "text": "Uma no dice nada. Se queda.", "next": "uma_monolog_silencio" }
    ]
  },
  {
    "id": "uma_monolog_silencio",
    "lines": [
      { "speaker": "", "text": "El silencio que no incomoda es el más honesto que existe.", "next": "pablo_cierre_final" }
    ]
  },
  {
    "id": "pablo_cierre_final",
    "lines": [
      { "speaker": "Pablo", "text": "La lluvia sigue.", "next": null }
    ]
  }

]
```

---

## 16. DATA: ITEMS EN JSON

### `res://data/items.json`

```json
[
  {
    "id": "fragmento_disco",
    "name": "Fragmento de tapa",
    "description_base": "La mitad de una tapa de disco. Se ve parte de un dibujo y las primeras letras de un nombre: CEL...",
    "description_updated": "Era la mitad de la tapa del disco de Celso Bardi. Alguien los separó o los perdió. Los dos terminaron en el mismo barrio.",
    "icon_path": "res://assets/sprites/objects/fragmento_disco.png"
  },
  {
    "id": "disco_vinilo",
    "name": "Disco de vinilo — Celso Bardi",
    "description_base": "Encontrado en la fuente seca de la plaza. Celso Bardi. El disco se llama 'Adentro'. Mojado pero en buen estado.",
    "description_updated": "La canción que sonó en fragmentos durante todo el recorrido estaba acá. La llevo conmigo.",
    "icon_path": "res://assets/sprites/objects/disco_vinilo.png"
  },
  {
    "id": "foto_polaroid",
    "name": "Foto polaroid",
    "description_base": "Dos personas en un parque. No se ven las caras. Una tiene sombrero. No sé si es mi sombrero o es otro sombrero.",
    "description_updated": "La puse en el marco. Se ve bien ahí.",
    "icon_path": "res://assets/sprites/objects/foto_polaroid.png"
  },
  {
    "id": "nota_padre_1",
    "name": "Nota de papá",
    "description_base": "Si ves algo que te llama la atención, seguí por ahí.",
    "description_updated": "Hay otra nota igual. Misma letra, mismo papel. No sé si las escribió juntas o separadas.",
    "icon_path": "res://assets/sprites/objects/nota_papel.png"
  },
  {
    "id": "nota_padre_2",
    "name": "Segunda nota — H",
    "description_base": "A veces llegar no es lo difícil. Es animarse a seguir cuando ya llegaste.",
    "description_updated": "",
    "icon_path": "res://assets/sprites/objects/nota_papel.png"
  },
  {
    "id": "boceto_plaza",
    "name": "Boceto — La plaza",
    "description_base": "Un dibujo rápido de la plaza bajo la lluvia. La fuente seca. Los árboles. Lo hice yo.",
    "description_updated": "",
    "icon_path": "res://assets/sprites/objects/boceto.png"
  }
]
```

---

## 17. TRANSICIONES Y EFECTOS VISUALES

### 17.1 Fade entre Escenas

Cada zona hereda de `BaseZone` el fade-in automático. El fade-out se llama desde `_fade_out_and_load()`.

```gdscript
# En BaseZone:
func _fade_in() -> void:
	transition_overlay.color.a = 1.0
	var tween := create_tween()
	tween.tween_property(transition_overlay, "color:a", 0.0, 0.8)

func _fade_out_and_load(scene_path: String) -> void:
	if GameManager.uma_node:
		GameManager.uma_node.freeze()
	var tween := create_tween()
	tween.tween_property(transition_overlay, "color:a", 1.0, 0.8)
	await tween.finished
	GameManager.save_game()
	GameManager.change_scene(scene_path)
```

### 17.2 Primer Plano de Uma (Zona Interior)

Efecto especial al final de la Zona Interior: la cámara "rompe" la perspectiva lateral para mostrar la cara de Uma de frente.

```gdscript
# En zona_interior.gd, antes del trigger_pasaje_final:
func _primer_plano_uma() -> void:
	if GameManager.uma_node == null:
		return
	GameManager.uma_node.freeze()

	# Crear un panel negro que revela solo el área de la cara de Uma
	var mask := ColorRect.new()
	mask.color = Color(0, 0, 0, 0)
	mask.anchors_preset = Control.PRESET_FULL_RECT
	add_child(mask)

	var tween := create_tween()
	tween.tween_property(mask, "color:a", 0.85, 1.5)
	await tween.finished

	# Uma reproduce animación "think" mirando al frente
	GameManager.uma_node.anim.play("think")
	await get_tree().create_timer(3.0).timeout

	tween = create_tween()
	tween.tween_property(mask, "color:a", 0.0, 1.5)
	await tween.finished
	mask.queue_free()
	GameManager.uma_node.unfreeze()
```

### 17.3 Aparición de Textos Flotantes (Acto V)

*(Ver sección 11.2 — `floating_text.gd` y `_spawn_floating_texts()` en `espacio_final.gd`)*

### 17.4 `EndSummary.tscn` — Pantalla Final

```
EndSummary (Node2D)
├── Background (ColorRect) color negro
├── VBoxContainer (centrado)
│   ├── TitleLabel — "Una historia pequeña"
│   ├── Separator
│   ├── StatsLabel — "Objetos encontrados: X / 6"
│   ├── StatsLabel — "Personas que conociste: X"
│   ├── StatsLabel — "Tiempo en la calle: X min"
│   ├── Separator
│   ├── NotebookPreview (HBoxContainer)
│   │   └── [miniaturas de los bocetos del cuaderno]
│   ├── Separator
│   └── FinalQuote — "A veces salir un ratito cambia las cosas."
└── BtnMenu (Button) — "Volver al menú"
```

```gdscript
# res://scripts/ui/end_summary.gd
extends Node2D

@onready var stats_obj:     Label = $VBoxContainer/StatsObjetos
@onready var stats_npc:     Label = $VBoxContainer/StatsNPCs
@onready var stats_tiempo:  Label = $VBoxContainer/StatsTiempo
@onready var notebook_prev: HBoxContainer = $VBoxContainer/NotebookPreview
@onready var btn_menu:      Button = $VBoxContainer/BtnMenu

func _ready() -> void:
	btn_menu.pressed.connect(func(): GameManager.change_scene("res://scenes/ui/MainMenu.tscn"))
	MusicManager.play_fragment("frag1_cocina")  # melodía suave de fondo
	_populate_stats()
	_populate_notebook()
	_fade_in()

func _populate_stats() -> void:
	var obj_count := GameManager.get_inventory_count()
	var npc_count := _count_npcs()
	var tiempo := _get_tiempo_exterior()
	stats_obj.text    = "Objetos que llevaste: %d de 6" % obj_count
	stats_npc.text    = "Personas con quienes hablaste: %d" % npc_count
	stats_tiempo.text = "Tiempo afuera: %d minutos" % int(tiempo / 60.0)

func _count_npcs() -> int:
	var count := 0
	var npc_flags := ["habló_elena", "habló_tomas", "habló_mario",
	                  "habló_ernesto", "habló_ramiro_nico", "charly_escuchó_canción"]
	for f in npc_flags:
		if FlagsManager.get_flag(f):
			count += 1
	var nelida_nivel := FlagsManager.get_flag("nelida_nivel") as int
	if nelida_nivel > 0:
		count += 1
	var valentina_nivel := FlagsManager.get_flag("valentina_nivel") as int
	if valentina_nivel > 0:
		count += 1
	return count

func _get_tiempo_exterior() -> float:
	var t: float = 0.0
	for key in ["barrio", "plaza", "zona_interior", "antesala", "espacio_final"]:
		t += GameManager.zone_times.get(key, 0.0)
	return t

func _populate_notebook() -> void:
	for sketch in GameManager.get_sketches():
		var tex_rect := TextureRect.new()
		tex_rect.custom_minimum_size = Vector2(48, 48)
		tex_rect.stretch_mode = TextureRect.STRETCH_KEEP_ASPECT_CENTERED
		if sketch.has("texture"):
			tex_rect.texture = load(sketch["texture"])
		notebook_prev.add_child(tex_rect)

func _fade_in() -> void:
	modulate.a = 0.0
	var tween := create_tween()
	tween.tween_property(self, "modulate:a", 1.0, 2.0)
```

---

## 18. NOTAS DE IMPLEMENTACIÓN Y CHECKLIST

### 18.1 Orden de Desarrollo Recomendado

```
FASE 1 — Fundamentos (sin arte)
  [1] Implementar GameManager, FlagsManager, MusicManager, DialogData, ItemData
  [2] Verificar save/load funciona antes de tocar escenas
  [3] Implementar Uma con placeholders y movimiento

FASE 2 — Sistema de Diálogo
  [4] Implementar DialogBox completo
  [5] Cargar dialogos.json y probar con 10 diálogos de prueba
  [6] Probar condiciones, choices, set_flag dentro del diálogo

FASE 3 — Casa y Barrio
  [7] Construir Casa.tscn con triggers
  [8] Probar los tres detonantes → salida
  [9] Construir Barrio.tscn con todos los NPCs y objetos
  [10] Verificar fragmento, polaroid, placa Bardi

FASE 4 — Sistema de Clima
  [11] Implementar WeatherSystem
  [12] Integrar en Plaza con el arco climático
  [13] Conectar HUD con íconos de clima

FASE 5 — Plaza y Mecánicas de Banco
  [14] Construir Plaza.tscn
  [15] Implementar Bench con lógica de auto-dibujar
  [16] Verificar disco, NPCs Valentina y Ernesto

FASE 6 — Zona Interior, Antesala y Final
  [17] Construir ZonaInterior.tscn con activadores
  [18] Implementar DiscSlot, HatHook, PhotoFrame
  [19] Construir Antesala.tscn con nota y lógica de espera
  [20] Construir EspacioFinal.tscn con secuencia completa

FASE 7 — Sistemas de Soporte
  [21] Implementar Inventory Panel completo
  [22] Implementar Notebook con páginas y bocetos
  [23] Implementar EndSummary con estadísticas
  [24] Implementar MusicManager con crossfade y fragmentos

FASE 8 — Arte y Audio
  [25] Sprites de Uma (todas las animaciones)
  [26] Sprites de NPCs
  [27] Tilesets exterior e interior
  [28] Tracks de música y SFX
  [29] Fuentes

FASE 9 — Polish
  [30] Efectos de partículas de lluvia
  [31] Transiciones entre escenas
  [32] Primer plano de Uma en Zona Interior
  [33] Textos flotantes en Acto V
  [34] Balance de volumen y mezcla
  [35] Prueba de flujo completo de punta a punta
```

### 18.2 Checklist por Escena

Cada escena se considera lista cuando cumple todos estos puntos:

**Técnico:**
- [ ] Hereda de `BaseZone` correctamente
- [ ] Todos los TileMapLayers en las capas correctas (z_index)
- [ ] `y_sort_enabled = true` en Characters y Objects
- [ ] Fade in y fade out funcionan
- [ ] Clima inicial correcto (se aplica con `instant=true`)
- [ ] Música correcta al inicio
- [ ] Guardado automático al salir
- [ ] Uma spawna en posición correcta

**Narrativo:**
- [ ] Todos los NPCs tienen `dialog_ids` asignados
- [ ] Todos los PickupItems tienen `item_id` y `pickup_dialog_id`
- [ ] Todos los InspectableObjects tienen `dialog_id`
- [ ] Todos los triggers están conectados a sus funciones
- [ ] Flags se setean correctamente en cada evento
- [ ] Condiciones de diálogo probadas (con y sin items/flags)

**NPCs:**
- [ ] Todos los NPCs tienen animación `idle` funcionando
- [ ] Nélida: 3 niveles de diálogo probados
- [ ] Valentina: 3 niveles + condición disco probados
- [ ] Charly: melodía se corta correctamente
- [ ] Pablo: se activa solo al final, no antes

### 18.3 Configuración de Buses de Audio

Crear los siguientes buses en **Proyecto → Audio**:

| Bus | Padre | Uso |
|---|---|---|
| `Master` | — | Volumen global |
| `Music` | Master | Música narrativa y fragmentos |
| `Ambience` | Master | Lluvia, viento, ambiente |
| `SFX` | Master | Pasos, items, diálogo blip |
| `Voice` | Master | Voz de Elena (si se implementa) |

### 18.4 Variables de Configuración Expuestas (para Opciones)

```gdscript
# Valores accesibles desde OptionsMenu
const CONFIG_PATH: String = "user://settings.cfg"

var config: ConfigFile = ConfigFile.new()

func save_settings(music_vol: float, sfx_vol: float, text_speed: float) -> void:
	config.set_value("audio", "music_volume", music_vol)
	config.set_value("audio", "sfx_volume", sfx_vol)
	config.set_value("gameplay", "text_speed", text_speed)
	config.save(CONFIG_PATH)

func load_settings() -> void:
	if config.load(CONFIG_PATH) != OK:
		return
	var mv: float = config.get_value("audio", "music_volume", 1.0)
	var sv: float = config.get_value("audio", "sfx_volume", 1.0)
	var ts: float = config.get_value("gameplay", "text_speed", 0.03)
	MusicManager.set_music_volume(mv)
	# text_speed se pasa al DialogBox
```

### 18.5 Consideraciones de Rendimiento

- Los mapas son pequeños (máximo 80×24 tiles). No se necesita LOD ni streaming de chunks.
- Los GPUParticles2D de lluvia deben tener `local_coords = false` para que sigan la cámara.
- Los AudioStreamPlayer para música deben estar en el árbol raíz (como hijos del Autoload MusicManager), no dentro de las escenas de mundo, para sobrevivir los cambios de escena.
- El `y_sort_enabled` en los nodos Characters y Objects es suficiente para el orden visual. No se necesita shader de profundidad.
- Las transiciones de fade son tweens de propiedad alpha sobre un `ColorRect` de canvas layer. Es el método más liviano disponible.

### 18.6 Depuración — Flags en Tiempo Real

Para depurar el estado del juego durante el desarrollo, agregar este script a una escena de debug:

```gdscript
# Debug: mostrar todos los flags en pantalla
# Solo en builds de desarrollo

extends CanvasLayer

@onready var label: Label = $Label

func _process(_delta: float) -> void:
	if not OS.is_debug_build():
		queue_free()
		return
	var text := "=== FLAGS ===\n"
	for key in FlagsManager.flags.keys():
		var val = FlagsManager.flags[key]
		if val != false and val != 0 and val != "":
			text += "%s: %s\n" % [key, str(val)]
	text += "\n=== INVENTORY ===\n"
	text += str(GameManager.inventory)
	text += "\n=== MELODY ===\n"
	text += str(GameManager.melody_fragments_heard)
	label.text = text
```

---

*HUMITA — Documento de Desarrollo Godot v2.0*

*Última revisión: Documento completo integrado con Historia Extendida v2.0*

*"Una mecánica simple. Una carga narrativa completa. Uma primero."*