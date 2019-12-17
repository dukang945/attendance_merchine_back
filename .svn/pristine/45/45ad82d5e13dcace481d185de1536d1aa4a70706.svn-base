// The Vue build version to load with the `import` command
// (runtime-only or standalone) has been set in webpack.base.conf with an alias.
import Vue from 'vue'
import App from './App'
import router from './router'
import ElementUI from 'element-ui'
import 'element-ui/lib/theme-chalk/index.css'
import axios from 'axios'
import qs from 'qs'
import './assets/icon/iconfont.css'
import Icon from 'vue-svg-icon/Icon.vue';
Vue.component('icon', Icon);
//兼容ie 的promise 处理
import promise from 'es6-promise'
promise.polyfill();

Vue.prototype.$axios = axios;
// Vue.prototype.$http = axios;
Vue.prototype.$qs = qs;
Vue.use(ElementUI);
// 引用高德地图组件
import VueAMap from 'vue-amap';

Vue.use(VueAMap);
VueAMap.initAMapApiLoader({
  key: 'bc6e9e39232439329db4ef928be87ac0',
  plugin: ['AMap.Scale', 'AMap.OverView', 'AMap.ToolBar', 'AMap.MapType','Geocoder'],
  v: '1.4.4',
});
Vue.config.productionTip = false
// 路由拦截
router.beforeEach((to, from, next) => {
  const role = sessionStorage.getItem('token');
  if (!role && to.path !== '/login') {
    next('/login');
  } else if (to.meta.permission) {
    // 如果是管理员权限则可进入
    role === 'admin' ? next() : next('/403');
  } else {
    // 简单的判断IE10及以下不进入富文本编辑器，该组件不兼容
    if (navigator.userAgent.indexOf('MSIE') > -1 && to.path === '/editor') {
      Vue.prototype.$alert('vue-quill-editor组件不兼容IE10及以下浏览器，请使用更高版本的浏览器查看', '浏览器不兼容通知', {
        confirmButtonText: '确定'
      });
    } else {
      next();
    }
  }
})
// 请求头添加token
// axios.interceptors.request.use(
//   config => {
//     if (sessionStorage.getItem('token') && config.url.split('?')[0] !== 'https://restapi.amap.com/v3/geocode/geo') {
//       console.log(config.url.split('?')[0])
//       config.headers.Authorization = sessionStorage.getItem('token');
//     }else if(config.url.split('?')[0] == 'https://restapi.amap.com/v3/geocode/geo'){
//       // config.headers.Authorization = ''
//       console.log(1)
//     }

//     return config;
//   },
//   error => {
//     return Promise.reject(error);
//   });
// 响应拦截
axios.interceptors.response.use((res) => {
  if (res.data.code == 504) {
    console.log(res)
    // this.$message.warning('token过期，请重新登录！')
    router.push("/login")

    //其余错误状态处理    
  }
  return res
}, function (error) {
  return Promise.reject(error);
});
/* eslint-disable no-new */
new Vue({
  el: '#app',
  router,
  components: {
    App
  },
  template: '<App/>'
})
