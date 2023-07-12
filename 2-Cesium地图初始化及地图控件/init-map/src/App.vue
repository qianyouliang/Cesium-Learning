<template>
  <div id="cesiumContainer"></div>
</template>

<script setup>
import * as Cesium from "cesium";
import { onMounted, getCurrentInstance } from "vue";

const { appContext } = getCurrentInstance();
const global = appContext.config.globalProperties;

//必须在挂载后引入cesium地图组件
onMounted(() => {
  Cesium.Ion.defaultAccessToken =
    "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiI5ZGJlOTBmMi0xYjNmLTRhMzYtOTlhMy0zNGIwOGUyNGFkYTUiLCJpZCI6MTE1MTg3LCJpYXQiOjE2Njg2NzM2NTB9.kQwPYh1h1KTzNcwTFkoW36f8DpbDGhdDhrpzHHFUfTk";
  const esri = new Cesium.ArcGisMapServerImageryProvider({
    url: "https://services.arcgisonline.com/ArcGIS/rest/services/World_Imagery/MapServer",
    enablePickFeatures: false,
  });
  const viewer = new Cesium.Viewer("cesiumContainer", {
    imageryProvider: esri, //默认的谷歌地图影像  影像图层 ImageryLayer,这里是esri的影像图层

    //地形图层TerrainProvider
    terrainProvider: Cesium.createWorldTerrain({
      requestWaterMask: true, //水面特效
    }), //viewer是所有api的入口

    //图层控件显隐控制
    timeline: false, //隐藏时间轴
    animation: false, //隐藏动画控制器
    geocoder: false, //隐藏地名查找控制器
    homeButton: false, //隐藏Home按钮
    sceneModePicker: false, //隐藏投影方式控制器
    baseLayerPicker: false, //隐藏图层选择控制器
    navigationHelpButton: false, //隐藏帮助按钮
    fullscreenButton: false, //隐藏全屏按钮
  });
  global.$viewer = viewer;
});
</script>

<style scoped>
#cesiumContainer {
  width: 100vw;
  height: 100vh;
  overflow: hidden;
}
:deep(.cesium-viewer-bottom) {
  display: none;
}
</style>
