<style lang="stylus" rel="stylesheet/stylus">
  @import 'common/stylus/index'
  .tab
    display:flex
    width:100%
    height:40px
    line-height:40px
    border-1px(rgba(7,17,27,0.1))
    .tab-item
      flex:1
      text-align:center
      a
        display:block
        font-size:14px
        color rgb(77,85,93)
        &.active
          font-size 14px
          color rgb(240,20,20)
</style>

<template>
<div>
  <v-header :seller="seller"></v-header>
  <div class="tab">
    <div class="tab-item" @click="gotoMenu()">
      <a :class="{ active: isMenuActive }">商品</a>
    </div>
    <div class="tab-item" @click="gotoStore()">
      <a :class="{ active: isStoreActive }">商家</a>
    </div>
  </div>
  <keep-alive>
    <router-view :seller="seller"></router-view>
  </keep-alive>
</div>

</template>

<script>
import header from 'components/header/header'
import {getAllStores} from 'api/store'

const ERR_OK = 0

export default {
  data() {
    return {
      seller: {},
      isMenuActive:true,
      isStoreActive:false,
      storeId:this.$route.params.storeId,
      tableId:this.$route.params.tableId
    }
  },
  created() {
    var storeId = this.$data.storeId;
    getAllStores({storeId:storeId}).then(response => {
      this.seller = response.data[0];
      this.seller.avatar = !this.seller.storeLogo?'/image/companyLogo.jpeg':'/api/store/edit/storeLogoDown/'
                     + storeId +'/img.jpg?l=' +　new Date().getTime();
    })
  },
  methods: {
    gotoStore() {
      var storeId = this.$data.storeId,
          tableId = this.$data.tableId;
      this.$data.isMenuActive = false;
      this.$data.isStoreActive = true;
      this.$router.push({
        path:'/seller/' +　storeId + '/' + tableId
      });
    },
    gotoMenu() {
      var storeId = this.$data.storeId,
          tableId = this.$data.tableId;
      this.$data.isMenuActive = true;
      this.$data.isStoreActive = false;
      this.$router.push({
        path:'/goods/' +　storeId + '/' + tableId
      });
    }
  },
  components: {
    'v-header': header
  }
}

</script>
