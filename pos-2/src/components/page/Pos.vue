<template>
  <div class="pos">
    <el-row>
      <el-col :span="7" class="pos-order" id="order-list">
        <el-tabs>
          <el-tab-pane label="点单">
            <el-table :data="tableData" border style="width:100% ">
              <el-table-column
                prop="goodsName"
                label="商品名称"
              ></el-table-column>
              <el-table-column
                prop="count"
                label="数量"
                width="50"
              ></el-table-column>
              <el-table-column
                prop="price"
                label="金额"
                width="70"
              ></el-table-column>
              <el-table-column label="操作" width="100" fixed="right">
                <!-- slot-scope="scope" -->
                <template slot-scope="scope">
                  <el-button
                    type="text"
                    size="small"
                    @click="delSingleGoods(scope.row)"
                    >删除</el-button
                  >
                  <!--ELement 中规定需要怎么写 -->
                  <el-button
                    type="text"
                    size="small"
                    @click="addOrderList(scope.row)"
                    >添加</el-button
                  >
                </template>
              </el-table-column>
            </el-table>
            <div class="totalDiv">
              <p>
                数量： <span>{{ totalCount }}个</span>
              </p>
              <p>
                金额： <span>{{ totalMoney }}元</span>
              </p>
            </div>
            <div class="div-btn">
              <el-button type="warning">挂单</el-button>
              <el-button type="danger" @click="delAllGoods()">删除</el-button>
              <el-button type="success" @click="checkout()">结账</el-button>
            </div>
          </el-tab-pane>

          <el-tab-pane label="挂单"> </el-tab-pane>

          <el-tab-pane label="外卖"> </el-tab-pane>
        </el-tabs>
      </el-col>

      <el-col :span="17">
        <div class="often-goods">
          <div class="title">常用商品</div>
          <div class="often-goods-list">
            <ul>
              <li
                v-for="goods in oftenGoods"
                :key="goods"
                @click="addOrderList(goods)"
              >
                <sapn>{{ goods.goodsName }}</sapn>
                <span class="o-price">￥{{ goods.price }}元</span>
              </li>
            </ul>
          </div>
        </div>

        <div class="goods-type">
          <el-tabs>
            <el-tab-pane label="汉堡">
              <div>
                <ul class="cookList">
                  <li
                    v-for="item in type0Goods"
                    :key="item"
                    @click="addOrderList(item)"
                  >
                    <span class="foodImg"
                      ><img :src="item.goodsImg" width="100%"
                    /></span>
                    <span class="foodName">{{ item.goodsName }}</span>
                    <span class="foodPrice">￥{{ item.price }}元</span>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="小食">
              <div>
                <ul class="cookList">
                  <li
                    v-for="item in type1Goods"
                    :key="item"
                    @click="addOrderList(item)"
                  >
                    <span class="foodImg"
                      ><img :src="item.goodsImg" width="100%"
                    /></span>
                    <span class="foodName">{{ item.goodsName }}</span>
                    <span class="foodPrice">￥{{ item.price }}元</span>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="饮料">
              <div>
                <ul class="cookList">
                  <li
                    v-for="item in type2Goods"
                    :key="item"
                    @click="addOrderList(item)"
                  >
                    <span class="foodImg"
                      ><img :src="item.goodsImg" width="100%"
                    /></span>
                    <span class="foodName">{{ item.goodsName }}</span>
                    <span class="foodPrice">￥{{ item.price }}元</span>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="套餐">
              <div>
                <ul class="cookList">
                  <li
                    v-for="item in type3Goods"
                    :key="item"
                    @click="addOrderList(item)"
                  >
                    <span class="foodImg"
                      ><img :src="item.goodsImg" width="100%"
                    /></span>
                    <span class="foodName">{{ item.goodsName }}</span>
                    <span class="foodPrice">￥{{ item.price }}元</span>
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
import axios from "axios";
export default {
  name: "Pos",
  data() {
    return {
      tableData: [],
      oftenGoods: [],
      type0Goods: [],
      type1Goods: [],
      type2Goods: [],
      type3Goods: [],
      totalMoney: 0,
      totalCount: 0
    };
  },
  created: function() {
    //    常用商品数据
    axios
      .get(
        "https://www.easy-mock.com/mock/5b8b30dbf032f03c5e71de7f/kuaican/oftenGoods"
      )
      .then(response => {
        console.log(response);
        this.oftenGoods = response.data;
      })
      .catch(response => {
        console.log();
        alert("网络错误，不能访问");
      });
    //分类商品数据
    axios
      .get(
        "https://www.easy-mock.com/mock/5b8b30dbf032f03c5e71de7f/kuaican/typeGoods"
      )
      .then(response => {
        console.log(response);
        this.type0Goods = response.data[0];
        this.type1Goods = response.data[1];
        this.type2Goods = response.data[2];
        this.type3Goods = response.data[3];
      })
      .catch(response => {
        console.log();
        alert("网络错误，不能访问");
      });
  },
  mounted: function() {
    var orderHeiht = document.body.clientHeight;
    // console.log(orderHeiht);
    document.getElementById("order-list").style.height = orderHeiht + "px";
  },
  methods: {
    addOrderList(goods) {
      this.totalMoney = 0;
      this.totalCount = 0;

      // 商品是否存在于订单列表中
      let isHave = false;
      for (let i = 0; i < this.tableData.length; i++) {
        if (this.tableData[i].goodsId == goods.goodsId) {
          isHave = true;
        }
      }
      // 根据判断的值编写逻辑
      if (isHave) {
        // 改变列表中的商品数量
        let arr = this.tableData.filter(o => o.goodsId == goods.goodsId);
        arr[0].count++;
        console.log(arr);
      } else {
        let newGoods = {
          goodsId: goods.goodsId,
          goodsName: goods.goodsName,
          price: goods.price,
          count: 1
        };
        this.tableData.push(newGoods);
      }
      // 计算汇总中的金额和数量
      // this.tableData.forEach(element =>{
      //     this.totalCount +=element.count;
      //     this.totalMoney = this.totalMoney+(element.price*element.count)
      // })
      this.getAllMoney();
    },
    // 删除单个商品
    delSingleGoods(goods) {
      this.tableData = this.tableData.filter(o => o.goodsId != goods.goodsId);
      this.$message.error('您删除了'+goods.goodsName);
      this.getAllMoney();
    },
    //汇总数量金额
    getAllMoney() {
      this.totalMoney = 0;
      this.totalCount = 0;

      if (this.tableData) {
        this.tableData.forEach(element => {
          this.totalCount += element.count;
          this.totalMoney = this.totalMoney + element.price * element.count;
        });
      }
    },
    // 删除所有商品
    delAllGoods() {
      if (this.totalCount == 0) {
        this.$message.error("已经没有订单了，不要再点了");
      } else {
        this.$message({
          message: "订单已删除",
          type: "success"
        });
      };
      (this.tableData = []), (this.totalMoney = 0);
      this.totalCount = 0;
    },
    // 结账
    checkout() {
      if (this.totalCount != 0) {
        (this.tableData = []), (this.totalMoney = 0);
        this.totalCount = 0;
        this.$message({
          message: "感谢你又出了一份力",
          type: "success"
        });
      } else {
        this.$message.error("老板还没有选择哦！！！ 现在不能结账！！！");
      }
    }
  }
};
</script>

