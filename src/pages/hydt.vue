<template>
  <div class="hydtcontainer"
       v-loading="loading"
       element-loading-text="拼命加载中"
       element-loading-spinner="el-icon-loading"
       element-loading-background="rgba(0, 0, 0, 0.6)"
  >
    <div class="topInfo" @click="goDetail(listData[0])">
      <img src="../assets/test.jpg" alt="" class="topImg">
      <p class="topTitle">{{listData&&listData[0]&&listData[0].newsTitle}}</p>
    </div>
    <div class="hydtlistContainer" v-for="(item,index) in listData" :key="item.id" v-if="index!==0" @click="goDetail(item)">
      <p class="hydttitle">{{item.newsTitle}}</p>
      <img src="../assets/test2.jpg" alt="" class="listImg">
    </div>
  </div>
</template>
<script>
  export default {
    name: 'hydt',
    data () {
      return {
        listData: [],
        loading: true
      }
    },
    mounted () {

    },
    methods: {
      getList() {
        fetch('http://115.236.161.140:8090/wechat/getHydtNews')
          .then((response) => {
            return response.json();
          })
          .then((myJson) => {
            this.listData = myJson.data
            this.loading = false
          });
      },
      goDetail(item){
        this.$store.dispatch('gloabal/getZsyzDetail',{
          data:item
        })
        this.$router.push(`/zsyzxq/${item.id}`)
      }
    },
    created () {
      this.getList()
    },
  }
</script>

<style lang="scss">
  .hydtcontainer{
    height: 100%;
  }
  .topInfo{
    position: relative;
  }
  .topTitle{
    position: absolute;
    bottom: 20px;
    margin-left: 30px;
    font-size: 24px;
    width: 60%;
    overflow: hidden;
    white-space: nowrap;
    text-overflow: ellipsis
  }
  .topImg{
    height: 200px;
    width: 100%;
  }
  .hydttitle{
    /*margin-right: 20px;*/
    width: 70%;
    overflow: hidden;
    white-space: nowrap;
    text-overflow: ellipsis;
    border-bottom: 1px solid #efeff4;
    line-height: 100px
  }
  .hydtlistContainer{
    display: flex;
    justify-content: space-between;
    padding: 10px 0;
    margin: 20px 30px;
    align-items: center;
    font-size: 24px;
    cursor: pointer;
    height: 60px;
    align-items: center;
  }

  .listImg{
    height: 80px;
    width: 28%;
    border: 1px solid #ccc;
    background: antiquewhite;
  }
  .el-icon-loading{
    font-size: 60px;
  }
  .el-loading-spinner .el-loading-text{
    font-size: 24px;
  }
</style>
