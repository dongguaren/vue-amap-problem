初学 vue，最近做个高德地图相关的应用练练手。找了半天找到 [vue-amap](https://elemefe.github.io/vue-amap/#/)，用起来还不错。今天遇到了一个问题，谷歌无果。
***
####1. 产生问题的环境的复现

```
vue init webpack test
cd test
npm install vue-amap --save
npm run dev
```
在 test 文件夹使用 vue-cli 初始化 vue 项目并将将 vue-amap 添加至开发依赖。
在使用 vue-cli 初始化中，使用的配置如下：
![ vue-cli 初始化配置](https://upload-images.jianshu.io/upload_images/6742807-c7fb0fdf00bf133d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

之后按照[基础-地图](https://elemefe.github.io/vue-amap/#/zh-cn/base/amap)的说明，在网页上显示一个地图。说明中的代码示例需要改动一点才能正常运行。
![基础地图](https://upload-images.jianshu.io/upload_images/6742807-3736b58c2d64f3e0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 2.问题
以下是对应 vue 文件的 script 部分。

```
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
            console.log(o.getCenter())
            console.log(this.$refs.map.$$getInstance())
            o.getCity(result => {
              console.log(result)
            });

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
```
我发现如果我在“数据一”的位置声明 :
```
testData1:new AMap.LngLat(114.360664, 30.528582),
```

Chrom 报错：
![报错信息](https://upload-images.jianshu.io/upload_images/6742807-c7f85eaad80f1c39.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
但是在其他位置（数据2-4）声明这个语句就不会报错。
对此我觉得很奇怪。
难道在数据1和数据2-4对应的 context 不一样吗？


