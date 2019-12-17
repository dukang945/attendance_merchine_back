import Vue from 'vue'
import Router from 'vue-router'
import main from '@/components/common/main'
import login from '@/components/common/login'
import staff from '@/components/staff'
import attendance from '@/components/attendance'
import anomaly from '@/components/anomaly'




Vue.use(Router)

export default new Router({
  routes: [
    {
      path: '/',
      component: main,
      name:'首页',
      redirect:'/staff',
      children:[
        {
        path: '/staff',
        name: '员工管理',
        component: staff
      },
        {
        path: '/attendance',
        name: '考勤管理',
        component: attendance
      },
        {
        path: '/anomaly',
        name: '异常管理',
        component: anomaly
      },
    ]
    },
    {
      path: '/login',
      component: login,
      name:'登录',
    }
  ]
})
