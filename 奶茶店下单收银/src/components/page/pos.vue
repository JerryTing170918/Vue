<!-- eslint-disable -->
<template>
  <div class="pos">
    <el-row>
      <el-col :span="7" class="pos-order" id="order-list">订单栏
        <el-tabs>
          <el-tab-pane label="点餐">
              <el-table :data="tableData" border style="width:100%">
                <el-table-column prop="goodsName" label="商品名称"></el-table-column>
                <el-table-column prop="count" label="数量"></el-table-column>
                <el-table-column prop="price" label="价格"></el-table-column>
                <el-table-column prop="action" label="操作">
                  <template scope="scope">
                    <el-button type="text" size="small" v-on:click="delSingleGoods(scope.row)">删除</el-button>
                    <el-button type="text" size="small" v-on:click="addOrderList(scope.row)">增加</el-button>
                  </template>
                </el-table-column>
              </el-table>  
            <div class="totalDiv">
              <small>数量：</small>{{totalCount}} <small>总价：</small>{{totalMoney}}元
            </div>
            <div class="actionBtn">
              <el-button type="danger" v-on:click="delAllGoods()">删除</el-button>
              <el-button type="success" @click="checkout()">买单</el-button>
            </div>
          </el-tab-pane>
          <el-tab-pane label="打包">
            <el-table :data="tableData" border style="width:100%">
              <el-table-column prop="goodsName" label="商品名称"></el-table-column>
              <el-table-column prop="count" label="数量"></el-table-column>
              <el-table-column prop="price" label="价格"></el-table-column>
              <el-table-column prop="action" label="操作">
                <template scope="scope">
                  <el-button type="text" size="small" v-on:click="delSingleGoods(scope.row)">删除</el-button>
                  <el-button type="text" size="small" v-on:click="addOrderList(scope.row)">增加</el-button>
                </template>
              </el-table-column>
            </el-table>
            <div class="totalDiv">
              <small>数量：</small>{{totalCount}} <small>总价：</small>{{totalMoney}}元
            </div>
            <div class="actionBtn">
              <el-button type="danger" v-on:click="delAllGoods()">删除</el-button>
              <el-button type="success" @click="checkout()">买单</el-button>
            </div>
          </el-tab-pane>
        </el-tabs>

      </el-col>
      <el-col :span="17" class="pos-goods"><!--商品栏-->
        <div class="hot-sell">
          <div class="title">热卖品</div>
          <div class="hot-sell-list">
            <ul>
              <li v-for="goods in hotSell"  @click="addOrderList(goods)"><!--点击右侧热卖添加到左侧订单栏-->
                <span class="goodImg"><img :src="goods.goodsImg" width="90px"></span>
                <span class="goodName">{{goods.goodsName}}</span>
                <span class="goodPrice">￥{{goods.price}}元</span>
              </li>
            </ul>
          </div>
        </div>

        <div class="goods-type">
          <el-tabs>
            <el-tab-pane label="奶昔">
              <div>
                <ul class="goodsList">
                  <li v-for="goods in type0Goods" @click="addOrderList(goods)"><!--从类型中点击添加到左侧订单栏-->
                    <span class="goodImg"><img :src="goods.goodsImg" width="90px"></span>
                    <span class="goodName">{{goods.goodsName}}</span>
                    <span class="goodPrice">￥{{goods.price}}元</span>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="奶茶">
              <div>
                <ul class="goodsList">
                  <li v-for="goods in type1Goods" @click="addOrderList(goods)">
                    <span class="goodImg"><img :src="goods.goodsImg" width="90px"></span>
                    <span class="goodName">{{goods.goodsName}}</span>
                    <span class="goodPrice">￥{{goods.price}}元</span>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="沙冰">
              <div>
                <ul class="goodsList">
                  <li v-for="goods in type2Goods" @click="addOrderList(goods)">
                    <span class="goodImg"><img :src="goods.goodsImg" width="90px"></span>
                    <span class="goodName">{{goods.goodsName}}</span>
                    <span class="goodPrice">￥{{goods.price}}元</span>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="刨冰">
              <div>
                <ul class="goodsList">
                  <li v-for="goods in type3Goods" @click="addOrderList(goods)">
                    <span class="goodImg"><img :src="goods.goodsImg" width="90px"></span>
                    <span class="goodName">{{goods.goodsName}}</span>
                    <span class="goodPrice">￥{{goods.price}}元</span>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
          </el-tabs>
        </div>
      </el-col>
    </el-row>
  </div>
</template>

