# EmuSanti — Lakka para Rockchip RK322X

## 1. Descripción

EmuSanti es un proyecto comunitario basado en Lakka y LibreELEC, adaptado para TV Box con procesadores Rockchip de la familia RK322X.

Su objetivo es convertir estos dispositivos en pequeñas consolas de emulación retro mediante RetroArch y su ecosistema de emuladores.

**Proyecto independiente y experimental. No es una versión oficial de Lakka ni de LibreELEC.**

## 2. Hardware objetivo

La adaptación se centra inicialmente en dispositivos con estas características:

- SoC: Rockchip RK3228A.
- Memoria RAM: 1 GB.
- Arquitectura: ARMv7, 32 bits.
- Plataforma: Rockchip RK322X.

La compatibilidad puede variar según la placa, la memoria, el Wi-Fi y el bootloader de cada TV Box.

## 3. Base técnica

La versión actual utiliza:

- Lakka 6.1.
- LibreELEC Build System.
- Plataforma Rockchip.
- Dispositivo RK322X.
- Kernel Linux 6.18.19.
- U-Boot 2025.07.
- RetroArch 1.22.2.
- Arquitectura ARM de 32 bits.

## 4. Cambios y adaptaciones realizadas

### 4.1. Integración de Lakka 6.1

Se han incorporado las actualizaciones de Lakka 6.1 manteniendo las adaptaciones específicas para RK322X.

### 4.2. Adaptación de controladores Wi-Fi al kernel 6.18

Se han realizado correcciones para adaptar controladores antiguos a las interfaces del kernel:

**SSV6051**
- Actualización de la firma de `ssv6200_config`.
- Incorporación del parámetro `link_id`.

**ESP8089**
- Actualización de `esp_op_config`.
- Actualización de `esp_op_set_rts_threshold`.
- Incorporación del parámetro `link_id`.

**RTL8189ES**
- Adaptación de `cfg80211_rtw_set_wiphy_params`.
- Adaptación de `cfg80211_rtw_set_txpower`.
- Adaptación de `cfg80211_rtw_get_txpower`.
- Compatibilidad condicional con kernels Linux 6.18 y posteriores.

Estas modificaciones permiten adaptar los controladores a las interfaces del kernel utilizado.

## 5. Compilación nativa en Linux

La compilación de EmuSanti se realiza directamente en Linux mediante el sistema de construcción de LibreELEC.

**No se necesita Docker.**

### Requisitos

- Linux de 64 bits.
- Git.
- GCC y herramientas de desarrollo.
- Make.
- Python 3.
- Conexión a Internet.
- Espacio libre suficiente para descargar las fuentes y compilar.

Se recomienda disponer de varios GB de RAM y suficiente espacio libre en disco.

### 5.1. Comprobar dependencias

Desde la carpeta raíz del repositorio:

```bash
scripts/checkdeps
```

Este comando comprueba las dependencias necesarias para compilar.

### 5.2. Compilar EmuSanti para RK322X

Comando utilizado para generar la imagen:

```bash
BUILDER_NAME=EmuSanti PROJECT=Rockchip DEVICE=RK322X ARCH=arm make image
```

Configuración utilizada:

- `BUILDER_NAME=EmuSanti`: nombre del builder.
- `PROJECT=Rockchip`: plataforma Rockchip.
- `DEVICE=RK322X`: dispositivo objetivo.
- `ARCH=arm`: arquitectura ARM de 32 bits.
- `make image`: construye la imagen del sistema.

La compilación incluye **73 cores de Libretro** seleccionados para la plataforma ARM de 32 bits.

Los archivos generados se encuentran en la carpeta `target/`.

## 6. Repositorio y ramas

Repositorio GitHub:

https://github.com/santibb80-svg/Lakka-RK322x

Rama de desarrollo:

`lab/lakka-6.1-update`

Rama de respaldo:

`backup/rk322x-port-original`

Repositorio oficial utilizado como referencia:

https://github.com/libretro/Lakka-LibreELEC

Los cambios específicos de RK322X se mantienen en este fork.

## 7. EmuSanti 1.0

La primera versión funcional se ha compilado y publicado como GitHub Release.

**Descarga:**

https://github.com/santibb80-svg/Lakka-RK322x/releases/tag/EmuSanti-1.0

Características de esta versión:

- Lakka 6.1.
- RetroArch 1.22.2.
- ARMv7 de 32 bits.
- 73 cores de Libretro.
- Adaptación para TV Box Rockchip RK322X.

La imagen se ha probado en hardware real con un TV Box Rockchip RK3228A.

Se ha comprobado el arranque del sistema, la interfaz de RetroArch y la detección de un mando Xbox 360.

## 8. Limitaciones y advertencias

- Proyecto experimental y en desarrollo.
- No todos los TV Box RK322X utilizan la misma placa o configuración de hardware.
- La compatibilidad Wi-Fi depende del controlador instalado en cada dispositivo.
- No se garantiza el funcionamiento de todas las características en todos los modelos.
- Se recomienda conservar una copia de seguridad del firmware original antes de instalar una imagen nueva.

La instalación y las pruebas se realizan bajo responsabilidad del usuario.

## 9. Créditos y agradecimientos

Este proyecto se apoya en el trabajo de:

- Lakka: https://www.lakka.tv/
- Lakka LibreELEC: https://github.com/libretro/Lakka-LibreELEC
- LibreELEC: https://libreelec.tv/
- Comunidad Rockchip.
- Desarrolladores originales de los controladores Wi-Fi utilizados.

Se respetan las licencias originales de los componentes y controladores incluidos.

## 10. Filosofía del proyecto

EmuSanti nace con la intención de dar una segunda vida a los TV Box Rockchip de bajo coste, manteniendo el soporte de estos dispositivos y facilitando su utilización como pequeñas consolas de emulación retro.

El proyecto busca conservar las adaptaciones existentes, corregir incompatibilidades y documentar los avances para que otros usuarios puedan colaborar y continuar su desarrollo.

**EmuSanti — Una segunda vida para los TV Box Rockchip.**
