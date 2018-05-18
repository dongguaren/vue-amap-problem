<template>
  <div class="amap-page-container">
    <el-amap ref="map" vid="amapDemo" :amap-manager="amapManager" :center="center" :zoom="zoom" :plugin="plugin" :events="events" class="amap-demo">
    </el-amap>

    <div class="toolbar">
      <button @click="getMap()">get map</button>
    </div>
  </div>
</template>

<style>
  .amap-page-container{
    height: 500px;
    width: 500px;
  }
  .amap-demo {
    height: 500px;
    width: 500px;
  }
</style>

<script>
  // NPM 方式
  import { AMapManager } from 'vue-amap';
  export default {
    data() {
      return {
        amapManager:new AMapManager(),
        zoom: 12,
        /**
         *    数据一
         *    报错
         */
        // testData1:new AMap.LngLat(114.360664, 30.528582),
        center: [121.59996, 31.197646],
        events: {
          init: (o) => {
            console.log(o.getCenter());
            console.log(this.$refs.map.$$getInstance());
            o.getCity(result => {
              console.log(result)
            });
            console.log(this);

            /**
             * 数据二
             * 不报错
             */
              // let testData2 =  new AMap.LngLat(114.360664, 30.528582);
            console.log(this);
          },
          'moveend': () => {

          },
          'zoomchange': () => {
          },
          'click': (e) => {
            console.log("map click");
            /**
             * 数据三
             * 不报错
             * @type {toLngLat|a}
             */
            let testData3 =  new AMap.LngLat(114.360664, 30.528582);
          }
        },
        plugin: ['ToolBar', {
          pName: 'MapType',
          defaultType: 0,
          events: {
            init(o) {
              console.log(o);
            }
          }
        }]
      };
    },

    methods: {
      getMap() {
        // amap vue component
        console.log(this.amapManager._componentMap);
        /**
         * 数据四
         * 不报错
         * @type {toLngLat|a}
         */
        let testData4 =  new AMap.LngLat(114.360664, 30.528582);
      }
    }
  };
</script>
