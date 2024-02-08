
<template>
  <view class="mainbody">
    <view id="cesiumContainer"></view>

    <view class="title">
      <webTitle></webTitle>
    </view>
    <view class="leftContent">
      <leftContent></leftContent>
    </view>
    <view class="rightContent">
      <rightContent></rightContent>
    </view>

    
    <view class="mid-botton">
      <el-popover placement="top" :width="400" trigger="click">
        <template #reference>
          <el-button @click="visible = true">切换位置</el-button>
        </template>
        <view class="bnt_group">
          <el-button @click="to_base">回原点</el-button>
          <el-button @click="to_zwy">切换植物园</el-button>
          <el-button @click="to_lyh">飞到涞源湖</el-button>
        </view>
      </el-popover>

      <el-popover placement="top" :width="550" trigger="click">
        <template #reference>
          <el-button @click="visible = true">添加标记</el-button>
        </template>
        <view class="bnt_group">
          <el-checkbox v-model="tdt_show" @change="tdt_add" label="天地图" border />
          <el-checkbox v-model="gddt_show" @change="gddt_add" label="高德地图" border />
          <el-checkbox v-model="mark_show" @change="mark_add" label="添加点" border />
          <el-checkbox v-model="bj_show" @change="bj_add" label="添加边界" border />
        </view>
      </el-popover>

      <el-button @click="get_position">获取位置</el-button>
    </view>


  </view>
</template>

<script setup>
import * as Cesium from 'cesium'
import { ref, onMounted } from 'vue';
import { ElNotification } from 'element-plus'
import webTitle from './components/title.vue'
import leftContent from './components/leftcontent.vue'
import rightContent from './components/rightcontent.vue'

const viewer = ref(null);

// 默认加载
onMounted(() => {
  viewer.value = new Cesium.Viewer('cesiumContainer',{
    animation:false,
    geocoder:false,
    homeButton:false,
    baseLayerPicker:false,
    navigationHelpButton:false,
    timeline:false,
    sceneModePicker:false,
    infoBox:false
  })

  viewer.value.camera.setView({
    destination: {
      x: -2142604.1624498027, y: 4496503.382473746, z: 3989473.4201492015
    }
  })
}
)

// 切换地点，原点
const to_base = () => {
  viewer.value.camera.setView({
    destination: {
      x: -2142604.1624498027, y: 4496503.382473746, z: 3989473.4201492015
    }
  })
}

// 切换地点，植物园
const to_zwy = () => {
  viewer.value.camera.setView({
    destination: Cesium.Cartesian3.fromDegrees(115.47926460119214, 38.91884348091114, 2565.328680250685)
  })
}
// 切换地点，涞源湖
const to_lyh = () => {
  viewer.value.camera.flyTo({
    destination: Cesium.Cartesian3.fromDegrees(114.73584146346973, 39.33895007153513, 4180.8551463136)
  })
}

// 获取坐标
const get_position = () => {
  var cameraPosition = viewer.value.camera.positionCartographic;
  var lon = Cesium.Math.toDegrees(cameraPosition.longitude);
  var lat = Cesium.Math.toDegrees(cameraPosition.latitude)
  var height = cameraPosition.height;

  ElNotification({
    title: '当前位置',
    dangerouslyUseHTMLString: true,
    message: viewer.value.camera.position + '<br>lon:' + lon + '<br>lat:' + lat + '<br>height:' + height
  })
}

const tdt_show = ref(false)
const tdtLayer_base = ref(null)
const tdtLayer_name = ref(null)
const tdt_add = () => {
  if (tdt_show.value) {
    // 路名
    const tdtLayer_lm_add = new Cesium.UrlTemplateImageryProvider({
      url: "http://t0.tianditu.com/DataServer?T=cva_w&x={x}&y={y}&l={z}&tk=d0065784f897d3562ed5e2503c096939",
      minimumLevel: 3,
      maximumLevel: 18,
    });

    // 底图
    const tdtLayer_dt_add = new Cesium.UrlTemplateImageryProvider({
      url: "http://t0.tianditu.com/DataServer?T=img_w&x={x}&y={y}&l={z}&tk=d0065784f897d3562ed5e2503c096939",
      minimumLevel: 3,
      maximumLevel: 18,
    });

    tdtLayer_base.value = viewer.value.imageryLayers.addImageryProvider(tdtLayer_dt_add);
    tdtLayer_name.value = viewer.value.imageryLayers.addImageryProvider(tdtLayer_lm_add);
  } else {
    // 取消底图
    var layerIndex = viewer.value.imageryLayers.indexOf(tdtLayer_base.value)
    if (layerIndex != -1) {
      viewer.value.imageryLayers.remove(viewer.value.imageryLayers.get(layerIndex))

    }
    // 取消路名
    var layerIndex = viewer.value.imageryLayers.indexOf(tdtLayer_name.value)
    if (layerIndex != -1) {
      viewer.value.imageryLayers.remove(viewer.value.imageryLayers.get(layerIndex))
    }
  }

}


