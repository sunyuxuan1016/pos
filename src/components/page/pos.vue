<template>
  <div class="pos">
  <el-row>
    <el-col :span='7' class="pos-order" id="order-list">
      <el-tabs>
        <el-tab-pane label='点餐'>
          <el-table :data="tabledat" border height="400" style="widt:100%">
            <el-table-column prop="goodsName" label="商品名称"></el-table-column>
            <el-table-column prop="count" label="数量"></el-table-column>
            <el-table-column prop="price" label="金额"></el-table-column>
            <el-table-column prop="goodsName" label="操作" fixed="right">
              <template slot-scope="scope">
                <el-button type="text" size="small" @click="delsinglegoods(scope.row)">删除</el-button>
                <el-button type="text" size="small" @click="addorderlist(scope.row)">增加</el-button>
              </template>
            </el-table-column>
          </el-table>
          <div>
            <span>数量：{{totalCount}}</span>
            <span>金额:{{totalMoney}}</span>
            
          </div>
          <div class="div-btn">
              <el-button type="warning" size="small" @click="guadan()">挂单</el-button>
              <el-button type="danger" size="small" @click="dalallgoods()">删除</el-button>
           
              <el-button type="success" size="small" @click="checkout()">结账</el-button>
          </div>
        </el-tab-pane>



        <el-tab-pane label='挂单'>
              <el-table :data="guadions[0]" border height="400" style="widt:100%">
            <el-table-column prop="goodsName" label="商品名称"></el-table-column>
            <el-table-column prop="count" label="数量"></el-table-column>
            <el-table-column prop="price" label="金额"></el-table-column>
            <el-table-column prop="goodsName" label="操作" fixed="right">
             
            </el-table-column>
          </el-table>
          <div>
            <span>数量：{{guadancount}}</span>
            <span>金额:{{guadanMoney}}</span>
            
          </div>
          <div class="div-btn">
            
              <el-button type="danger" size="small" @click="dalallguadan()">删除</el-button>
           
              <el-button type="success" size="small" @click="guandankout()">结账</el-button>
          </div>
        </el-tab-pane>




        <el-tab-pane label='外卖'>
