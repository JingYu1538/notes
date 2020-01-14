### leaflet 自定义坐标系 

##### 通过下面方法自定义的坐标系，与图层无关，与传入的坐标点本身的坐标系一致，传入的坐标点坐标需要统一，此处使用了超图 API 

```javascript
import proj4 from "proj4"
import L from "leaflet"
import "@supermap/iclient-leaflet"
/*
 *@description: 获取 crs 信息
 *@param: epsgCodeStr crs 坐标系 可以通过 proj4 自定义
 *@param: bounds 矩形范围
 *@param: resolutions zoom 级别 分辨率
 */
/* point 的坐标系为图层的坐标系 */
let bounds = {
    left: pointA,
    bottom: pointB,
    right: pointC,
    top: pointD
}
proj4.defs(
        "EPSG:2436",
        "+proj=tmerc +lat_0=0 +lon_0=117 +k=1 +x_0=500000 +y_0=0 +ellps=krass +towgs84=15.8,-154.4,-82.3,0,0,0,0 +units=m +no_defs"
);
let epsgCodeStr = "EPSG:3857" || "EPSG:2436"
/* 如果数据坐标点和图层坐标点坐标系一致 则不需要这个属性 */
let mapcrs = L.CRS.EPSG3857 || getCRS(epsgCodeStr, bounds, resolutions) 

/* 获取 crs 信息 */
function getCRS(epsgCodeStr, bounds, resolutions) {
    return L.Proj.CRS(epsgCodeStr, {
      /* 像素坐标 表示矩形范围 */
      bounds: L.bounds(
           [bounds.left, bounds.bottom],
           [bounds.right, bounds.top]
       ),
       resolutions: resolutions,
       /* 坐标系原点 左上*/
       origin: [bounds.left, bounds.top]
    })
}

let map = L.map("Leafmap", {
     /* unproject 给定投影坐标 返回经纬度坐标 相反的 peoject 把地理坐标转换成 自定义坐标系 */
     /* 中心点坐标 */
     center: mapcrs.unproject(L.point(x,y)),
     maxZoom: 18,
     zoom: 6,
     crs: getCRS(epsgCodeStr, bounds, resolutions)
})
L.supermap.tiledMapLayer(url).addTo(map)

```



