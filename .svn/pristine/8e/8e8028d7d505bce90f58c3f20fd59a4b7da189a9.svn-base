import Vue from 'vue'
import App from './App'
import { fetch, post, file } from './utils/utils.js'

//定义全局变量
Vue.prototype.$fetch = fetch;
Vue.prototype.$post = post;
Vue.prototype.$file = file;
Vue.config.productionTip = false

// Vue.prototype.$reUrl = "https://test.callas2.caih.com" //测试
Vue.prototype.$reUrl = "https://api.caih.com/adminDoc" //生产
App.mpType = 'app'

const app = new Vue({
    ...App
})
app.$mount()