<template>
  <div>
    <headerTop/>
    <img src="./timg.jpg" class="img">
    <div class="fiveLink">
      <div @click="handleToList" class="linkItem">
        <img src="./icon1.png" class="imgItem">
        <p class="title">政治学习</p>
      </div>
      <router-link to="/home/study/thinkingReport" class="linkItem">
        <img src="./icon2.png" class="imgItem">
        <p class="title">思想汇报</p>
      </router-link>
      <div @click="hangClickToTwo" class="linkItem">
        <img src="./icon3.png" class="imgItem">
        <p class="title">心得总结</p>
      </div>
      <div  @click="commentFor" class="linkItem">
        <img src="./icon4.png" class="imgItem">
        <p class="title">民主评议</p>
      </div>
      <div @click="ToMap" class="linkItem">
        <img src="./icon5.png" class="imgItem">
        <p class="title">流动党员找组织</p>
      </div>
    </div>
  </div>
</template>

<script>
  import headerTop from '@/components/header'

  export default {
    name: "index",
    components: {
      headerTop
    },
    data() {
      return {
        title: '掌上组织生活',
        userName:{},
      }
    },
    methods: {
      changeTitle() {
        this.$store.commit('CHANGE-HEADERTEXT', this.title)
        this.userName = JSON.parse(sessionStorage.getItem('user'))
      },
      hangClickToTwo(){
        // this.$router.push('/home/study/summarize')
        if(this.userName){
          this.$router.push('/home/study/summarize')
        }else {
          this.$router.push('/login')
        }
      },
      handleToList(){
        const title = "政治学习"
        this.$store.commit('CHANGE-HEADERTEXT',title)
        this.$store.commit('CHANGGE-LISTTABLE-TITLE',title)
        this.$router.push('/home/listShow')
      },
      ToMap(){
        const title = "流动党员找组织"
        this.$store.commit('CHANGE-HEADERTEXT',title)
        this.$router.push('/home/study/Bmap')
      },
      commentFor(){
        // this.$router.push('/home/study/commentFor')
        if(this.userName){
          this.$router.push('/home/study/commentFor')
        }else {
          this.$router.push('/login')
        }
      }

    },
    created() {
      this.changeTitle()
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
  .img {
    margin-top: 45px;
    width: 7.5rem;
    height: 186px;
    display: block;
  }
  .fiveLink{
    display: flex;
    flex-wrap: wrap;
    align-items: center;
    .linkItem{
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      width: 2.5rem;
      height: 145px;
      .imgItem{
        height: 85px;
        width: 85px;
      }
      .title{
        margin-top: 10px;
        font-size: 14px;
        color: #666;
      }
    }
  }
</style>
