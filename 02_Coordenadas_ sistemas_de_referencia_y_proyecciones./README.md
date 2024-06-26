# 2. Coordenadas, sistemas de referencia y proyecciones.
![scheme](./scheme.png)

## 2.1. 🧭 Coordenadas
Par de valores numéricos que representan la ubicación de un punto en la superficie de la Tierra. Estos valores, comúnmente expresados en grados decimales de latitud y longitud (cartográficas). En el caso de **CesiumJS** podemos entrar:
  * **Cartesianas**  
    Las coordenadas cartesianas se utilizan para representar puntos en un espacio bidimensional o tridimensional.
    Se representan como (x, y) para coordenadas 2D o (x, y, z) para coordenadas 3D.
  

    * **Cartesian2**: Un punto en coordenadas cartesianas en 2D (x,y).  
      [📘 Doc](https://cesium.com/learn/cesiumjs/ref-doc/Cartesian2.html)

     * **Cartesian3**: Un punto en coordenadas cartesianas en 3D (x, y, z).  
      [📘 Doc](https://cesium.com/learn/cesiumjs/ref-doc/Cartesian3.html)
    
      * **Cartesian4**: Un punto en coordenadas cartesianas en 4D (x, y, z, w).      
      La **"W"** representa el tiempo, un momento específico en el tiempo.  
      [📘 Doc](https://cesium.com/learn/cesiumjs/ref-doc/Cartesian4.html)
  * **Cartográficas**   
   Normalmente se expresan en latitud y longitud, que son medidas angulares con respecto al ecuador y el meridiano de Greenwich, respectivamente.

    * **Cartographic**: Las coordenadas son definidas por la longitud, latitud y la altura (longitude, latitude, height).  
      [📘 Doc](https://cesium.com/learn/cesiumjs/ref-doc/Cartographic.html)  
      
    * **CartographicGeocoderService**: Geocodifica consultas que contienen coordenadas cartográficas (longitude, latitude, height).  
      [📘 Doc](https://cesium.com/learn/cesiumjs/ref-doc/CartographicGeocoderService.html)  
      
   ```JavaScript
   import { Cartesian2, Cartesian3, Cartesian4, Cartographic } from 'cesium';
   const cat2 = new Certesian2(x, y);
   ```
### 2.1.1. 🔄 Transformaciones de Coordenadas
* **Coordenadas Cartográficas a Cartesianas** | [📘 Doc](https://cesium.com/learn/cesiumjs/ref-doc/Cartographic.html?classFilter=Cartographic#.toCartesian)
  ```javascript
  const cartographic = Cesium.Cartographic.fromDegrees(INITIAL_LONGITUDE, INITIAL_LATITUDE, INITIAL_HEIGHT);
  const cartesian = Cesium.Cartographic.toCartesian(cartographic);
  ```
* **Coordenadas Cartesianas a Cartográficas** | [📘 Doc](https://cesium.com/learn/cesiumjs/ref-doc/Ellipsoid.html?classFilter=Ellipsoid#cartesianToCartographic)
  ```javascript
  const cartographic = Cesium.Ellipsoid.WGS84.cartesianToCartographic(cartesian);
  ```

 ▶️ Transformaciones de Coordenadas: [📋 HTML](https://github.com/AlvaroCodes/cesiumJS_notebook/blob/main/02_Coordenadas_%20sistemas_de_referencia_y_proyecciones./examples/01_transformCoord.html)  | 🚀[CodePen](https://codepen.io/AlvaroCodes/pen/MWdeEZP)  

### 2.1.2. 📐 Grados (Degrees) y Radianes (Radians)
* **Grados (Degrees):** Se utilizan para representar latitud y longitud (uso más común).
* **Radianes (Radians):** Unidad estándar para medir ángulos (cálculo matemáticos precisos).
```javascript
// Crear una posición usando radianes
const latitudeRadians = Cesium.Math.toRadians(40.7128);
const longitudeRadians = Cesium.Math.toRadians(-74.0060);
const height = 100000; // en metros

const positionRadians = new Cesium.Cartographic(longitudeRadians, latitudeRadians, height);
```

 ▶️ radiansDestination: [📋 HTML](https://github.com/AlvaroCodes/cesiumJS_notebook/blob/main/02_Coordenadas_%20sistemas_de_referencia_y_proyecciones./examples/02_radiansDestination.html)  | 🚀[CodePen](https://codepen.io/AlvaroCodes/pen/WNBxXoa)  
<br/>
<br/>
**Conversión entre Grados y Radianes**  
Se utiliza la clase Cesium.Math para realizar conversiones entre grados y radianes, así como otras operaciones matemáticas.  
  * De grados a radianes | [📘 Doc](https://cesium.com/learn/cesiumjs/ref-doc/Math.html?classFilter=math#.toRadians)  
    ```javascript
    const radians = Cesium.Math.toRadians(degrees);
    ```
  * De radianes a grados | [📘 Doc](https://cesium.com/learn/cesiumjs/ref-doc/Math.html?classFilter=math#.toDegrees)  
    ```javascript
    const degrees = Cesium.Math.toDegrees(radians);
    ```

## 2.2. 🗺️ Proyecciones y Sistema de referencia
En CesiumJS, la proyección geográfica por defecto es EPSG:4326 (WGS84). Cuando se trabaja con la proyección 2D, CesiumJS utiliza EPSG:3857 (Web Mercator).

  * **WebMercatorProjection (EPSG:3857)** | [📘 Doc](https://cesium.com/learn/ion-sdk/ref-doc/WebMercatorProjection.html)
    
  * **GeographicProjection (EPSG:4326)** | [📘 Doc](https://cesium.com/learn/ion-sdk/ref-doc/GeographicProjection.html)

   ```JavaScript
   import { Viewer, WebMercatorProjection } from 'cesium';
   const viewer = new Viewer("cesiumContainer", mapProjection: new WebMercatorProjection());
   ```

Desde la escena se puede hacer el "getProjection":
```javascript
function getProjection() {
  // Obtener proyección del mapa: Aunque cambie del 3D al 2D parece que no cambia la proyección.
  console.log(viewer.scene.mapProjection);
  if (viewer.scene.mapProjection instanceof Cesium.WebMercatorProjection) {
    console.log('EPSG:3857');
  } else if (viewer.scene.mapProjection instanceof Cesium.GeographicProjection) {
    console.log('EPSG:4326');
  }
}
```

### 2.2.1. 🔵 Dimensiones Geoespaciales (Rectangle y Ellipsoid)
* **Rectangle**: Rectángulo en coordenadas geográficas (longitud y latitud). Útil para definir áreas en la superficie, como zonas de visualización o regiones de interés.  
    [📘 Doc](https://cesium.com/learn/cesiumjs/ref-doc/Rectangle.html?classFilter=recta)
  
* **Ellipsoid**: Las coordenadas también se pueden asociar con elipsoides personalizados, no solo con la forma estándar de la Tierra (WGS84). Esto es útil para simulaciones o representaciones de otros cuerpos celestes.  
   [📘 Doc](https://cesium.com/learn/cesiumjs/ref-doc/Ellipsoid.html?classFilter=ellips)

