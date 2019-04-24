
<template>
  <div class="asider">
    <!-- 查询 -->
    <div class="query">
      <table>
        <!-- <tr>
          <td class="right" width="8%">
            <label class="b-lable">一级分类</label>
          </td>
          <td class="left" width="8%">
            <span class="combo" style="width: 158px; height: 20px;">
              <span class="textbox-addon" style="right: 0px;width: 18px; height: 20px;">
                <a class="textbox-icon combo-arrow" icon-index="0" tabindex="-1"></a>
              </span>
            </span>
          </td>
          <td class="right" width="8%"><label class="b-label">分类明细</label></td>
          <td class="left" width="8%">
            <span class="b-span">
              <input class="b-txt combotree-f combo-f textbox-f" id="p_catatree" textboxname="p_catatree" style="display: none;" comboname="p_catatree">
              <span class="textbox combo" style="width: 158px; height: 20px;">
                <span class="textbox-addon textbox-addon-right" style="right: 0px;">
                  <a class="textbox-icon icon-combo_clear" icon-index="0" tabindex="-1" style="width: 18px; height: 20px;"></a>
                  <a class="textbox-icon combo-arrow" icon-index="1" tabindex="-1" style="width: 18px; height: 20px;"></a>
                </span>
                <input type="text" class="textbox-text validatebox-text textbox-prompt" autocomplete="off" readonly="readonly" placeholder="" style="margin-left: 0px; margin-right: 36px; padding-top: 3px; padding-bottom: 3px; width: 120px;">
              </span>
            </span>
          </td>
        </tr> -->
        <tr>
          <td class="right" width="8%">
            <label class="b-label">商品查询</label>
          </td>
          <td class="left" colspan="4" width="8%">
            <span class="textbox" style="width: 158px; height: 20px;">
              <!-- <input type="text" class="textbox-text" ref="searchval" autocomplete="off" placeholder="" style="margin-left: 0px; margin-right: 0px; padding-top: 3px; padding-bottom: 3px; width: 156px;"> -->
              <el-input placeholder="输入关键字进行过滤" type="text" class="textbox-text" ref="searchval" autocomplete="off" style="margin-left: 0px; margin-right: 0px; padding-top: 3px; padding-bottom: 3px; width: 156px;"></el-input>
            </span>
          </td>
          <td>
            <el-button @click="productSearch()">查询</el-button>
          </td>
          <td>
            <el-button @click="allopen">全部展开</el-button>
          </td>
        </tr>
      </table>
    </div>
    <!-- 展示数据 -->
    <div class="show_data">
      <div class="box" style="text-align:left">
        <div style="width:53px;text-align:center">商品ID</div>
        <div style="width:240px;text-align:center">商品名称</div>
        <div style="width:38px">零售价</div>
        <div style="width:38px">促销价</div>
        <div style="width:38px">会员价</div>
        <div style="width:50px;border-right:none;padding-left:20px" class="addcart">加购物车</div>
      </div>
      <el-collapse v-model="activeNames" >
        <div v-for="(dat,ind) in datasfoud" :key="'dat.id'+ind" class="box" style="position:relative">
          <div v-if="dat.groupPromoList">
            <el-collapse-item :name="ind">
              <template slot="title">
                <div style="width:40px;padding-left:10px">{{dat.productvarid}}</div>
                <div style="width:236px">{{dat.productvarname}}</div>
                <div style="width:30px">{{dat.retailprice}}</div>
                <div style="width:30px">{{dat.promoprice}}</div>
                <div style="width:30px">{{dat.memberprice}}</div>
                <div style="width:108px;border-right:none;">
                  <input type="button" class="addshop" @click="addshopcart(dat.operparam,ind, 1)"  value="+" :key="'x1'+ind">
                  <input type="button" class="addshop" @click="addshopcart(dat.operparam,ind,-1)"  value="-" :key="'x2'+ind">
                  <input type="text" class="addshop1" v-model="dat.cartqty">
                </div>
              </template>
              <div v-for="(data,index) in dat.groupPromoList" :key="'care'+index" style="width:100%;border-top:1px solid #aaa">
                <div style="width:40px">{{data.id}}</div>
                <div style="width:250px">{{data.promottext}}</div>
                <div style="width:110px">
                  <input type="button" class="addshop" @click="addshopcart(data.operparam, ind, 1)"  value="+" :key="'x1'+ind">
                  <input type="button" class="addshop" @click="addshopcart(data.operparam, ind,-1)"  value="-" :key="'x2'+ind">
                  <input type="text" class="addshop1" v-model="data.qty">
                </div>
              </div>
            </el-collapse-item>
          </div>
          <div v-else>
            <template>
              <div style="width:40px;padding-left:0px">{{dat.productvarid}}</div>
              <div style="width:240px">{{dat.productvarname}}</div>
              <div style="width:30px">{{dat.retailprice}}</div>
              <div style="width:30px">{{dat.promoprice}}</div>
              <div style="width:30px">{{dat.memberprice}}</div>
              <div style="width:110px;border-right:none">
                <input type="button" class="addshop" @click="addshopcart(dat.operparam,ind, 1)"  value="+" :key="'x1'+ind">
                <input type="button" class="addshop" @click="addshopcart(dat.operparam,ind,-1)"  value="-" :key="'x2'+ind">
                <input type="text" class="addshop1" v-model="dat.cartqty">
              </div>
            </template>
              <div v-for="(data,ind) in dat.groupPromoList" :key="'care'+ind" style="width:100%;border-top:1px solid #aaa">
                <div style="width:40px">{{data.id}}</div>
                <div style="width:250px">{{data.promottext}}</div>
                <div style="width:110px">
                  <input type="button" class="addshop" @click="addshopcart(data.operparam, ind, 1)"  value="+" :key="'x1'+ind">
                  <input type="button" class="addshop" @click="addshopcart(data.operparam, ind,-1)"  value="-" :key="'x2'+ind">
                  <input type="text" class="addshop1" v-model="data.qty">
                </div>
              </div>
          </div>
        </div>
      </el-collapse>
      <!-- <div v-for="(dat,ind) in datasfoud" :key="'car'+ind" class="box" style="text-align:left;padding-left:10px;overflow:hidden">
        <div style="width:40px">{{dat.productvarid}}</div>
        <div style="width:250px" class="proname">{{dat.productvarname}}</div>
        <div style="width:25px">{{dat.retailprice}}</div>
        <div style="width:25px">{{dat.promoprice}}</div>
        <div style="width:25px">{{dat.memberprice}}</div>
        <div style="width:110px">
          <input type="button" class="addshop" @click="addshopcart(dat.operparam,ind, 1)"  value="+" :key="'x1'+ind">
          <input type="button" class="addshop" @click="addshopcart(dat.operparam,ind,-1)"  value="-" :key="'x2'+ind">
          <input type="text" class="addshop1" v-model="dat.cartqty">
        </div>
        <div v-if="dat.groupPromoList" @click="displayed" style="width:20px;border-right:none">
          <i class="el-icon-arrow-right" v-show="classed"></i>
          <i class="el-icon-arrow-down" v-show="classed1"></i>
        </div>
        <div v-if="classed1 === true" v-for="(data,index) in dat.groupPromoList" :key="data.id+index" style="width:100%;border-top:1px solid gray;text-align:left">
          <div style="width:40px">{{data.id}}</div>
          <div style="width:250px">{{data.promottext}}</div>
          <div style="width:110px">
            <input type="button" class="addshop" @click="addshopcart(data.operparam, ind, 1)"  value="+" :key="'x1'+ind">
            <input type="button" class="addshop" @click="addshopcart(data.operparam, ind,-1)"  value="-" :key="'x2'+ind">
            <input type="text" class="addshop1" v-model="data.qty">
          </div>
        </div>
      </div> -->
    </div>
  </div>
