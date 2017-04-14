<template>
  <div id="app">
  	<v-header :seller="seller"></v-header>
    <div class="tab">
    	<router-link class="tab_item" to="/goods" >商品</router-link>
    	<router-link class="tab_item" to="/ratings">评论</router-link>
    	<router-link class="tab_item" to="/seller">商家</router-link>
    </div>
    <router-view :seller="seller"></router-view>
  </div>
</template>

<script>
import header from './components/header/header.vue';

var ERR_OK = 0;

export default {
  name: 'app',
  data() {
    return {
      seller: {}
    };
  },
  created() {
  	this.$http.get('/api/seller').then((response) => {
  		response = response.body;
  		if (response.errno === ERR_OK) {
  			this.seller = response.data;
  			console.log(this.seller);
  		}
  	});
  },
  components: {
    'v-header': header
  }
};
</script>

<style>
#app .tab {
	display: flex;
	width: 100%;
	height: 40px;
	line-height: 40px;
	border-bottom: 1px solid rgba(7,17,27,0.1);
}
#app .tab .tab_item {
	flex: 1;
	text-align: center;
	display: block;
	text-decoration: none;
	font-size: 14px;
	color: rgb(77,85,93);
}
#app .tab .active{
	color: rgb(240,20,20);
}
</style>
