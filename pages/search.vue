<template>
    <div class="page">
        <!--1、创建一个search文件，自动帮我们添加了路由，让search这个路由配置了search.vue文件，即做了映射-->
        <!--2、把search.vue文件作为配置的入口文件-->
        Page is search
        <ul>
            <li v-for="(item,index) in $store.state.city.list" :key="index">{{item}}</li>
        </ul>
        <ul>
            <li v-for="(iteml, index) in list" :key="index">{{iteml}}</li>
        </ul>
    </div>
</template>

<script>
import axios from 'axios'
export default {
  layout: 'search',
  data() {
    return {
      list: []
    }
  },
  // mounted这个过程 在服务器端渲染（即开启ssr的过程中，只有created在服务器端渲染）是不执行这个过程的，只有在浏览器端被执行，所以下面的数据只在浏览器中发生了效果，在源码的模板中找不到以下数据
  // async mounted() {
    // 服务端没有在页面渲染的时候把数据扔进去，是在浏览器端发出的请求获得的这个数据，最后渲染到页面上
  //   let self = this
  //   let {status, data: {list}} = await axios.get('/city/list')
  //   if (status === 200) {
  //     self.list = list
  //   }
  // }
  // ssr原理：服务器端把编译好的内容下发你(源码中有数据)，把异步获取的数据也同时扔给浏览器端，浏览器的虚拟dom利用数据进行编译 与服务器编译好的内容做对比，为dom注册事件，发生交互。
  //  服务器端在页面渲染的时候把数据扔进去
  async asyncData() {
    let {status, data: {list}} = await axios.get('http://localhost:3000/city/list')
    if (status === 200) {
      return {
        list
      }
    }
  }
  // fetch()、asyncData()都可以获取异步数据，此处使用fetch无效，因为fetch主要用来处理vuex相关的事情，asyncData()主要用来处理组件的数据
  // async fetch() {
  //   let {status, data: {list}} = await axios.get('http://localhost:3000/city/list')
  //   if (status === 200) {
  //     return {
  //       list
  //     }
  //   }
  // }

}
</script>

<style lang="css">

</style>