</template>
<script>
export default {
  // aside 8003 main 8002
  props: ['member'],
  data () {
    return {
      val: '',
      datasfoud: [],
      groups: [],
      filterText: '',
      activeNames: []
    }
  },
  methods: {
    // 点击全部展开
    allopen () {
      this.activeNames = []
      for (var i = 0; i < this.datasfoud.length; i++) {
        if (this.datasfoud[i].groupPromoList) {
          this.activeNames.push(i.toString())
        }
      }
      console.log('this.activeNames=>', this.activeNames)
    },
    // 获取购物车商品数量
    getCartqty () {
      this.$http.post('/cart/getCartQty',
        {
          'channeltype': 'A',
          'gradeid': 10,
          'malorgid': 10214,
          'mempricefieldid': 0,
          'memtype': 1,
          'memberid': 521,
          'outorgid': 10218
        })
        .then(res => {
          if (res.status === 200) {
            for (var i = 0; i < this.datasfoud.length; i++) {
              this.datasfoud[i].cartqty = res.data.data[i].qty
              // console.log(res.data.data[i].qty)
            }
          }
        })
    },
    // 查找商品列表
    getAlldata () {
      let that = this
      this.$http.post('/app/productSearch', Object.assign({}, this.member))
        .then(res => {
          let { data, status } = res
          if (status === 200) {
            that.datasfoud = data.data.prolist
            // console.log('/app/productSearch=>', this.datasfoud)
            this.$emit('intoshowCart')
            // 调用getCartQty()方法再点击查询按钮时将购物车数量渲染到页面上
            // this.getCartqty()
          } else {
            that.$message.error(data.errorMsg)
          }
        })
    },
    // 得到查询数据
    productSearch () {
      let searchText = this.$refs.searchval.currentValue
      if (!searchText) { // 空串 ， undefined ， 0
        return this.getAlldata()
      } else {
        // 提交参数
        this.$http.post('/app/productSearch', Object.assign({}, this.member, {
          'keyword': searchText
        }))
          .then(res => {
            if (res.status === 200) {
              this.datasfoud = res.data.data.prolist
              // console.log('/app/productSearch=>', this.datasfoud)
              this.$emit('intoshowCart')
            }
          })
          .catch((err) => {
            console.log(err)
          })
      }
    },
    // 点击添加到购物车 更改加购物车后面数量
    addshopcart (operparam, index, num) {
      console.log('operparam=>', operparam)
      this.$http.post('/cart/dealCartQty', Object.assign({}, this.member, {
        'answersParameter': JSON.parse(operparam),
        'receptionParameter': {
          'qty': num,
          'opertype': 1
        }
      })).then(res => {
        if (res.status === 200) {
          console.log('addshopcart=>', res)
          let row = this.datasfoud[index]
          row.cartqty = res.data.data[0].qty
          this.datasfoud.splice(index, 1, row)
          this.productSearch()
          this.$emit('intoshowCart')
        } else {
          this.$message.error(res.data.errorMsg)
          this.productSearch()
          this.$emit('intoshowCart')
        }
      })
    }
    // addshopcarte (operparam, index, num) {
    //   this.$http.post('/cart/dealCartQty', Object.assign({}, this.member, {
    //     'answersParameter': JSON.parse(operparam),
    //     'receptionParameter': {
    //       'qty': num,
    //       'opertype': 1
    //     }
    //   })).then(res => {
    //     if (res.status === 200) {
    //       for (var i = 0; i < this.datasfoud.length; i++) {
    //         if (this.datasfoud[i].groupPromoList) {
    //           for (var j = 0; j < this.datasfoud[i].groupPromoList.length; i++) {
    //             for (var h = 0; h <= res.data.data.length; i++) {
    //               console.log('this.datasfoud[i].groupPromoList=>', this.datasfoud[i].groupPromoList)
    //               // console.log('this.datasfoud[i].groupPromoList=>', this.datasfoud[i].groupPromoList)
    //               this.datasfoud[i].groupPromoList = res.data.data[h].promotlist
    //             }
    //           }
    //         }
    //       }
    //       this.$emit('intoshowCart')
    //     } else {
    //       this.$message.error(res.data.errorMsg)
    //       this.$emit('intoshowCart')
    //     }
    //   })
    // }
  }
}
</script>

