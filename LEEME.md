# Lakka RK322X

## Proyecto de adaptación de Lakka para TV Box Rockchip RK322X

Lakka RK322X es un proyecto comunitario cuyo objetivo es adaptar la distribución Linux Lakka a TV Box basados en procesadores Rockchip de la familia RK322X.

Lakka transforma pequeños dispositivos en consolas de emulación retro mediante RetroArch y su ecosistema de emuladores.

Este proyecto parte del código de Lakka/LibreELEC y conserva las adaptaciones específicas necesarias para los dispositivos RK322X.

**Proyecto independiente y experimental. No es una versión oficial de Lakka ni de LibreELEC.**

---

## 1. Objetivo del proyecto

El objetivo principal es recuperar, mantener y mejorar la compatibilidad de TV Box Rockchip RK322X con versiones recientes de Lakka.

El desarrollo se centra inicialmente en dispositivos con las siguientes características:

- Procesador: Rockchip RK3228A.
- Memoria RAM: 1 GB.
- Arquitectura: ARM.
- Plataforma: RK322X.

La compatibilidad puede variar según el fabricante, la placa, la memoria, el Wi-Fi y el bootloader de cada dispositivo.

---

## 2. Base del proyecto

El proyecto utiliza como base:

- Lakka 6.1.
- LibreELEC Build System.
- Plataforma Rockchip.
- Configuración específica RK322X.
- Kernel Linux 6.18.19 en la compilación realizada.

Se ha integrado la actualización oficial de Lakka 6.1, manteniendo las adaptaciones originales para RK322X.

---

## 3. Cambios y mejoras realizadas

### 3.1. Integración de Lakka 6.1

Se han incorporado las actualizaciones del proyecto oficial Lakka 6.1 a la rama de desarrollo, conservando los archivos y configuraciones específicos de RK322X.

### 3.2. Compatibilidad de controladores Wi-Fi con Linux 6.18

Durante la compilación se detectaron incompatibilidades entre varios controladores Wi-Fi antiguos y las nuevas firmas de funciones de la API inalámbrica del kernel.

Se han realizado las siguientes correcciones:

#### SSV6051

Archivo:

`projects/Rockchip/devices/RK322X/patches/linux/default/wifi-driver-ssv6051.patch`

Modificación:
- Actualización de la firma de `ssv6200_config`.
- Incorporación del parámetro `link_id` requerido por el kernel.

#### ESP8089

Archivo:

`projects/Rockchip/devices/RK322X/patches/linux/default/wifi-driver-esp8089.patch`

Modificaciones:
- Actualización de `esp_op_config`.
- Actualización de `esp_op_set_rts_threshold`.
- Incorporación del parámetro `link_id` en ambas funciones.

#### RTL8189ES

Nuevo archivo:

`projects/Rockchip/devices/RK322X/packages/RTL8189ES/patches/rtl8189es-kernel-6.18.patch`

Modificaciones:
- Adaptación de `cfg80211_rtw_set_wiphy_params`.
- Adaptación de `cfg80211_rtw_set_txpower`.
- Adaptación de `cfg80211_rtw_get_txpower`.
- Incorporación condicional de parámetros para kernels Linux 6.18 y posteriores.

Estas correcciones permiten adaptar las firmas de los controladores a las interfaces esperadas por el kernel.

### 3.3. Modificación del entorno Docker

Archivo:

`tools/docker/noble/Dockerfile`

Se ha incorporado el paquete `curl` a la imagen Docker basada en Ubuntu 24.04 Noble, necesario para completar correctamente el proceso de compilación.

---

## 4. Entorno de compilación

La compilación se realiza mediante Docker utilizando el sistema de construcción de LibreELEC.

### Requisitos

- Linux de 64 bits.
- Docker instalado y funcionando.
- Git.
- Conexión a Internet.
- Espacio libre suficiente para descargar las fuentes y compilar.

### 4.1. Construir la imagen Docker

