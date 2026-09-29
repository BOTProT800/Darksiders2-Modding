# Darksiders II Modding

Guía y consejos para crear mods de **Darksiders II: Deathinitive Edition** en PC.

Aquí se reúne lo que se sabe de los archivos del juego: qué se puede cambiar hoy,
con qué herramienta y cómo hacerlo sin romper la instalación.

> Este repositorio no contiene archivos del juego. Para seguir la guía necesitas tu
> propia copia de Darksiders II Deathinitive Edition; las herramientas se han probado
> con la versión de Steam.

## Qué se puede modear hoy

| Qué | Cómo | Estado |
|---|---|---|
| Texturas con el mismo formato, tamaño y mipmaps | Darksiders2DLL | Probado en el juego |
| Texturas a otra resolución, o PNG | DLL | Experimental |
| Forma de los modelos (`.2`) | Addon de Blender y la DLL | Probado solo con la cabeza de Death |
| Huecos del inventario por categoría | La DLL | Experimental, sin probar en partidas |
| Audio, interfaz, textos, materiales y animaciones | — | Solo se pueden extraer |

**Probado** quiere decir que el cambio se ha visto en el juego. **Experimental**
quiere decir que pasa las pruebas, pero falta confirmarlo en el juego.

## Por dónde empezar

1. [Primeros pasos](docs/primeros-pasos.md): comprueba tu versión del juego y haz
   una copia de seguridad.
2. [Elegir método](docs/metodos.md): Mod Manager o DLL, con más detalle.
3. [Tu primera textura](docs/guias/texturas.md): cambia paso a paso un icono de
   32×32 del menú, y deshaz el cambio.

## Cómo está organizado el juego

```text
Darksiders II Deathinitive Edition/
├── Darksiders2.exe
└── media/
    ├── manifest.bin          índice: en qué .upak está cada archivo y dónde
    ├── media.upak            ~7 GB: texturas, modelos, interfaz, sonido…
    ├── maps.upak             mapas
    ├── anim_streams.upak     streams de animación, sin índice
    ├── sounds_streamed/PC/   audio de Wwise (core.pck, en.pck, es.pck)
    ├── video/                vídeos Bink (.bik) y subtítulos
    └── scripts.obsp          scripts del juego
```

Dentro de `media.upak` y `maps.upak` hay 1.681 paquetes con 75.627 archivos, de los
que 15.254 son texturas DDS. Cada archivo tiene una ruta virtual, por ejemplo
`media/ui/ui_shell/newgameplus.dds`, y **un mod es una carpeta que repite esa ruta**:

```text
MiMod/
└── media/ui/ui_shell/newgameplus.dds
```

## Forma de instalar un mod

| | Darkside Mod Manager | Darksiders2DLL |
|---|---|---|
| Cómo funciona | Sustituye el archivo dentro del `.upak`, después de guardar una copia del original | Se instala como `dinput8.dll` y carga los mods de la carpeta `mods/` al arrancar el juego; no toca los `.upak` |
| Formato del mod | Carpeta, ZIP o RAR con la ruta del juego | `mods/<nombre>/media/…` |
| Límite principal | El paquete modificado tiene que caber en el espacio del original, con un margen de unos cientos de bytes | Solo funciona con un `Darksiders2.exe` concreto (ver [Requisitos](#requisitos)) |
| Deshacer | «Restaurar todo» en el propio programa | Sacar el mod de `mods/` o desinstalar la DLL |

**¿Cuál elijo?**

- Para texturas del mismo tamaño y formato sirven los dos.
- Para texturas más grandes, PNG, modelos o inventario, la DLL.
- Si usas el DLL Loader de LOKI o mods que dependen de él, como Custom FOV o DS2LM,
  el Mod Manager: la DLL ocupa el mismo `dinput8.dll` y no pueden convivir.

## Herramientas

| Herramienta | Para qué sirve | Disponible |
|---|---|---|
| Darkstractor | Explorar y extraer todo el contenido del juego sin modificarlo
| [Darksiders2DLL](https://github.com/BOTProT800/Darksiders2DLL) | Cargar mods sueltos al arrancar el juego: texturas, modelos e inventario | Código abierto; sin versión compilada todavía |

De terceros:

- Texturas: [Paint.NET](https://www.getpaint.net/) o [GIMP](https://www.gimp.org/) con soporte DDS.
- Modelos: [Blender](https://www.blender.org/) 4.2 o posterior.
- Audio: [vgmstream](https://github.com/vgmstream/vgmstream),
  [wwiser](https://github.com/bnnm/wwiser) y [ww2ogg](https://github.com/hcs64/ww2ogg).

## Requisitos

- Darksiders II Deathinitive Edition para Windows (en Steam, App ID 388410).
- La DLL solo funciona con el `Darksiders2.exe` cuyo SHA-256 es
  `5580738EF70BC5BBCC72D7DC4A9C319956CD14DBFEF6F9DBEC54C1B5D97799FB`; con otro
  ejecutable desactiva los mods. Para comprobarlo, abre PowerShell en la carpeta
  del juego y ejecuta `Get-FileHash .\Darksiders2.exe`.
- La edición original de 2012 no está verificada.

## Consejos rápidos

- Cierra el juego antes de instalar o desinstalar nada.
- Salvo que uses el modo flexible o las texturas HD de la DLL, exporta el DDS con la
  misma resolución y el mismo número de mipmaps que el original. Los mipmaps de más
  son el error más común.
- Haz un cambio cada vez y pruébalo en el juego antes del siguiente.
- Prueba los cambios de inventario con una copia de tu partida.
- Si un mod de la DLL no aparece, mira su log en `%LOCALAPPDATA%\Darksiders2DLL\logs`.

## Contribuir

¿Has descubierto algo de un formato o tienes un truco que funciona? Abre un issue o
un pull request. Cita siempre de dónde sale la información y no subas archivos del
juego.

## Créditos

Esta guía se apoya en la investigación de la comunidad: DS2extract, los hilos de
ZenHAX sobre los manifiestos y sobre la Deathinitive Edition (con Doctor Loboto
entre sus participantes), QuickBMS de aluigi y sus scripts para Darksiders, el
plugin de Noesis de finale00, los maxscripts de zaramot y los scripts de Blender
de Szkaradek123. La lista completa, con fuentes y licencias, estará en la
referencia de créditos.

## Aviso

Darksiders II y todos sus archivos pertenecen a sus propietarios. Este proyecto no
está afiliado con ellos y no distribuye contenido del juego.