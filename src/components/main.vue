<template>
  <div id="shopcar">
    <header id="searchBar">
      <div id="header_row">
        <div class="s1">
          <el-checkbox v-model="checkeAll" @change="hadleCheckAllChange(checkeAll)"></el-checkbox>
          <label for class="goodcheack">全选</label>
        </div>
        <div class="s2">商品</div>
        <div class="s4">单价</div>
        <div class="s5">数量</div>
        <div class="s6">小计</div>
        <div class="s7">操作</div>
      </div>
      <div id="header_row" v-show="searchBarFixed" class="isFixed">
        <div class="s1">
          <el-checkbox v-model="checkeAll" @change="hadleCheckAllChange(checkeAll)"></el-checkbox>
          <label for class="goodcheack">全选</label>
        </div>
        <div class="s2">商品</div>
        <div class="s4">单价</div>
        <div class="s5">数量</div>
        <div class="s6">小计</div>
        <div class="s7">操作</div>
      </div>
    </header>
    <main>
      <template v-for="(data1, ind1) in datas.groups">
        <template v-for="(data, ind) in data1.elements">
          <!--线的判断开始-->
            <template v-if="data.eletype == '1'" >
              <template v-if="data.showtype == '1'">
                <Lineone :key="data.id" :data1="data1" :id="'line'+ind" ></Lineone>
              </template>
              <template v-else-if="data.showtype == '2'">
                <Linetwo :key="data.id" :data1="data1" :id="'line'+ind"></Linetwo>
              </template>
              <template v-else-if="data.showtype == '3'">
                <Linethree :key="data.id" :data1="data1" :id="'line'+ind"></Linethree>
              </template>
              <template v-else-if="data.showtype == '4'">
                <Linefour :key="data.id" :data1="data1" :id="'line'+ind"></Linefour>
              </template>
            </template>
          <!--线的判断结束-->
          <!-- 文本判断开始 -->
            <template v-else-if="data.eletype == '2'" ><!--文字-->
              <template v-if=" data.isgift== '0' ">
                <Textnormal @inputNumberChange="inputNumberChange" @delet_nom="delet_nom" @dealShowCartChoice="dealShowCartChoice" @groupchoice="groupchoice" @cartPromoPop="cartPromoPop" @handleClose="handleClose" @cartPromoPopz="cartPromoPopz" @handleClosez="handleClosez" @cartPromoPopj="cartPromoPopj" @handleClosej="handleClosej" :elements="data1.elements" :item="data" :data1="data1" :key="data.id" :id="'text'+ind"></Textnormal>
              </template>
              <template v-else-if="data.isgift == '1'" >
                  <Textgift @inputNumberChange="inputNumberChange" @delet_nom="delet_nom" @dealShowCartChoice="dealShowCartChoice" @groupchoice="groupchoice" @cartPromoPop="cartPromoPop"  @handleClose="handleClose" @cartPromoPopz="cartPromoPopz" @handleClosez="handleClosez" @cartPromoPopj="cartPromoPopj" @handleClosej="handleClosej" :elements="data1.elements" :item="data" :data1="data1" :key="data.id" :id="'text'+ind"></Textgift>
              </template>
            </template>
          <!-- 文本判断结束 -->
          <!--商品的判断开始-->
            <template v-else-if="data.eletype == '3'" >
              <template v-if="data.isgift == '0'">
                <Goodnomal  ref="good" @inputNumberChange="inputNumberChange" @getRadioVal="getRadioVal" @showtogglesure="showtogglesure" @delet_nom="delet_nom" @dealShowCartChoice="dealShowCartChoice" @groupchoice="groupchoice" :item="data" :data1="data1" :ind="ind" :ind1="ind1" :key="data.id" :id="'good'+ind"></Goodnomal>
              </template>
              <template v-else-if="data.isgift == '1'" >
                <Gift @inputNumberChange="inputNumberChange" @getRadioVal="getRadioVal" @showtogglesure="showtogglesure" @delet_nom="delet_nom" @dealShowCartChoice="dealShowCartChoice" @groupchoice="groupchoice" :item="data" :data1="data1" :ind="ind" :ind1="ind1" :key="data.id" :id="'good'+ind"></Gift>
              </template>
            </template>
          <!--商品的判断结束-->
        </template>
      </template>
    </main>
    <!-- 任选组合弹出框 -->
      <el-dialog
        title="任选组合"
        class="renxuan"
        :visible.sync="dialogVisible"
        width="50%">
        <div style="text-align: left;display: inline-block">{{this.tableData.text1}}</div>
        <p style="text-align: left;display: inline-block;margin-left:100px">
          应选商品<span class="red">{{this.tableData.selqty * this.tableData.groupqty+'件'}}</span>,
          已选<span class="red">{{this.poplist+'件'}}</span>
          <span v-if="this.poplist > (this.tableData.selqty * this.tableData.groupqty)">已超过<b class="red">{{this.poplist -(this.tableData.selqty * this.tableData.groupqty)+'件'}}</b></span>
          <span v-else-if="this.poplist < (this.tableData.selqty * this.tableData.groupqty)">还需选<b class="red">{{(this.tableData.selqty * this.tableData.groupqty)- this.poplist+'件'}}</b></span>
        </p>
        <table style="margin-top:10px">
          <tr style="height:30px">
            <td>商品名称</td>
            <td>商品价格</td>
            <td>商品属性</td>
            <td>商品数量</td>
          </tr>
          <tr style="height:30px" v-for="(data, index) in this.tableData.showCartProductInfoList" :key="index">
            <td v-text="data.productvarname"></td>
            <td v-text="0" style="text-align:center"></td>
            <td>
              <p v-if="data.fielditemname1 && data.fieldname1" v-text="data.fielditemname1+':'+ data.fieldname1"></p>
              <p v-else-if="data.fielditemname2 && data.fieldname2" v-text="data.fielditemname2+':'+ data.fieldname2"></p>
              <p v-else>暂无数据</p>
            </td>
            <td>
            <el-input-number v-model="data.qty" @change="handleChangePop" size="mini" :min="0"></el-input-number>
            </td>
          </tr>
        </table>
        <div style="width: 100%;margin-top:15px">
          <a>小计</a>
          <span style="margin-left: 70px">{{'单价'+'  '+'￥'+ this.tableData.groupprice}}</span>
          <div class="counter">
            <a @click="handle">-</a>
            <input type="text" v-model.lazy="this.tableData.groupqty">
            <a @click="add">+</a>
          </div>
          <span style="margin-left:145px" >金额 {{' ￥'+this.tableData.groupqty* this.tableData.groupprice+'元'}}</span>
        </div>
        <span slot="footer" class="dialog-footer">
          <el-button @click="dialogVisible = false">取 消</el-button>
          <el-button type="primary" @click="handleClose">确 定</el-button>
        </span>
      </el-dialog>
      <!-- 赠品组合弹出界面 -->
      <el-dialog
        :visible.sync="dialogVisible1"
        width="50%" class="zeng">
          <div style="text-align: left;border-bottom:1px solid #aaa ">
            <a v-if="this.tableData.grouptype === 'F'">分组加价购</a>
            <a v-else-if="this.tableData.grouptype === 'B'">分组赠品</a>
            <a style="margin-left: 70px" v-text="this.tableData.text1"></a>
            <a style="margin-left: 70px">{{this.tableData.maxmsg}}</a>
          </div>
          <table v-for="(data, ind) in this.tableData.grouplist" :key="ind" style=" width: 100%;margin-bottom:10px; border:none;border-bottom:2px solid #aaa;padding-bottom:20px">
            <tr v-for="(data1, ind1) in data.list" :key="ind1">
              <td style="width:50px;border-border:1px solid #eee">{{data1.imageurl}}</td>
              <td style="border-border:1px solid #eee">{{data1.productvarname}}</td>
              <td style="border-border:1px solid #eee">{{'￥'+data1.promoprice}}</td>
              <td style="border-border:1px solid #eee">{{data1.qty+'件'}}</td>
            </tr>
            <tr style="padding-bottom: 10px;">
                <td>小计</td>
                <td >{{'单价'+'  '+'￥'+ data.price}}</td>
                <td class="counter">
                  <el-input-number v-model="data.qty" @change="handleChangegift" size="mini" :min="0"></el-input-number>
                </td>
                <td>金额 {{' ￥'+data.price* data.qty+'元'}}</td>
            </tr>
          </table>
        <span slot="footer" class="dialog-footer">
            <el-button @click="dialogVisible2 = false">取 消</el-button>
            <el-button type="primary" @click="handleClose(item.id)">确 定</el-button>
          </span>
      </el-dialog>
      <!-- 加价购弹出界面 -->
      <el-dialog
        :visible.sync="dialogVisible2"
        width="50%" class="jiajiagou">
        <div style="text-align: left;margin-bottom:10px"><span>直接加价购</span> <span style="margin-left:30px">{{this.tableData.text1}}</span></div>
        <table style="border-bottom:2px solid #aaa">
          <tr style="border-top:2px solid #ddd;height:30px;">
            <td>商品图片</td>
            <td style="width:150px">商品名称</td>
            <td>促销价格</td>
            <td>商品数量</td>
          </tr>
          <tr style="height:30px;line-height:30px" v-for="(data, index) in this.tableData.showCartProductInfoList" :key="index">
            <td v-if="data.imageurl" ><img :src="data.imageurl" alt=""></td>
            <td v-else>暂无数据</td>
            <td>{{data.productvarname}}</td>
            <td>{{data.promoprice}}</td>
            <td v-text="data.qty">
              <!-- <el-input-number v-model="data.qty" @change="handleChangePopj" size="mini" :min="0"></el-input-number> -->
            </td>
          </tr>
        </table>
        <div style="width: 100%;margin-top:10px">
          <a>小计</a>
          <span style="margin-left:50px">{{'单价      '+'   '+'￥'+ this.tableData.groupprice}}</span>
          <div class="counter">
            <a @click="handlej">-</a>
            <input type="text" v-model="this.tableData.groupqty" :max="3">
            <a @click="addj">+</a>
              <!-- <el-input-number size="mini" v-model.lazy="this.tableData.groupqty" @change="handlepopgroupj" :min="this.tableData.minqty" ></el-input-number> -->
          </div>
          <span style="margin-left:120px" >金额 {{' ￥'+this.tableData.groupqty* this.tableData.groupprice+'元'}}</span>
        </div>
        <span slot="footer" class="dialog-footer">
          <el-button @click="dialogVisible2 = false">取 消</el-button>
          <el-button type="primary" @click="handleClosej">确 定</el-button>
        </span>
      </el-dialog>
    <footer id="float">
      <div class="footer_row" >
        <div class="toolbar-wrap">
          <div class="selected-item-list  hide" style="display:block"></div>
          <div class="selec_all">
            <el-checkbox v-model="checkeAll" @change="hadleCheckAllChange(checkeAll)"></el-checkbox>
            <label for class="goodcheack">全选</label>
          </div>
          <div class="operation">
            <button @click="delchoice_nom">删除选中的商品</button>
            <button @click="cartClear">清空购物车</button>
          </div>
          <div class="toolbar-right">
            <div class="btn_area" style="width:110px">
              <a href="" class="submit-btn">去结算</a>
            </div>
            <div class="price-sum" style="width:120px">
              <span class="txt txt-new">总价：</span>
              <span class="price sumPrice"><em v-if="this.datas.choiceoldamt">{{'￥'+this.datas.choiceoldamt}}</em><em v-else>0</em></span>
              <b class="ml5 price-tips"></b>
              <br>
              <span class="txt">已节省：</span>
              <span class="price totalRePrice" v-if="this.datas.choicereduceamt">{{'-￥'+this.datas.choicereduceamt}}</span>
              <span class="price totalRePrice" v-else>{{'-￥'+ 0}}</span>
            </div>
            <div class="amount-sum">
              已选择<em v-if="this.datas.choiceqty">{{this.datas.choiceqty}}</em> <em v-else>0</em>件商品<b class="up" clstag="clickcart|keycount|xincart|cart_thumbnailOpen"></b>
            </div>
            <div class="clear"></div>
          </div>
          <div class="clear"></div>
        </div>
      </div>
      <!-- <div class="footer_row isFixed1">
        <div class="toolbar-wrap">
          <div class="selected-item-list  hide" style="display:block"></div>
          <div class="selec_all">
            <el-checkbox v-model="checkeAll" @change="hadleCheckAllChange(checkeAll)"></el-checkbox>
            <label for class="goodcheack">全选</label>
          </div>
          <div class="operation">
            <button @click="delchoice_nom">删除选中的商品</button>
            <button @click="cartClear">清空购物车</button>
          </div>
          <div class="toolbar-right">
            <div class="btn_area">
              <a href="" class="submit-btn">去结算</a>
            </div>
            <div class="price-sum">
              <span class="txt txt-new">总价：</span>
              <span class="price sumPrice"><em v-if="this.datas.choiceoldamt">{{'￥'+this.datas.choiceoldamt}}</em><em v-else>0</em></span>
              <b class="ml5 price-tips"></b>
              <br>
              <span class="txt">已节省：</span>
              <span class="price totalRePrice" v-if="this.datas.choicereduceamt">{{'-￥'+this.datas.choicereduceamt}}</span>
              <span class="price totalRePrice" v-else>{{'-￥'+ 0}}</span>
            </div>
            <div class="amount-sum">
              已选择<em v-if="this.datas.choiceqty">{{this.datas.choiceqty}}</em> <em v-else>0</em>件商品<b class="up" clstag="clickcart|keycount|xincart|cart_thumbnailOpen"></b>
            </div>
            <div class="clear"></div>
          </div>
          <div class="clear"></div>
        </div>
      </div> -->
    </footer>
  </div>