<style  scoped>
.pos-order {
  background-color: #f9fafc;
  border-right: 1px solid #c0ccda;
}
.div-btn {
  margin-top: 10px;
}
.title {
  height: 20px;
  border-bottom: 1px solid #3ddce6;
  background-color: #f9fafc;
  float: left;
  padding: 10px;
  width: 100%;
  text-align: left;
}
/* .often-goods{
    
} */
.often-goods-list ul li {
  list-style: none;
  float: left;
  border: 1px solid #e5e9f2;
  padding: 10px;
  margin: 10px;
  background-color: #fff;
  cursor: pointer;
}
.o-price {
  color: #58b7ff;
}
.goods-type {
  clear: both;
}
.cookList li {
  list-style: none;
  width: 23%;
  border: 1px solid #e5e9f2;
  height: auot;
  overflow: hidden;
  background-color: #fff;
  padding: 2px;
  float: left;
  margin: 2px;
  cursor: pointer;
}
.cookList li span {
  display: block;
  float: left;
}
.foodImg {
  width: 40%;
}
.foodName {
  font-size: 16px;
  padding-left: 10px;
  color: brown;
}
.foodPrice {
  font-size: 16px;
  padding-left: 10px;
  padding-top: 10px;
}
.totalDiv {
  background-color: #fff;
  padding: 10px;
  border-bottom: 1px solid #d3dce6;
}
</style>