<template>
  <div class="pos">
  	<el-row>
  		<el-col :span="7" class="pos_order" id="order-list">
  			<el-tabs>
  				<el-tab-pane label="点餐">
  					<el-table :data="tableData" border style="width:100%">
  						<el-table-column prop="goodsName" label="商品名称">
  							
  						</el-table-column>
  						<el-table-column prop="count" label="量" width="50">
  							
  						</el-table-column>
  						<el-table-column prop="price" label="金额" width="70">
  							
  						</el-table-column>
  						<el-table-column label="操作" width="100" fixed="right">
  							<template scope="scope">
  								<el-button type="text" size="small" @click="delSingleGoods(scope.row)">删除</el-button>
  								<el-button type="text" size="small" @click="addOrderList(scope.row)">增加</el-button>
  							</template>
  						</el-table-column>
  					</el-table>
  					<div class="totalDiv">
  						数量: {{totalCOunt}}    金额: {{totalMoney}}元
  					</div>
  					<div class="div-btn">
  						<el-button type="warning">挂单
  							
  						</el-button>
  						<el-button type="danger" @click="delAllGoods()">删除
  							
  						</el-button>
  						<el-button type="success" @click="checkout()">结账
  							
  						</el-button>
  					</div>
  				</el-tab-pane>
  				<el-tab-pane label="挂单">
  					挂单
  				</el-tab-pane>
  				<el-tab-pane label="外卖">
  					外卖
  				</el-tab-pane>
  			</el-tabs>
  		</el-col>
  		<el-col :span="17">
  			<div class="ofen-goods">
  				<div class="title">常用商品</div>
  				<div class="ofen-goods-list">
  					<ul>
  						<li v-for="goods in ofenGoods" @click="addOrderList(goods)">
  							<span>{{goods.goodsName}}</span>
  							<span class="o-price">￥{{goods.price}}元</span>
  						</li>
  					</ul>
  				</div>
  			</div>

  			<div class="goods-type">
  				<el-tabs>
  					<el-tab-pane label="汉堡">
  						<div>
  							<ul class="cookList">
  								<li v-for="typegood in type0Goods" @click="addOrderList(typegood)">
  									<span class="foodImg"><img :src="typegood.goodsImg" alt="" width="100%"></span>
  									<span class="foodName">{{typegood.goodsName}}</span>
  									<span class="foodPrice">￥{{typegood.price}}元</span>
  								</li>
  							</ul>
  						</div>
  					</el-tab-pane>
  					<el-tab-pane label="小食">
  						<div>
  							<ul class="cookList">
  								<li v-for="typegood in type1Goods" @click="addOrderList(typegood)">
  									<span class="foodImg"><img :src="typegood.goodsImg" alt="" width="100%"></span>
  									<span class="foodName">{{typegood.goodsName}}</span>
  									<span class="foodPrice">￥{{typegood.price}}元</span>
  								</li>
  							</ul>
  						</div>
  					</el-tab-pane>
  					<el-tab-pane label="饮料">
  						<div>
  							<ul class="cookList">
  								<li v-for="typegood in type2Goods" @click="addOrderList(typegood)">
  									<span class="foodImg"><img :src="typegood.goodsImg" alt="" width="100%"></span>
  									<span class="foodName">{{typegood.goodsName}}</span>
  									<span class="foodPrice">￥{{typegood.price}}元</span>
  								</li>
  							</ul>
  						</div>
  					</el-tab-pane>
  					<el-tab-pane label="套餐">
  						<div>
  							<ul class="cookList">
  								<li v-for="typegood in type3Goods" @click="addOrderList(typegood)">
  									<span class="foodImg"><img :src="typegood.goodsImg" alt="" width="100%"></span>
  									<span class="foodName">{{typegood.goodsName}}</span>
  									<span class="foodPrice">￥{{typegood.price}}元</span>
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
import axios from 'axios'
export default{
  name: 'pos',
  data(){
  	return {
  		tableData:[],
  		ofenGoods:[],
  			type0Goods:[],
  			type1Goods:[],
  			type2Goods:[],
  			type3Goods:[],
  			totalMoney:0,
  			totalCOunt:0
  	}
  },
  created: function(){
  	axios.get('http://jspang.com/DemoApi/oftenGoods.php')
  	.then(response=>{
  		// console.log(response);
  		this.ofenGoods = response.data;
  	})
  	.catch(error=>{
  		alert('网络错误')
  	})

  	axios.get('http://jspang.com/DemoApi/typeGoods.php')
  	.then(response=>{
  		// console.log(response);
  		this.type0Goods = response.data[0];
  		this.type1Goods = response.data[1];
  		this.type2Goods = response.data[2];
  		this.type3Goods = response.data[3];
  	})
  	.catch(error=>{
  		alert('网络错误')
  	})
  },
  mounted:function(){
  	var orderHeight = document.body.clientHeight;
  	console.log(orderHeight);
  	document.getElementById("order-list").style.height = orderHeight + 'px';
  },
  methods: {
  	addOrderList(goods){
  		this.totalMoney = 0;
  		this.totalCOunt = 0;
  		//商品是否已经存在于订单列表
  		let isHava = false;
  		for(let i=0;i<this.tableData.length;i++){
  			if(this.tableData[i].goodsId==goods.goodsId){
  				isHava = true;
  			}
  		}
  		//根据判断的值写一个业务逻辑
  		if(isHava){
  			//改变列表中商品数量
  			let arr = this.tableData.filter(o=>o.goodsId == goods.goodsId);
  			arr[0].count++;
  		}else{
  			let newGoods = {goodsId: goods.goodsId,goodsName: goods.goodsName,price:goods.price,count:1}
  			this.tableData.push(newGoods)
  		}
  		this.getAllMoney();
  		
  	},
  	//删除单个商品
  	delSingleGoods(goods){
  		this.tableData = this.tableData.filter(o=>o.goodsId != goods.goodsId);
  		this.getAllMoney();
  	},
  	delAllGoods(){
  		this.tableData = [];
  		this.totalCOunt = 0;
  		this.totalMoney = 0;
  	},
  	checkout(){
  		if(this.totalCOunt != 0){
  			this.tableData = [];
  			this.totalCOunt = 0;
  			this.totalMoney = 0;
  			this.$message({
  				message:'结账成功',
  				type:'success'
  			})
  		}else{
  			this.$message.error('不能空结')
  		}
  	},
  	//汇总数量和金额
  	getAllMoney(){
  		this.totalMoney=0;
  		this.totalCOunt=0;
  		if(this.tableData){
  			this.tableData.forEach((element)=>{
  			this.totalCOunt += element.count;
  			this.totalMoney = this.totalMoney+(element.price*element.count)
  			})
  		}
  	}
  }
}
</script>