外卖
        </el-tab-pane>
      </el-tabs>
    </el-col>
    <el-col :span="17">
      <div class="ofetn-goud">
        <div class="title">常用商品</div>
          <div class="ofetn-goud-list">
            <ul>
              <li v-for="goods in oftengoods" :key="goods.id" @click="addorderlist(goods)">
                <span>{{goods.goodsName}}</span>
                <span>{{goods.goodsId}}</span>
              
                <span class="o-pic">￥{{goods.price}}</span>
              </li>
            </ul>
          </div>
      </div>

      <div class="goods-type">
          <el-tabs>
            <el-tab-pane label="汉堡">
              <div>
                <ul class="cookList">
                  <li v-for="food in type0Goods" :key="food.id" @click="addorderlist(food)">
                    <span class="foodImg"><img :src="food.goodsImg"/></span>
                    <span class="foodName">{{food.goodsName}}</span>
                    <span class="foodPrice">￥{{food.price}}元</span>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="小食品">
              <div>
                <ul class="cookList">
                  <li v-for="food in type1Goods" :key="food.id" @click="addorderlist(food)" >
                    <span class="foodImg"><img :src="food.goodsImg"/></span>
                    <span class="foodName">{{food.goodsName}}</span>
                    <span class="foodPrice">￥{{food.price}}元</span>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="饮料">
              <div>
                <ul class="cookList">
                  <li v-for="food in type2Goods" :key="food.id" @click="addorderlist(food)" >
                    <span class="foodImg"><img :src="food.goodsImg"/></span>
                    <span class="foodName">{{food.goodsName}}</span>
                    <span class="foodPrice">￥{{food.price}}元</span>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="套餐"><div>
                <ul class="cookList">
                  <li v-for="food in type3Goods" :key="food.id" @click="addorderlist(food)">
                    <span class="foodImg"><img :src="food.goodsImg"/></span>
                    <span class="foodName">{{food.goodsName}}</span>
                    <span class="foodPrice">￥{{food.price}}元</span>
                  </li>
                </ul>
              </div></el-tab-pane>
          </el-tabs>
      </div>
    </el-col>
  </el-row>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "pos",
  data() {
    return {
      tabledat: [],

      oftengoods: [],

      type0Goods: [],
      type1Goods: [],
      type2Goods: [],
      type3Goods: [],

      totalMoney: 0,
      totalCount: 0,

      //挂单
      guadions: [],
      guadancount: 0,
      guadanMoney: 0
    };
  },
  created: function() {
    axios
      .get("http://jspang.com/DemoApi/oftenGoods.php")
      .then(Response => {
        console.log(Response.data);
        this.oftengoods = Response.data;
      })
      .catch(error => {
        //console.log(error);
        alert("网络错误，不能访问");
      });
    axios
      .get("http://jspang.com/DemoApi/typeGoods.php")
      .then(Response => {
        console.log(Response.data);
        this.type0Goods = Response.data[0];
        this.type1Goods = Response.data[1];
        this.type2Goods = Response.data[2];
        this.type3Goods = Response.data[3];
      })
      .catch(error => {
        //console.log(error);
        alert("网络错误，不能访问");
      });
  },
  mounted: function() {
    var orderheight = document.body.clientHeight;
    //console.log(orderheight);
    document.getElementById("order-list").style.height = orderheight + "px";
  },
  methods: {
    addorderlist(goods) {
      //商品是否已经存在订单列表
      let isHave = false;
      //console.log(this);
      for (let i = 0; i < this.tabledat.length; i++) {
        if (this.tabledat[i].goodsId == goods.goodsId) {
          isHave = true;
        }
      }
      console.log(isHave);
      //根据判断的值编写业务逻辑
      if (isHave) {
        //改变列表中商品数量
        let arr = this.tabledat.filter(o => o.goodsId == goods.goodsId);
        //console.log(this.tabledat);
        arr[0].count++;
      } else {
        let newGoods = {
          goodsId: goods.goodsId,
          goodsName: goods.goodsName,
          price: goods.price,
          count: 1
        };
        this.tabledat.push(newGoods);
      }
      //汇总
      this.getallmoney();
      //计算价格数量
      // this.totalMoney = 0; //清0汇总数量
      // this.totalCount = 0;
      // this.tabledat.forEach(Element => {
      //   console.log(this.totalCount);
      //   console.log(Element);
      //   this.totalCount += Element.count;
      //   this.totalMoney = this.totalMoney + Element.price * Element.count;
      // });
    },

    //删除单个商品
    delsinglegoods(goods) {
      this.tabledat = this.tabledat.filter(o => o.goodsId != goods.goodsId);
      this.getallmoney();
    },
    //删除所有商品
    dalallgoods() {
      (this.guadions = []), (this.guadancount = 0), (this.guadanMoney = 0);
      //this.getallmoney();
    },
    //删除所有挂单商品
    dalallguadan() {
      (this.guadions = []), (this.guadancount = 0), (this.guadanMoney = 0);
    },
    //模拟点餐结账
    checkout() {
      if (this.totalCount != 0) {
        this.tabledat = [];
        this.totalMoney = 0;
        this.totalCount = 0;
        this.$message({
          message: "结账成功，感谢你又为店里出了一份力",
          type: "success"
        });
      } else {
        this.$message.error("不能空结账，老板了解");
      }
    },
    //模拟挂单结账
    guandankout() {
      if (this.guadancount != 0) {
        this.guadions = [];
        this.guadanMoney = 0;
        this.guadancount = 0;
        this.$message({
          message: "结账成功，感谢你又为店里出了一份力",
          type: "success"
        });
      } else {
        this.$message.error("不能空结账，老板了解");
      }
    },
    //挂单
    guadan() {
      //执行挂单操作,把点餐数组和价格赋值挂单数组
      if (this.tabledat != "") {
        this.guadions = [];
        //let obj = { key: this.tabledat };
        this.guadions.push(this.tabledat);
        this.tabledat = [];
        // console.log(this.totalMoney);
        // console.log(this.totalCount);

        this.guadanMoney = this.totalMoney;
        this.guadancount = this.totalCount;

        // console.log(this.guadanMoney);
        // console.log(this.guadancount);
        // console.log(typeof this.guadancount);
        this.totalMoney = 0;
        this.totalCount = 0;
      } else {
        this.$message.error("不能挂空单");
      }
    },

    //汇总数量和金钱
    getallmoney() {
      this.totalMoney = 0; //清0汇总数量
      this.totalCount = 0;
      if (this.tabledat) {
        this.tabledat.forEach(Element => {
          //console.log(this.totalCount);
          //console.log(Element);
          this.totalCount += Element.count;
          this.totalMoney = this.totalMoney + Element.price * Element.count;
        });
      }
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.pos-order {
  background: #f9fafc;
  border-right: 1px solid #c0ccda;
}
.div-btn {
  margin-top: 10px;
}
.title {
  height: 20px;
  border-bottom: 1px solid #d3dce6;
  background: #f9fafc;
  padding: 10px;
  text-align: left;
}
.ofetn-goud-list ul li {
  list-style: none;
  float: left;
  border: 1px solid #9cb4e2;
  padding: 10px;
  margin: 10px;
  background: #fff;
}
.o-pic {
  color: #58b7ff;
}
.goods-type {
  clear: both;
  margin-left: 10px;
  text-align: center;
}
.cookList li {
  list-style: none;
  width: 26%;
  border: 1px solid #e5e9f2;
  height: auot;
  overflow: hidden;
  background-color: #fff;
  padding: 2px;
  float: left;
  margin: 2px;
}
.cookList li span {
  display: block;
  float: left;
}
.foodImg {
  width: 40%;
}
.foodImg img {
  width: 100%;
}
.foodName {
  font-size: 15px;
  padding-left: 10px;
  color: brown;
}
.foodPrice {
  font-size: 16px;
  padding-left: 10px;
  padding-top: 10px;
}
</style>