</template>

<script>
import 'lib-flexible'
import Lineone from '@/components/good/line_one.vue'
import Linetwo from '@/components/good/line_two.vue'
import Linethree from '@/components/good/line_three.vue'
import Linefour from '@/components/good/line_four.vue'
import Textnormal from '@/components/good/text_normal.vue'
import Textgift from '@/components/good/text_gift.vue'
import Goodnomal from '@/components/good/goods_normal.vue'
import Gift from '@/components/good/gift.vue'
// import datas from './data.json';
// import vm from '@/assets/js/event.js'
export default {
  components: {
    Lineone,
    Linetwo,
    Linethree,
    Linefour,
    Textnormal,
    Textgift,
    Goodnomal,
    Gift
  },
  // mounted () {
  //   this.getAllqty()
  // },
  props: ['member'],
  data () {
    return {
      datas: [],
      checkeAll: false,
      prooptlist: [],
      tableData: [],
      dialogVisible: false,
      dialogVisible1: false,
      dialogVisible2: false,
      num: 0,
      amt: 0,
      reduceamt: 0,
      choiceqty: 0,
      carttype: '3',
      memberid: '521',
      membertype: '0',
      cartversion: '',
      elements: [],
      popdata: [],
      poplist: 0, // 弹出框显示已选商品的数量
      poplistj: 0,
      checkList: [],
      maxqty: 0, // 用于统计弹出数据列表中计数器中商品总数
      inputval: '',
      searchBarFixed: '' // 顶部浮动需要
    }
  },
  computed: {
  },
  watch: {
    datas (val) {
    },
    prooptlist (val) {
      this.prooptlist = val
    }
  },
  created () {
  },
  mounted () {
    window.addEventListener('scroll', this.handleScroll)
  },
  destroyed () {
    window.removeEventListener('scroll', this.handleScroll)
  },
  methods: {
    handleScroll () {
      let scrollTop = window.pageYOffset || document.documentElement.scrollTop || document.body.scrollTop
      let offsetTop = document.querySelector('#searchBar').offsetTop
      this.searchBarFixed = scrollTop > (offsetTop - 0)
      // console.log(scrollTop)
    },
    // 当选中文本时，跟他一组的商品得背景颜色也会发生改变
    groupchoice () {
      for (var h = 0; h <= this.datas.groups.length; h++) {
        if (this.datas.groups[h].elements.length > 2) {
          for (var k = 0; k <= this.datas.groups[h].elements.length; k++) {
            if (this.datas.groups[h].elements[k].eletype === 2 && this.datas.groups[h].elements[k].ischoice === 1) {
              return 'row itemChoice'
            }
          }
        }
        this.elements.push()
      }
    },
    // 基础购物车
    refresh () {
      this.$http.post('/cart/getCart', Object.assign({}, this.member, {
        'answersParameter': {
          'protype': 0
        },
        'receptionParameter': {
          // 'ischoice': 0,
          'opertype': 0,
          'qty': 10
        }
      })).then(res => {
        this.datas = res.data.data
      })
    },
    // 得到展示购物车
    intoshowCart () {
      this.prooptlist = []
      this.$http.post('/cart/intoShowCart', Object.assign({}, this.member))
        .then(res => {
          let {data} = res.data
          if (res.data.data.totalqty === data.choiceqty) {
            this.checkeAll = true
          } else if (this.datas.totalqty !== data.choiceqty) {
            this.checkeAll = false
          }
          this.datas = data
          this.amt = this.datas.choiceamt
          // console.log('/cart/intoShowCart=>', this.datas)
          this.reduceamt = this.datas.choicereduceamt
          this.choiceqty = this.datas.choiceqty
          this.carttype = this.datas.carttype
          this.memberid = this.datas.memberid
          this.membertype = this.datas.memtype
          this.cartversion = this.datas.version
          for (var i = 0; i < this.datas.groups.length; i++) {
            for (var j = 0; j < this.datas.groups[i].elements.length; j++) {
              if (this.datas.groups[i].elements[j].ischoice === 1 || this.datas.groups[i].ischoice === 1) { // 判断被选中
                if (this.datas.groups[i].grouptype === 1) { // 判断是否是组合
                  if (this.datas.groups[i].elements[j].showtype === 2) { // 判断是否是组合标头信息,商品级促销
                    this.prooptlist.push(JSON.parse(this.datas.groups[i].elements[j].operparam))
                  }
                } else {
                  this.prooptlist.push(this.datas.groups[i].elements[j])
                }
              }
            }
            // console.log('this.prooptlist=>', this.prooptlist)
          }
        })
    },
    // 得到购物车是商品总数量 暂时没用
    getAllqty () {
      this.$http.post('8002/cart/getCartQty', {
        'channeltype': 'A',
        'malorgid': 1,
        'memtype': 0,
        'outorgid': 854
      })
        .then(res => {
          if (res.status === 200) {
            // console.log(res.data)
          } else {
            this.$message.errorMsg()
          }
        })
    },
    // 改变商品数量 传递到商品组件 点击计数器时触发
    inputNumberChange (operparam, opertype, num, updataqty) {
      if (opertype === 0) { // 设置数量
        if (num > 200) {
          alert('商品数量最大值是200')
          num = 200
          // return false
        } else if (!num) {
          num = 1
        }
      } else if (opertype === 1) { // 累加数量
        if (updataqty > 199) {
          if (num === +1) {
            alert('商品数量最大值是200')
            num = 0
            return false
          } else if (num === -1) {
            num = -1
          }
        }
      }
      this.$http.post('/cart/dealShowCartQty', Object.assign({}, this.member, {
        'answersParameter': JSON.parse(operparam),
        'receptionParameter': {
          'opertype': opertype,
          'qty': num
        }
      })).then(res => {
        // console.log('/cart/dealShowCartQty11=>', res)
        this.datas = res.data.data
        this.$emit('productSearch')
      })
    },
    numberChange () {
      alert('商品数量不能超过200')
    },
    // 促销框中的方法
    getRadioVal (val) {
      this.inputval = val
      console.log('this.inputval=>', this.inputval)
    },
    // 促销框中确定按钮出发的方法
    showtogglesure (operparam) {
      console.log('operparam=>', operparam)
      this.$http.post('/cart/dealShowCartPromoChoice', Object.assign({}, this.member, {
        'answersParameter': JSON.parse(operparam),
        'pid': this.inputval.pid,
        'gid': this.inputval.gid
      })).then(res => {
        console.log(res)
        this.datas = res.data.data.groups
        console.log('this.datas=>', this.datas)
        this.intoshowCart()
      })
    },
    // 删除商品
    delet_nom (operparam) {
      // console.log('operparam=>', operparam)
      let con = confirm('你确定要删除商品吗')
      if (con === true) {
        this.$http.post('/cart/dealShowCartRemove', Object.assign({}, this.member, {
          'answersParameter': JSON.parse(operparam)
        }))
          .then(res => {
            this.tableData = res.data.data.groups
            console.log('delet_nom=>', this.tableData)
            if (res.status === 200) {
              this.intoshowCart()
              this.$emit('productSearch')
            } else {
              this.$message.errorMsg()
            }
          })
      } else {
      }
    },
    // 删除选中商品
    delchoice_nom (operparam) {
      this.$http.post('/cart/dealShowCartRemoveList', Object.assign({}, this.member, {
        'answersParameter': operparam,
        'prooptlist': this.prooptlist
      }))
        .then(res => {
          if (res.status === 200) {
            this.$emit('productSearch')
          } else {
            this.$message.errorMsg()
          }
        })
      this.prooptlist = []
    },
    // 选择或取消选择
    dealShowCartChoice (operparam, id, choice) {
      this.$http.post('/cart/dealShowCartChoice', Object.assign({}, this.member, {
        'answersParameter': JSON.parse(operparam),
        'receptionParameter': {
          'ischoice': choice,
          'productvarid': id
        }
      })).then(res => {
        let {data} = res.data
        this.tableData = data
        if (data.totalqty === data.choiceqty) {
          this.checkeAll = true
        } else if (this.datas.totalqty !== data.choiceqty) {
          this.checkeAll = false
        }
        this.intoshowCart()
        this.prooptlist = []
      })
    },
    // 清空购物车
    cartClear () {
      this.$http.post('/cart/cartClear', {
        'carttype': this.carttype,
        'memberid': this.memberid,
        'memtype': this.membertype
      })
        .then(res => {
          this.datas = []
        })
    },
    // 全选
    hadleCheckAllChange (val) {
      console.log('val=>', val)
      this.$http.post('/cart/dealShowCartChoiceAll', Object.assign({}, this.member, {
        'answersParameter': {
          'precartversion': this.cartversion
        },
        'receptionParameter': {
          'ischoice': val === true ? 1 : 0
        }
      }))
        .then(res => {
          this.datas = res.data.data
          // console.log('this.datas=>', this.datas)
          for (var i = 0; i < this.datas.groups.length; i++) {
            for (var j = 0; j < this.datas.groups[i].elements.length; j++) {
              if (this.checkeAll === true) {
                this.datas.groups[i].ischoice = 1
                this.datas.groups[i].elements[j].ischoice = 1
                // console.log('true=>', this.datas)
              } else if (this.checkeAll === false) {
                this.datas.groups[i].ischoice = 0
                this.datas.groups[i].elements[j].ischoice = 0
                // console.log('false', this.datas)
              }
            }
          }
        })
    },
    // 得到任选组合数据
    cartPromoPop (clickparam) {
      // maxqty = 0 时  弹出框不弹出
      if (this.tableData.maxqty === 0) {
        this.dialogVisible = false
      } else {
        this.dialogVisible = true
        this.$http.post('/cart/getShowCartPromoPop', Object.assign({}, this.member, {
          'answersParameter': JSON.parse(clickparam)
        }))
          .then(res => {
            this.tableData = res.data.data
            console.log('this.tableData=>', res.data.data)
            // 得到组合中的商品数量 在弹出框中显示
            this.poplist = 0
            for (var i = 0; i < this.tableData.showCartProductInfoList.length; i++) {
              this.poplist = this.poplist + this.tableData.showCartProductInfoList[i].qty
            }
          })
        this.poplist = this.tableData.selqty * this.tableData.groupqty
      }
    },
    // 任选组合商品得加减
    handleChangePop () {
      this.poplist = 0
      this.maxqty = 0
      for (var i = 0; i < this.tableData.showCartProductInfoList.length; i++) {
        this.maxqty = this.maxqty + this.tableData.showCartProductInfoList[i].qty
        this.poplist = this.poplist + this.tableData.showCartProductInfoList[i].qty
      }
      console.log('this.maxqty=>', this.maxqty)
    },
    add () {
      this.tableData.groupqty++
      // console.log('this.tableData.groupqty=>', this.tableData.groupqty)
    },
    handle () {
      this.tableData.groupqty--
      // console.log('this.tableDatsa.groupqty=>', this.tableData.groupqty)
    },
    // 处理任选组合数据
    handleClose () {
      this.maxqty = 0
      this.checkList = [] // 用于传输弹出框中商品数量大于1的商品列表
      for (var i = 0; i < this.tableData.showCartProductInfoList.length; i++) {
        if (this.tableData.showCartProductInfoList[i].qty !== 0) {
          this.checkList.push(this.tableData.showCartProductInfoList[i])
          this.maxqty = this.maxqty + this.tableData.showCartProductInfoList[i].qty
        }
      }
      if (this.tableData.maxqty === -1) {
        this.$http.post('/cart/dealShowCartPromoPop', Object.assign({}, this.member, {
          'answersParameter': JSON.parse(this.tableData.operparam),
          'list': this.checkList,
          'receptionParameter': {
            'groupqty': this.tableData.groupqty
          }
        }))
          .then(res => {
            this.datas = res.data.data
            this.dialogVisible = false
          })
      } else {
        if (this.tableData.maxqty * this.tableData.groupqty === this.maxqty) {
          this.$http.post('/cart/dealShowCartPromoPop', Object.assign({}, this.member, {
            'answersParameter': JSON.parse(this.tableData.operparam),
            'list': this.checkList,
            'receptionParameter': {
              'groupqty': this.tableData.groupqty
            }
          }))
            .then(res => {
              if (this.maxqty > this.tableData.maxqty) {
                alert('商品数量选择不符合要，请重新选择')
              } else {
                this.datas = res.data.data
                this.dialogVisible = false
              }
            })
        } else {
          alert('商品数量选择不符合要求，请重新选择')
        }
      }
    },
    // 得到赠品组合数据
    cartPromoPopz (clickparam) {
      // maxqty = 0 时  弹出框不弹出
      if (this.tableData.maxqty === 0) {
        this.dialogVisible = false
      } else {
        this.dialogVisible = true
        this.$http.post('/cart/getShowCartPromoPop', Object.assign({}, this.member, {
          'answersParameter': JSON.parse(clickparam)
        }))
          .then(res => {
            this.tableData = res.data.data
            // 得到组合中的商品数量 在弹出框中显示
            this.poplistz = 0
            for (var i = 0; i < this.tableData.showCartProductInfoList.length; i++) {
              this.poplistz = this.poplistz + this.tableData.showCartProductInfoList[i].qty
            }
          })
        this.poplistz = this.tableData.selqty * this.tableData.groupqty
      }
    },
    // 赠品组合商品组加减
    handleChangegift (value) {
      this.poplist = 0
      for (var i = 0; i < this.tableData.grouplist.length; i++) {
        this.poplist = this.poplist + this.tableData.grouplist[i].qty
      }
      console.log('this.poplist=>', this.poplist)
    },
    // 处理赠品组合数据
    handleClosez () {
      this.poplistz = 0
      for (var i = 0; i < this.tableData.showCartProductInfoList.length; i++) {
        if (this.tableData.showCartProductInfoList[i].qty !== 0) {
          this.checkList.push(this.tableData.showCartProductInfoList[i])
          this.poplistz = this.poplistz + this.tableData.showCartProductInfoList[i].qty
        }
      }
      if (this.tableData.maxqty <= this.poplistz) {
        this.$http.post('/cart/dealShowCartPromoPop', Object.assign({}, this.member, {
          'answersParameter': JSON.parse(this.tableData.operparam),
          'list': this.checkList
        }))
          .then(res => {
            this.dialogVisible1 = false
            this.datas = res.data.data
          })
      } else {
        alert(this.tableData.maxmsg + '，请重新选择')
      }
    },
    // 得到加价购组合数据
    cartPromoPopj (clickparam) {
      // maxqty = 0 时  弹出框不弹出
      if (this.tableData.maxqty === 0) {
        this.dialogVisible2 = false
      } else {
        this.dialogVisible2 = true
        this.$http.post('/cart/getShowCartPromoPop', Object.assign({}, this.member, {
          'answersParameter': JSON.parse(clickparam)
        }))
          .then(res => {
            this.tableData = res.data.data
            // 得到组合中的商品数量 在弹出框中显示
            this.poplistj = 0
            for (var i = 0; i < this.tableData.showCartProductInfoList.length; i++) {
              this.poplistj = this.poplistj + this.tableData.showCartProductInfoList[i].qty
            }
            this.checkList = []
            this.poplistj = this.tableData.selqty * this.tableData.groupqty
          })
      }
    },
    // 加价购数据加减
    addj () {
      this.tableData.groupqty++
    },
    handlej () {
      if (this.tableData.groupqty > 0) {
        this.tableData.groupqty--
      }
    },
    // 加价购组合商品组加减
    handleChangePopj (value) {
      this.poplistj = 0
      for (var i = 0; i < this.tableData.showCartProductInfoList.length; i++) {
        this.poplistj = this.poplistj + this.tableData.showCartProductInfoList[i].qty
        console.log(this.poplistj)
      }
    },
    // 处理加价购数据
    handleClosej () {
      this.poplistj = 0
      for (var i = 0; i < this.tableData.showCartProductInfoList.length; i++) {
        if (this.tableData.showCartProductInfoList[i].qty !== 0) {
          this.checkList.push(this.tableData.showCartProductInfoList[i])
          this.poplistj = this.poplistj + this.tableData.showCartProductInfoList[i].qty
        }
      }
      // console.log('this.tableData.maxqty=>', this.tableData.maxqty)
      // console.log('this.tableData.groupqty=>', this.tableData.groupqty)
      if (this.tableData.maxqty >= this.poplistj * this.tableData.groupqty) {
        this.$http.post('/cart/dealShowCartPromoPop', Object.assign({}, this.member, {
          'answersParameter': JSON.parse(this.tableData.operparam),
          'list': this.checkList,
          'receptionParameter': {
            'groupqty': this.tableData.groupqty
          }
        }))
          .then(res => {
            this.dialogVisible2 = false
            this.datas = res.data.data
          })
      } else {
        alert(this.tableData.maxmsg + '，请重新选择')
      }
    }
  }
}
</script>
<style scoped>
/* shopcar样式 */
  #shopcar {
    width: 99.8%;
    border:1px solid #95B8E7;
    padding-left:1px
    /* border:1px solid blue */
  }
  #shopcar header{
    height: 48px;
    width: 100%;
    /* border:1px solid blue */
  }
  /* 顶部浮动条样式 */
  #searchBar .isFixed{
    position:fixed;
    background-color:#fff;
    top:0px;
    z-index:999;
  }
  #shopcar #header_row {
    width: 99.9%;
    height: 48px;
    z-index: 101;
    box-shadow: 0 -1px 8px rgba(0,1,1,.08);
    background: #fff;
    border-width: 1px;
    border-top: 1px solid #f0f0f0;
    /* border:1px solid blue */
  }
  #shopcar #header_row .s1,#header_row .s2,#header_row .s4,#header_row .s5,#header_row .s6,#header_row .s7 {
    /* border: 1px solid green; */
    text-align: center;
    height: 48px;
    line-height: 48px;
    display: inline-block;
  }
  #shopcar #header_row .s1 {
  width: 80px;
  }
  #shopcar #header_row .s1 .el-checkbox {
    text-align: center;
  }
  #shopcar #header_row .s1 .goodcheack {
    padding-left: 5px;
  }
  #shopcar #header_row .s2{
    width: 400px;
  }
  #shopcar #header_row .s4{
    width: 80px;
    text-align: left;
    /* border:1px solid red; */
  }
  #shopcar #header_row .s5{
    width: 150px;
  }
  #shopcar #header_row .s6{
    width: 90px;
  }
  #shopcar #header_row .s7{
    width: 90px;
    /* float: right; */
  }
  #shopcar footer{
    height: 55px;
    display: block;
  }
  #shopcar .footer_row {
    display: block;
    position: fixed;
    bottom: 0px;
    height: 55px;
    line-height: 55px;
    z-index:999;
    box-shadow: 0 -1px 8px rgba(0,1,1,.08);
    background: #fff;
    border-top: 1px solid #f0f0f0 \9;
  }
  #shopcar footer .toolbar-wrap{
    position: relative;
    width: 99.8%;
    display: block;
    border: 1px solid #fff
  }
  #shopcar footer .selec_all,footer .operation,footer .toolbar-right{
    display: inline-block;
    float: left;
    height: 55px;
    /* position: fixed; */
    /* border: 1px solid red; */
    line-height: 55px;
  }
  /* 全选 */
  #shopcar footer .selec_all{
    width: 80px;
    text-align: center;
    margin-right: 9px
  }
  #shopcar .footer_row .selec_all .el-checkbox {
    text-align: center;
  }
  #shopcar .footer_row .selec_all  .goodcheack {
    padding-left: 5px;
  }
  /* 删除选中商品 清空购物车 */
  #shopcar footer .operation{
    width: 270px;
    /* border: 1px solid blue; */
  }
  #shopcar footer .operation button{
    display: inline-block;
    width: 130px;
    border: none;
    background-color: #fff;
    line-height: 55px;
    height: 55px
  }
  /* 右侧样式 */
  #shopcar footer .toolbar-right{
    width: 560px;
    /* border:1px solid blueviolet; */
  }
  footer .toolbar-right .btn_area,footer .toolbar-right .price-sum,footer .toolbar-right .amount-sum{
    display: inline-block;
    text-align: center;
    float: right;
    height: 55px;
    width:230px;
    /* line-height: 20px; */
  }
  /*右侧 中间 */
  #shopcar footer .toolbar-right .price-sum{
    width: 240px;
    float: right;
    line-height: 29px;
    color: #666;
    position: relative;
    /* border:1px solid blueviolet; */
  }
  #shopcar footer .toolbar-right .price-sum .txt {
    float: left;
    width: 80px;
    /* border:1px solid blueviolet; */
    text-align: right;
    line-height: 24px;
    color: #999;
  }
  #shopcar footer .toolbar-right .price-sum .price em {
    font-size: 12px;
    color: #E2231A;
    font-weight: 700;
  }
  #shopcar footer .toolbar-right .price-sum .price {
    display: inline-block;
    text-align: right;
    font-family: verdana;
  }
  #shopcar footer .toolbar-right .price-sum .totalRePrice{
    margin-left:-20px;
  }
  /* 灯图标 */
  #shopcar footer .toolbar-right .price-sum .price-tips {
      vertical-align: text-bottom;
      display: inline-block;
      height: 20px;
      width: 20px;
      /* border:1px solid red; */
      margin-left: 5px;
      background: url(//misc.360buyimg.com/user/cart/css/i/price-tips.png) 0 0 no-repeat;
  }
  /* 去结算样式 */
  #shopcar footer .toolbar-right .toolbar-wrap .btn_area {
      float: right;
      /* width: 120px!important; */
  }
  #shopcar footer .toolbar-right .btn_area .submit-btn {
      display: block;
      position: relative;
      height: 55px;
      line-height:55px;
      color: #fff;
      text-align: center;
      font-size: 16px;
      font-weight: bold;
      font-family: "Microsoft YaHei";
      background: #e54346;
      overflow: hidden;
  }
 #shopcar .footer_row .toolbar-right .amount-sum{
   line-height: 30px;
 }

  #shopcar .footer_row .toolbar-right .amount-sum b .up{
  }
  #shopcar .footer_row .amount-sum b {
      display: inline-block;
      width: 15px;
      height: 6px;
      background: url(//misc.360buyimg.com/user/cart/css/i/cart-icons-201811.png) -60px -127px;
      overflow: hidden;
      vertical-align: middle;
      margin: -5px 0 0 5px;
      transition: all .2s ease-in 0s;
  }
