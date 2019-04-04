# leaflet-marker
应用于制作飞机航线图

##绘制基础地图
```
<!-- 插入到id为mapid的元素 8为地图级别-->
var mymap = L.map('mapid').setView([xxx,xxx], 8),
```
## 地图拖动或缩放调接口
```
<!-- 地图左上角缩放，拖动调用接口  -->
	mymap.on('zoomend dragend',function(){
		mymap.getZoom(); //获取范围经纬度
		getMap(); //获取接口方法
	})

```
## marker自定义图标
### 1.如果自定义几个图标以内，可以自定义地址
```
var greenIcon = L.icon({
    iconUrl: 'leaf-green.png', 
    shadowUrl: 'leaf-shadow.png',

    iconSize:     [38, 95], // size of the icon
    shadowSize:   [50, 64], // size of the shadow
    iconAnchor:   [22, 94], // point of the icon which will correspond to marker's location
    shadowAnchor: [4, 62],  // the same for the shadow
    popupAnchor:  [-3, -76] // point from which the popup should open relative to the iconAnchor
});
```
### 2.如果是范围内的很多图标，且都一样，如下：
```
<!-- 循环多个图标，我这里引入了字体图标 -->
var myIcon = L.divIcon({className: 'iconfont icon-99'}),
var res = data //接口返回数据，通过经纬度定位图标位置
$.each(res,function(i){
	let arr = [];
	arr.push(res[i].latitude)
	arr.push(res[i].longitude)
	var marker = L.marker(arr,{icon: myIcon}).addTo(mymap);
}）
```
### 3.如果要给marker 添加事件
```
marker.on('click',function(e){
	//function
}).on('mouseover',function(){
	//鼠标移入设置自定义样式 hovColor及文本
	marker.setIcon(L.divIcon({className: 'iconfont icon-99 hovColor',
		html:'<span class="tag">'+res[i].flight+'</span>'}))
}).on('mouseout',function(){
	marker.setIcon(L.divIcon({className: 'iconfont icon-99'}))
})
```
### 4.marker获取数据重新渲染
重新渲染需要先清除原标记再添加，且每一个marker都要重新渲染，需要使用LayerGroup组先存放marker
```
var myLayerGroup = new L.LayerGroup();
myLayerGroup.clearLayers(); //循环marker赋值前先清除清除marker标记组，便于重绘

<!-- marker 循环赋值后--> 
myLayerGroup.addLayer(marker);
mymap.addLayer(myLayerGroup);

```
## 绘制路线
当点击某个航班时，需要接口返回该航班历史经纬度数组，且实时返回，这时可以新建个数组变量latlngs，实时push数组给他，每一次push后重绘路线，看上去就像路线跟着飞机后面动
```
L.polyline(latlngs, {color: 'red',opacity:'0.8',weight:'1'}).addTo(mymap);
```
[参考地址](https://zh.flightaware.com/live/)