<style>
.asider {
  width: 100%;
  /* height: 1500px; */
  /* overflow: scroll; */
}
/* 查询样式 */
  .asider .query{
    /* height: 50px; */
    /* border:1px solid #aac5e7; */
  }
  .asider .b-label {
      height: 22px;
      line-height: 22px;
      vertical-align: middle;
      white-space: nowrap;
      width: 100%;
      padding-right: 5px;
  }
  .asider .combo {
    position: relative;
    border: 1px solid #95B8E7;
    border-radius: 5px;
    display: inline-block;
    margin: 0;
    padding: 0;
    overflow: hidden;
    vertical-align: middle;
  }
  .asider .textbox-addon {
    position: absolute;
    top: 0;
    background-color: #E0ECFF;
  }
  .asider .combo-arrow {
    width: 8px;
    height: 5px;
    background: url(//misc.360buyimg.com/user/cart/css/i/cart-icons-201811.png) -82px -3px;
    overflow: hidden;
    display: inline-block;
    vertical-align: middle;
    margin:7px 5px;
  }
  .asider .textbox-icon {
      text-decoration: none;
      outline-style: none;
  }
  .aside .b-txt {
    width: 160px;
    line-height: 22px;
    font-size: 12px;
    display: inline-block;
    vertical-align: middle;
  }
  .aside .textbox {
    position: relative;
    border: 1px solid #95B8E7;
    background-color: #fff;
    vertical-align: middle;
    display: inline-block;
    overflow: hidden;
    white-space: nowrap;
    margin: 0;
  }
/* 查询样式结束 */

/* 展示数据样式 */
  .show_data {
    border:1px solid #95B8E7;
    width: 99.7%;
    font-size:10px;
    margin-top: 10px;
  }
   .show_data .box{
     border-bottom: 2px solid #aaa;
     padding:0;
     margin:0;
     overflow: hidden;
   }
   .show_data .box div{
     /* border-right:1px solid #ccc; */
     background-color: #fff;
     display: inline-block;
     /* height: 48px; */
     line-height: 40px;
     /* overflow: hidden; */
   }
   .el-collapse-item__content {
     padding-bottom: 0px!important;
     line-height: 10px!important;
   }
  .show_data input {
    background-color: #fff;
    border:1px solid #aaa;
    height: 20px;
    line-height: 20px;
    font-size: 10px;
    display: inline-block;
  }
  .show_data .addshop:hover{
    border:1px solid #409EFF;
    color: #409EFF
  }
  .show_data .addshop{
    width: 40px;
    height: 27px;
    background-color: #ccc
  }
  .show_data .addshop1{
    width: 20px;
    border: none
  }
  .classa {
    display: inline-block;
  }
  .classb {
    display: inline-block
  }
/* 展示样式结束 */
</style>
