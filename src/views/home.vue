<template>
    <div class="container">
      <div class="container_main" >
        <Main :member="member" ref="main" @productSearch="productSearch"></Main>
      </div>
      <div class="container_aside">
        <Aside :member="member" @intoshowCart="intoshowCart" ref="aside"></Aside>
      </div>
      <div class="clear"></div>
    </div>
</template>

<script>
import Main from '@/components/main.vue'
import Aside from '@/components/aside.vue'
export default {
  components: {
    Main,
    Aside
  },
  // props: ['member'],
  data () {
    return {
      member: {
        'fixationParameter': {
          'memberid': '521',
          'memtype': 0,
          'channeltype': 'A',
          'malorgid': '597',
          'outorgid': '854'
        }
      }
    }
  },
  methods: {
    intoshowCart () {
      this.$refs.main.intoshowCart()
    },
    productSearch () {
      this.$refs.aside.productSearch()
    }
  }
}
</script>

<style>
.container {
    height: 1000px;
    margin:0;
    padding:0;
    /* border:1px solid red; */
  }
  .container .container_aside {
    width: 43%;
    float: left;
    background-color: #fff;
    color: #333;
    text-align: center;
    /* height: 700px; */
    margin-bottom: 10px;
    overflow-y: auto;
    overflow-x: hidden;
    /* border:1px solid red; */
  }
  .container .container_main {
    /* height: 700px; */
    width: 53%;
    float: right;
    background-color: #FFF;
    color: #333;
    margin:0;
    padding:0;
    /* overflow-y: auto; */
    /* overflow-x: hidden; */
    /* border:1px solid #aaa; */
  }
</style>
