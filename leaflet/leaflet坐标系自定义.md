### leaflet 自定义坐标系 

##### 通过下面方法自定义的坐标系，与图层无关，与传入的坐标点本身的坐标系一致即可，传入的坐标点坐标需要统一

```javascript
import proj4 from "proj4";
import L from "leaflet";
/*
 *@description: 获取 crs 信息
 *@param: epsgCodeStr crs 坐标系 可以通过 proj4 自定义
 *@param: bounds 矩形范围
 *@param: resolutions zoom 级别 分辨率
 */
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
    });
}

let map = L.map("Leafmap", {
     /* unproject 给定投影坐标 返回经纬度坐标 相反的 peoject 把地理坐标转换成 自定义坐标系 */
     /* 中心点坐标 */
     center: mapcrs.unproject(L.point(500749.6532526612, 305012.3241956122)),
     maxZoom: 18,
     zoom: 6,
     crs: getCRS(epsgCodeStr, bounds, resolutions)
});
```