Desde la carpeta raíz del proyecto:

```bash
sudo docker build --pull -t libreelec tools/docker/noble


4.2. Compilar Lakka para RK322X

Comando utilizado para la compilación:

sudo docker run -it --rm --log-driver none --user 1000:1000 -v "$PWD":/build -w /build -e HOME=/tmp -e BUILDER_NAME=EmuSanti -e BUILD_PERIODIC=nightly -e PROJECT=Rockchip -e DEVICE=RK322X -e ARCH=arm -e UBOOT_SYSTEM=rk322x -e DISABLE_LIBRETRO_CORES=yes libreelec make image


Configuración utilizada:
- BUILDER_NAME: EmuSanti.
- BUILD_PERIODIC: nightly.
- PROJECT: Rockchip.
- DEVICE: RK322X.
- ARCH: arm.
- UBOOT_SYSTEM: rk322x.
- DISABLE_LIBRETRO_CORES: yes.
Importante: La opción DISABLE_LIBRETRO_CORES=yes compila el sistema sin incluir los cores de Libretro.

El parámetro --user 1000:1000 corresponde al usuario utilizado en el entorno de desarrollo original. Puede ser necesario adaptarlo en otros equipos.

5. Repositorio y ramas

Repositorio GitHub:

https://github.com/santibb80-svg/Lakka-RK322x

Rama de desarrollo:

lab/lakka-6.1-update

Rama de respaldo:

backup/rk322x-port-original

El repositorio oficial se utiliza como referencia para integrar actualizaciones:

https://github.com/libretro/Lakka-LibreELEC

Los cambios específicos de RK322X se mantienen dentro de este repositorio.

6. Estado del proyecto
Estado: compilación completada correctamente con las correcciones incorporadas.

Se han conseguido:

- Integrar Lakka 6.1.
- Mantener las adaptaciones específicas RK322X.
- Corregir los errores de compilación de SSV6051.
- Corregir los errores de compilación de ESP8089.
- Corregir los errores de compilación de RTL8189ES.
- Actualizar el entorno Docker con curl.
- Completar la compilación del sistema.
La compilación correcta no implica que todas las funciones estén verificadas en todos los modelos de TV Box.

Es necesario realizar pruebas en el dispositivo físico para comprobar:

- Arranque del sistema.
- Salida de vídeo y resolución.
- Sonido.
- Conectividad Wi-Fi.
- Mandos y dispositivos USB.
- Rendimiento de la emulación.

7. Limitaciones y advertencias

- Proyecto experimental y en desarrollo.
- No todos los TV Box RK322X utilizan la misma placa o configuración de hardware.
- La compatibilidad Wi-Fi depende del controlador instalado en cada dispositivo.
- La compilación descrita se ha realizado sin cores de Libretro.
- No se garantiza la compatibilidad con todos los modelos.
- Se recomienda conservar una copia de seguridad del firmware original antes de probar una imagen nueva.
La instalación y las pruebas se realizan bajo responsabilidad del usuario.

8. Créditos y agradecimientos

Este proyecto se apoya en el trabajo de los desarrolladores y comunidades de:
- Lakka: https://www.lakka.tv/
- Lakka LibreELEC: https://github.com/libretro/Lakka-LibreELEC
- LibreELEC: https://libreelec.tv/
- Comunidad Rockchip.
- Desarrolladores originales de los controladores Wi-Fi utilizados.
Se respetan las licencias originales de los componentes y controladores incluidos.

9. Filosofía del proyecto

Lakka RK322X nace con la intención de dar una nueva oportunidad a TV Box Rockchip de bajo coste, manteniendo vivo el soporte de estos dispositivos y facilitando su utilización como pequeñas consolas de emulación retro.

El proyecto busca conservar las adaptaciones existentes, corregir incompatibilidades y documentar los avances para que otros usuarios puedan colaborar y continuar su desarrollo.

Lakka RK322X — Una segunda vida para los TV Box Rockchip.
