<template>
  <div>
    <headerTop/>
    <div class="mainComtant">
        <div v-for="(item,index) in content" :key="index" class="imgAndT">
          <img :src="item.pic" class="img">
          <p class="title">{{item.title}}</p>
        </div>
    </div>
    <p class="nomore">没有更多数据了</p>
  </div>
</template>

<script>
  import headerTop from '@/components/header'
  import axios from 'axios'

  export default {
    name: "index",
    components: {
      headerTop
    },
    data(){
      return{
        content:[]
      }
    },
    methods:{

      getData(){
        const titleNew='随时随地拍'
        this.$store.commit('CHANGE-HEADERTEXT',titleNew)
        axios.get('http://211.67.177.56:8080/hhdj/news/newsList.do?page=1&rows=10&type=7').then(res=>{
          // console.log(res)
          this.content = res.data.rows
        })
      }
    },
    created(){
      this.getData()
    },
    mounted(){
      document.addEventListener('plusready', function() {
        var webview = plus.webview.currentWebview();
        plus.key.addEventListener('backbutton', function() {
          webview.canBack(function(e) {
            if(e.canBack) {
              webview.back();
            } else {
              var first = null;
              plus.key.addEventListener('backbutton', function() {
                //首次按键，提示‘再按一次退出应用’
                if (!first) {
                  first = new Date().getTime();
                  this.$toast.success('再按一次退出应用');
                  console.log('再按一次退出应用');
                  setTimeout(function() {
                    first = null;
                  }, 1000);
                } else {
                  if (new Date().getTime() - first < 1500) {
                    plus.runtime.quit();
                  }
                }
              }, false);
            }
          })
        });
      });
    }
  }
</script>

<style scoped lang="scss">
.mainComtant{
  margin-top: 45px;
  display: flex;
  flex-wrap: wrap;
  .imgAndT{
    width: 40%;
    height: 40%;
    padding: 5%;
    .img{
      width: 100%;
      height: 80%;
    }
    .title{
      overflow: hidden;
      text-overflow: ellipsis;
      display: -webkit-box;
      -webkit-line-clamp: 2;
      -webkit-box-orient: vertical;
      font-size: 14px;
      color: #666;
    }
  }
}
  .nomore{
    font-size: 14px;
    color: #666;
    text-align: center;
    padding: 10px 0;
  }
</style>