// 添加高德地图
const gddt_show = ref(false)
const gddtLayer_base = ref(null)
const gddtLayer_name = ref(null)
const gddt_add = () => {
  if (gddt_show.value) {


    // 添加高德重点标记，道路名称等
    const Layer_lm_add = new Cesium.UrlTemplateImageryProvider({
      url: "http://webst02.is.autonavi.com/appmaptile?x={x}&y={y}&z={z}&lang=zh_cn&size=1&scale=1&style=8",
      minimumLevel: 3,
      maximumLevel: 18,
    })
    // 添加高德地图，底图也换成高德，要不有偏移
    const Layer_dt_add = new Cesium.UrlTemplateImageryProvider({
      url: "http://webst02.is.autonavi.com/appmaptile?style=6&x={x}&y={y}&z={z}",
      minimumLevel: 3,
      maximumLevel: 18,
    })
    // 先加载地图，再加载标记，
    gddtLayer_base.value = viewer.value.imageryLayers.addImageryProvider(Layer_dt_add);
    gddtLayer_name.value = viewer.value.imageryLayers.addImageryProvider(Layer_lm_add);
  } else {
    // viewer.value.imageryLayers.removeAll()

    var layerIndex = viewer.value.imageryLayers.indexOf(gddtLayer_base.value);
    if (layerIndex !== -1) {
      viewer.value.imageryLayers.remove(viewer.value.imageryLayers.get(layerIndex));
    }

    var layerIndex = viewer.value.imageryLayers.indexOf(gddtLayer_name.value);
    if (layerIndex !== -1) {
      viewer.value.imageryLayers.remove(viewer.value.imageryLayers.get(layerIndex));
    }

  }
}

// 添加点
const mark_show = ref(false)
const handler = ref(null)
const mark_add = () => {
  if (mark_show.value) {
    // 直接写坐标的方式添加。
    viewer.value.entities.add({
      id: "point1",
      position: Cesium.Cartesian3.fromDegrees(115.464806, 38.873891, 0),
      point: {
        pixelSize: 10,
        color: Cesium.Color.RED
      }
    })

    // 通过json添加
    const point2 = [
      {
        "id": "document",
        "name": "point_name",
        "version": "1.0"

      },
      {
        "id": "black",
        "position": { "cartographicDegrees": [115.464806, 38.86300, 0] },
        "point": {
          "color": { "rgba": [0, 0, 0, 255] },
          "pixelSize": 100,
        }
      },
      {
        "id": "green",
        "position": { "cartographicDegrees": [115.454006, 38.87300, 0] },
        "point": {
          "color": { "rgba": [0, 255, 0, 255] },
          "pixelSize": 100,
        }
      },
      {
        "id": "red",
        "position": { "cartographicDegrees": [115.450006, 38.87300, 0] },
        "point": {
          "color": { "rgba": [255, 0, 0, 255] },
          "pixelSize": 100,
        }
      }
    ]

    Cesium.CzmlDataSource.load(point2).then(dataSource => {
      viewer.value.dataSources.add(dataSource);
      const dataSourceList = viewer.value.dataSources.getByName("point_name")
    });



    // 创建单击事件
    handler.value = new Cesium.ScreenSpaceEventHandler(viewer.value.canvas)
    handler.value.setInputAction((event) => {
      var pickedObject = viewer.value.scene.pick(event.position);
      if (Cesium.defined(pickedObject) && pickedObject.id._id === "point1") {
        alert('添加了弹出框')
      }
    }, Cesium.ScreenSpaceEventType.LEFT_CLICK)

  } else {

    // 移除点
    viewer.value.entities.removeById("point1")


    // 移除数据集合
    const dataSourceList = viewer.value.dataSources.getByName("point_name");
    for (let i = 0; i < dataSourceList.length; i++) {
      const dataSource = dataSourceList[i];
      viewer.value.dataSources.remove(dataSource);
    }
  }


}

// 添加边界
const bj_show = ref(false)
const bj_add = () => {
  if (bj_show.value) {
    Cesium.CzmlDataSource.load('/geojson/bj.json').then((dataSource) => {
      viewer.value.dataSources.add(dataSource);
    });

    viewer.value.camera.flyTo({
      destination: Cesium.Cartesian3.fromDegrees(115.44970238826704, 38.927947243363036, 153020.27236867257)
    })

  } else {
    const dataSourceList = viewer.value.dataSources.getByName('bj');
    for (let i = 0; i < dataSourceList.length; i++) {
      const dataSource = dataSourceList[i];
      viewer.value.dataSources.remove(dataSource)
    }
  }

}

</script>



<style scoped>
@import url(cesium/Build/Cesium/Widgets/widgets.css);

.mainbody{
  width:100%;
  height: 100%;
  position: fixed;
  display: flex;
  justify-content: center;
}

#cesiumContainer{
  width:100%;
  height: 100%;
}
.mid-botton {
  position: absolute;
  bottom: 50px;
  text-align: center;

}
.title{
  top: 1px;
  left:1px;
  width: 100vw;
  height:8vh;
  justify-content: center;
  background-color: rgba(0,0,0,0.3);
  position: fixed;


}
.leftContent {
  position: fixed;
  top: 80px;
  left: 1px;
  width: 20vw;
  height: 85vh;
  background-color: rgba(0, 0, 125, 0.3);
}

.rightContent {
  position: fixed;
  top: 80px;
  right: 1px;
  width: 20vw;
  height: 85vh;
  background-color: rgba(0, 0, 125, 0.3);
}

</style>
