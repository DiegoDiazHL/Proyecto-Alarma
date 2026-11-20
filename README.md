# 💊 Proyecto Alarma: recordatorio de medicación

App Android que te recuerda cuándo tomar cada medicamento. Para que la alarma no se pueda posponer sin más, **solo se apaga escaneando con la cámara el nombre del medicamento**. Así te aseguras de tener la caja en la mano y no solo de haber visto el aviso.

Proyecto académico de 2º de DAM (Desarrollo de Aplicaciones Multiplataforma).

## Funcionalidades

- **Lista de medicamentos** con el nombre, las tomas al día y la duración del tratamiento.
- **Alta de medicamentos**, con el nombre escrito a mano o leído de la caja con la cámara (OCR).
- **Varias horas de toma por medicamento**, comprobando que tengan el formato `HH:MM`.
- **Alarmas exactas** con `AlarmManager.setExactAndAllowWhileIdle`, que suenan aunque el móvil esté en ahorro de batería.
- **Pantalla de alarma** que enciende la pantalla, se muestra sobre la pantalla de bloqueo y repite un sonido hasta que se apaga.
- **Apagado por escaneo**: la app reconoce el texto de la foto con Google ML Kit y lo compara con los medicamentos guardados.
- **Guardado en el dispositivo**: la lista se guarda en el almacenamiento interno, así que no se pierde al cerrar la app.
- Pantalla de bienvenida (splash) y pantalla "Sobre nosotros".

## Tecnologías

| | |
|---|---|
| Lenguaje | Kotlin |
| Plataforma | Android (minSdk 24 · targetSdk 35) |
| UI | XML layouts + ViewBinding, RecyclerView, Material Components |
| Reconocimiento de texto | Google ML Kit Text Recognition |
| Cámara | Intent `ACTION_IMAGE_CAPTURE` (dependencias de CameraX incluidas) |
| Alarmas | `AlarmManager` + `BroadcastReceiver` |
| Persistencia | Serialización de objetos en un archivo interno |
| Build | Gradle (Kotlin DSL) con version catalog |

## Estructura

```
app/src/main/java/com/example/proyecto_alarma_pm/
├── SplashScreen.kt   # Pantalla de bienvenida (3 s) → PillList
├── PillList.kt       # Pantalla principal: lista de medicamentos
├── PillAdapter.kt    # Adaptador del RecyclerView
├── AddPill.kt        # Alta/baja de medicamentos, OCR y programación de alarmas (incluye AlarmReceiver)
├── Alarm.kt          # Pantalla que suena y se apaga escaneando el medicamento
├── DataManager.kt    # Guardar/cargar la lista en el almacenamiento interno
├── Pill.kt           # Modelo de datos (Serializable)
└── AboutUs.kt        # Información del equipo
```

## Cómo funciona

1. En **Añadir pastilla** escribes el nombre (o lo escaneas), añades una o más horas y la duración, y pulsas **Guardar**.
2. La app programa una alarma exacta para cada hora. Si esa hora ya ha pasado hoy, la programa para mañana.
3. A esa hora, `AlarmReceiver` abre la pantalla `Alarm`, que suena en bucle.
4. Pulsas **ESCANEA PARA APAGAR** y haces una foto al nombre del medicamento.
5. Si el texto reconocido coincide con algún medicamento guardado, la alarma se apaga. Si no, sigue sonando.

## Instalación

### Opción 1: compilar el proyecto

Requisitos: Android Studio (reciente) y JDK 17.

```bash
git clone https://github.com/DiegoDiazHL/Proyecto-Alarma.git
```

Abre la carpeta en Android Studio, espera a que Gradle sincronice y ejecuta la app en un emulador o en un móvil.

Desde la terminal:

```bash
./gradlew assembleDebug
```

### Opción 2: instalar el APK

Si hay una versión publicada en la sección *Releases*, descarga el `.apk` en el móvil y ábrelo. Para instalarlo tienes que permitir la instalación de apps de origen desconocido.

### Permisos

- **Cámara**: para escanear el nombre del medicamento.
- **Alarmas exactas** (`SCHEDULE_EXACT_ALARM` / `USE_EXACT_ALARM`): para que el aviso suene a la hora exacta.

## Limitaciones conocidas e ideas de mejora

- Cada alarma suena una sola vez. Falta reprogramarla cada día durante toda la duración del tratamiento.
- Las alarmas se pierden si se reinicia el móvil. Se podría arreglar escuchando `BOOT_COMPLETED`.
- El escaneo compara todo el texto reconocido con el nombre guardado, así que si en la foto sale más texto, no coincide. Se podría buscar el nombre dentro del texto o leer el código de barras o QR de la caja.
- Los textos de la interfaz solo están en español.

## Equipo

Desarrollado por el equipo de 2DAM_M:

- Jorge Díaz Esteban
- Diego Díaz-Hellín López
- Rodrigo Herrero Hernández
- Pablo Mata de la Fuente
