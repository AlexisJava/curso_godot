# 🕹️ Club de Programación Godot – Proyecto Base “Recolector de Piñas"

> **Neuquén Capital, 2025**
> Este repositorio contiene el **juego base** para nuestras clases de Godot 4.4.1, con lógica de spawn/respawn de ítems, POO en GDScript y un flujo Git/GitHub colaborativo.

---

## 📚 Índice

1. [Descripción del Proyecto](#descripción-del-proyecto)
2. [Requisitos y Entorno](#requisitos-y-entorno)
3. [Estructura de Carpetas](#estructura-de-carpetas)
4. [Configuración Inicial](#configuración-inicial)
5. [Cómo Ejecutar](#cómo-ejecutar)
6. [Arquitectura de Código](#arquitectura-de-código)
7. [Flujo de Trabajo Git/GitHub](#flujo-de-trabajo-gitgithub)
8. [Roadmap de Clases y Temas](#roadmap-de-clases-y-temas)
9. [Guía de Contribuciones](#guía-de-contribuciones)
10. [Preguntas Frecuentes](#preguntas-frecuentes)
11. [Contactos y Créditos](#contactos-y-créditos)

---

## 1. Descripción del Proyecto

Este mini-juego enseña:

* **POO en Godot**: clases, herencia, señales
* **Spawn dinámico** de ítems en puntos fijos y respawn tras recoger
* **Control de entrada** y **movimiento** del jugador
* **Interfaz HUD** con `CanvasLayer` y `Label`
* **Buenas prácticas Git** en equipo

El jugador debe desplazarse y recoger piñas que reaparecen en distintas plataformas, acumulando puntos hasta alcanzar un objetivo.

---

## 2. Requisitos y Entorno

* **Godot Engine 4.4.1**
* **GIT** (versión ≥ 2.34) con clave SSH configurada
* **Sistema Operativo**: Linux (Arch recomendado), Windows o macOS
* **Editor de Texto**: VSCode, Sublime, Vim, etc.
* **Navegador Web** para GitHub

---

## 3. Estructura de Carpetas

```text
curso_godot/
├─ .gitignore
├─ project.godot
├─ README.md
├─ LICENSE
├─ assets/                  # Sprites, tilesets, fuentes…
│   ├─ player.png
│   └─ pineapple.png
├─ scenes/
│   ├─ Main.tscn            # Escena principal
│   ├─ Player.tscn          # Jugador
│   ├─ Moneda.tscn          # Ítem recolectable
│   └─ SpawnPoints.tscn     # Contenedor de P0…P5
├─ scripts/
│   ├─ Orquestador.gd       # Lógica de spawn/respawn/puntaje
│   ├─ Player.gd            # Movimiento y animaciones
│   └─ Moneda.gd            # Señal y detección de recogida
├─ items.tscn               # Alias para Moneda.tscn (opcional)
└─ posicion_en_pantalla.tscn# P0…P5 individual (si se usa standalone)
```

---

## 4. Configuración Inicial

1. **Clona el repositorio**

   ```bash
   git clone git@github.com:AlexisJava/curso_godot.git
   cd curso_godot
   ```

2. **Instala dependencias**

   * Asegúrate de tener Godot 4.4.1 en tu PATH
   * Verifica tu clave SSH con `ssh -T git@github.com`

3. **Ignora archivos temporales**

   * Revisa `.gitignore`, que excluye `.import/`, `.godot/`, `export.cfg`, backups y builds.

4. **Configura Input Map**
   En Godot: **Proyecto → Configuración del proyecto → Mapa de entradas**

   * `ui_izquierda`: ← / A
   * `ui_derecha`: → / D
   * `ui_arriba`: ↑ / W
   * `ui_abajo`: ↓ / S
   * (Opcional) `ui_saltar`: Espacio / Z

---

## 5. Cómo Ejecutar

1. Abre Godot y selecciona la carpeta del proyecto.
2. En el panel de escena, marca `Main.tscn` como principal.
3. Pulsa **Play** (F5).
4. Controla al jugador con WASD o flechas, recoge piñas y observa el HUD.

---

## 6. Arquitectura de Código

* **Orquestador.gd**

  * Exporta `tiempo_respawn`, `objetivo`, referencia a `Moneda.tscn`
  * Al arrancar recopila puntos `P0…P5`, hace spawn inicial, conecta `TimerRespawn`
  * Métodos:

    * `_spawn_item(pos)`
    * `_on_item_recogido(valor, pos)`
    * `_on_timer_respawn()`
    * `_mostrar_victoria()`

* **Player.gd**

  * `extends CharacterBody2D`
  * Maneja `velocity = move_and_slide(...)` en `_physics_process`
  * Cambia animaciones según estado

* **Moneda.gd**

  * `extends Area2D`, `class_name Moneda`
  * Señal `moneda_recogida(valor, global_position)`
  * Conecta `body_entered` → emite señal y `queue_free()`

---

## 7. Flujo de Trabajo Git/GitHub

* **Ramas**: trabajamos en `master` (principal).
* **Commits**: mensajes claros, p.ej. `feature: añade respawn de ítems`
* **Push**: `git push origin master`
* **Pull Requests**: para features mayores (nuevo tipo de ítem, enemigos…)
* **Revisiones**: cada PR debe ser aprobado por al menos otro miembro

---

## 8. Roadmap de Clases y Temas

| Fecha      | Tema                            | Archivos Principales             |
| ---------- | ------------------------------- | -------------------------------- |
| 2025-06-27 | Orquestador, spawn/respawn      | `Orquestador.gd`                 |
| 2025-07-04 | Movimiento y animaciones Player | `Player.gd`, `Player.tscn`       |
| 2025-07-11 | Señales y UI avanzada           | `Moneda.gd`, `CanvasLayer/Label` |
| 2025-07-18 | Herencia: `MonedaPoder`         | `MonedaPoder.gd`                 |
| 2025-07-25 | Enemigos básicos y colisiones   | `Enemigo.gd`, `Enemy.tscn`       |
| …          | …                               | …                                |

---

## 9. Guía de Contribuciones

1. Haz **fork** del repo.
2. `git clone` tu fork y crea rama `feature/tu-nombre`.
3. Trabaja, commitea y haz **push** a tu fork.
4. Abre un **Pull Request** contra `master` de este repo.
5. Espera revisión, corrige comentarios y mergea.

---

## 10. Preguntas Frecuentes

* **Q**: ¿Por qué usamos un solo `TimerRespawn`?
  **A**: Para centralizar el delay tras cada recogida y mantener un único ítem en pantalla.

* **Q**: ¿Cómo añado otro tipo de ítem?
  **A**: Duplica `Moneda.tscn`, crea `MonedaPoder.gd extends Moneda`, exporta valores distintos y conecta su señal igual que el original.

* **Q**: ¿Puedo cambiar la resolución?
  **A**: Sí, en **Proyecto → Configuración → Pantalla → Ventana** ajusta la resolución base y modo de escalado.

---

## 11. Contactos y Créditos

* **Profesor / Mentor**: Alexis Java – [alexis.figueroa@est.fi.uncoma.edu.ar](mailto:alexis.figueroa@est.fi.uncoma.edu.ar)
* **Repositorio base**: [https://github.com/AlexisJava/curso\_godot](https://github.com/AlexisJava/curso_godot)
* **Licencia**: MIT © 2025 Club Programación Godot

---

✨ ¡Bienvenidos al Club de Programación en Godot! ¡A divertirse aprendiendo! 🎉