<style>
	.pos_order{
		background: #f9fafc;
		border-right:  1px solid #c0ccda;
	}
	.div-btn{
		margin-top: 10px;
	}
	.title{
		height:  40px;
		border-bottom: 1px solid #D3dec6;
		background: #f9fafc;
		padding:  10px;
		text-align: left;
	}
	.ofen-goods-list ul li{
		list-style: none;
		float: left;
		border: 1px solid #E5E9F2;
		padding:  10px;
		margin: 10px;
		background: #fff;
	}
	.o-price{
		color:  #58b7ff;
	}
	.goods-type{
		clear: both;
	}
	.cookList li{
       list-style: none;
       width:23%;
       border:1px solid #E5E9F2;
       height: auot;
       overflow: hidden;
       background-color:#fff;
       padding: 2px;
       float:left;
       margin: 2px;
 
   }
   .cookList li span{
       
        display: block;
        float:left;
   }
   .foodImg{
       width: 40%;
   }
   .foodName{
       font-size: 18px;
       padding-left: 10px;
       color:brown;
 
   }
   .foodPrice{
       font-size: 16px;
       padding-left: 10px;
       padding-top:10px;
   }
   .totalDiv{
   	background: #fff;
   	padding:  10px;
   	border-bottom: 1px solid #0ff;
   }
</style>