/* shopcar样式结束  */
/* 弹出界面样式设置开始任选组合 */
.renxuan table {
  border-top: 2px solid #ddd;
  border-bottom: 2px solid #ddd;
  padding:10px 0;
}
.renxuan .el-dialog__header {
    padding: 16px 16px 0px;
}
.renxuan .el-dialog__body {
  padding: 15px 20px;
  color: #606266;
  font-size: 11px;
}
.renxuan td {
  border-bottom:1px solid #eee;
  padding-right: 29px;
}
.renxuan .el-input-number--mini{
  width: 100px;
}
.renxuan .counter {
  display: inline-block;
  width: 99px;
  height: 26px;
  line-height: 26px;
  border: 1px solid #ccc;
  border-radius: 4px;
  font-size: 12px;
  margin-left: 30px;
  margin-top: 7px;
}
.renxuan .counter:hover {
  border:1px solid#409EFF;
}
.renxuan .counter a{
  width: 26px;
  font-size: 12px;
  height: auto;
  text-align: center;
  background: #f5f7fa;
  color: #606266;
  display: inline-block;
}
.renxuan .counter a:hover{
  color: #409EFF;
}
.renxuan .counter a:first-child {
  left: 1px;
  border-radius: 4px 0 0 4px;
  border-right: 1px solid #dcdfe6;
  margin-left: 0px;
}
.renxuan .counter a:last-child {
  right: 1px;
  border-radius: 0 4px 4px 0;
  border-left: 1px solid #dcdfe6;
}
.renxuan .counter input {
  width: 38px;
  height: 26px;
  border: none;
  text-align: center;
  margin: 0;
}
/* 任选组合弹出界面样式设置结束 */
/* 加价购界面样式设置开始 */
.jiajiagou tr {
  text-align: center
}
.jiajiagou td {
  border-bottom:1px solid #eee;
  padding-right: 29px;
}
.jiajiagou .el-input-number--mini{
  width: 100px;
}
.jiajiagou .counter {
  display: inline-block;
  width: 99px;
  height: 26px;
  line-height: 26px;
  border: 1px solid #ccc;
  border-radius: 4px;
  font-size: 12px;
  margin-left: 70px;
  margin-top: 7px;
}
.jiajiagou .counter:hover {
  border:1px solid#409EFF;
}
.jiajiagou .counter a{
  width: 26px;
  font-size: 12px;
  height: auto;
  text-align: center;
  background: #f5f7fa;
  color: #606266;
  display: inline-block;
}
.jiajiagou .counter a:hover{
  color: #409EFF;
}
.jiajiagou .counter a:first-child {
  left: 1px;
  border-radius: 4px 0 0 4px;
  border-right: 1px solid #dcdfe6;
  margin-left: 0px;
}
.jiajiagou .counter a:last-child {
  right: 1px;
  border-radius: 0 4px 4px 0;
  border-left: 1px solid #dcdfe6;
}
.jiajiagou .counter input {
  width: 38px;
  height: 26px;
  border: none;
  text-align: center;
  margin: 0;
}
/* 加价购界面样式设置结束 */
</style>