<script>
/*eslint-disable*/
import axios from 'axios';
export default {
  name: 'pos',//element是虚拟dom
  data(){
    return {
      tableData:[],//格式
      hotSell:[],
      type0Goods:[],
      type1Goods:[],
      type2Goods:[],
      type3Goods:[],
      totalCount:0
    }
  },
  created() {
    axios.get('https://api.myjson.com/bins/1gj18m')//线上接口
    // axios.get('../static/hotSell.json')//本地模拟接口
    .then(response=>{
      this.hotSell=response.data;
    })
    .catch(error=>{
      alert('设备未联网')
    });
    axios.get('https://api.myjson.com/bins/i21ba')
    // axios.get('../static/type.json')
    .then(response=>{
      this.type0Goods=response.data[0];//格式
      this.type1Goods=response.data[1];
      this.type2Goods=response.data[2];
      this.type3Goods=response.data[3];
    })
    .catch(error=>{
      alert('设备未联网')
    })
  },
  
  mounted:function (){
    var orderHeight = document.body.clientHeight;
    console.log(orderHeight);
    document.getElementById('order-list').style.height=orderHeight+'px'//100%行高
  },
  methods:{
      //添加订单列表的方法
      addOrderList(goods){
            this.totalCount=0; //汇总数量清0
            this.totalMoney=0;
            let isHave=false;
            //判断是否这个商品已经存在于订单列表
            for (let i=0; i<this.tableData.length;i++){
                if(this.tableData[i].goodsId==goods.goodsId){
                    isHave=true; //存在
                }
            }
            //根据isHave的值判断订单列表中是否已经有此商品
            if(isHave){
                //存在就进行数量添加
                 let arr = this.tableData.filter(o =>o.goodsId == goods.goodsId);
                 arr[0].count++;
                 //console.log(arr);
            }else{
                //不存在就推入数组
                let newGoods={goodsId:goods.goodsId,goodsName:goods.goodsName,price:goods.price,count:1};
                 this.tableData.push(newGoods);
            }
            this.getAllMoney();
      },
      //删除单个商品
      delSingleGoods(goods){
        this.tableData=this.tableData.filter(o => o.goodsId !=goods.goodsId);
        this.getAllMoney();
      },
      //删除所有商品
      delAllGoods() {
            this.tableData = [];
            this.totalCount = 0;
            this.totalMoney = 0;
        },
      //汇总数量金额
      getAllMoney(){
        this.totalCount = 0;
        this.totalMoney = 0;
        if(this.tableData){
          //进行数量和价格的汇总计算
            this.tableData.forEach((element) => {
                this.totalCount+=element.count;
                this.totalMoney=this.totalMoney+(element.price*element.count);   
            });    
        }
      },
      //模拟买单
      checkout() {
        if (this.totalCount!=0) {
            this.tableData = [];
            this.totalCount = 0;
            this.totalMoney = 0;
            this.$message({
                message: '买单成功',
                type: 'success'
            });

        }else{
            this.$message.error('未选中商品');
        };

}
  }
};
</script>


<style scoped>
  .pos-order{
    background-color:#F9FaFC;
    border-right:1px solid rgb(230, 242, 255);
    height: 100%;
    float:left;
    overflow:auto;
  }
  .pos{
    overflow:auto;
  }
  .pos-goods{
    overflow:auto;
  }
  .actionBtn{
    margin-top: 10px;
  }
  .hot-sell-list ul li{
    list-style: none;
    float: left;
    border:1px solid rgb(225, 231, 243);
    width:210px;
    padding:5px;
    margin:10px;
    background-color: #fff;
    cursor: pointer;
  }
  .hot-sell-list ul li span{
    display: block;
    float:left;
  }
  span.goodName{
    margin:15px 0 0 10px;
  }
  span.goodPrice{
    margin:10px 0 0 10px;
  }
  .o-price{
    color:#58b7ff;
  }
  .goods-type{
    clear:both;
  }
  .goodsList li{
       list-style: none;
        float: left;
        border:1px solid rgb(225, 231, 243);
        width:210px;
        padding:5px;
        margin:10px;
        background-color: #fff;
        cursor: pointer;

   }
   .goodsList li span{
       
        display: block;
        float:left;
        
   }
   .goodImg{
       width: 40%;
   }
   .goodName{
       font-size: 18px;
       padding-left: 10px;
       color:brown;

   }
   .goodPrice{
       font-size: 16px;
       padding-left: 10px;
       padding-top:10px;
   }
   .totalDiv{
     background-color: #fff;
     padding: 10px;
     border-bottom: 1px solid brown;
   }
</style>
