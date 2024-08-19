<script setup>
import { computed, ref } from "vue";
import ProductList  from "/src/components/ProductList.vue";
import CartItems from "/src/components/CartItems.vue";

//產品及購物車商品
const products = ref([
    {id: 1,name: '珍珠奶茶',description: '香濃奶茶搭配QQ珍珠', price:50,stock:20},
    {id: 2,name: '冬瓜檸檬',description: '清新冬瓜配上新鮮檸檬',price: 45,stock: 18},
    {id: 3,name: '翡翠檸檬',description: '綠茶與檸檬的完美結合',price: 55,stock: 34},
    {id: 4,name: '四季春茶',description: '香醇四季春茶，回甘無比',price: 45,stock: 10},
    {id: 5,name: '阿薩姆奶茶',description: '阿薩姆紅茶搭配香醇鮮奶',price: 50,stock: 25},
    {id: 6,name: '檸檬冰茶',description: '檸檬與冰茶的清新組合',price: 45,stock: 20},
    {id: 7,name: '芒果綠茶',description: '芒果與綠茶的獨特風味',price: 55,stock: 18},
    {id: 8,name: '抹茶拿鐵',description: '抹茶與鮮奶的絕配',price: 60,stock: 20}
]);
const cartItems = ref([]);
const orderItems =ref({
  items: [],
  note: '',
  total: 0
});
const note =ref('');
//加入購物車+現有商品只增加數量
//如果購物車中已經有"該商品"，則數量＋，如沒有則push並增加數量屬性
const addToCart = (item)=>{
  const itemInCart = cartItems.value.find(ci=>ci.id===item.id);
  if(itemInCart){
    itemInCart.quantity++;
  }else{
    cartItems.value.push({...item,quantity:1});
  };
}
//計算總價
const countTotal = computed(()=>{
  let totalPay = 0;
  cartItems.value.forEach((item)=>{
    totalPay += item.price*item.quantity})
    return totalPay;
})
//刪除
const deleteCartItem = (item)=>{
  cartItems.value.splice(item.index,1);
}
//送出 order物品增加items物件及note、total屬性
const submitOrder =()=>{
  orderItems.value.items = cartItems.value.map(item =>({
    id:item.id,
    name: item.name,
    quantity: item.quantity,
    price: item.price
  }))
  orderItems.value.note = note.value;
  orderItems.value.total = cartItems.value.reduce((total, item) => total + (item.price * item.quantity), 0);
  
  cartItems.value = [];
  note.value = '';
}


</script>

<template>
<div id="root">
  <div class="container mt-5">
    <div class="row">
      <div class="col-md-4">
        <ProductList :products="products" @add-to-cart="addToCart"/>
      </div>
      <div class="col-md-8">
        <CartItems :cartItems="cartItems" @delete-cart-item="deleteCartItem" />
        <div class="text-end mb-3">
          <h5>總計: <span>${{countTotal}}</span></h5>
        </div>
        <textarea
            class="form-control mb-3"
            rows="3"
            placeholder="備註"
            v-model="note"
        ></textarea>
        <div class="text-end">
          <button class="btn btn-primary" @click="submitOrder">送出</button>
        </div>
      </div>
    </div>
    <hr />
    <div class="row justify-content-center">
      <div class="col-8">
        <div class="card">
          <div class="card-body">
            <div class="card-title">
              <h5>訂單</h5>
              <table class="table">
                <thead>
                  <tr>
                    <th scope="col">品項</th>
                    <th scope="col">數量</th>
                    <th scope="col">小計</th>
                  </tr>
                </thead>
                <tbody v-for="(order) in orderItems.items" :key="order.id">
                  <tr>
                    <td>{{order.name}}</td>
                    <td>{{order.quantity}}</td>
                    <td>{{order.quantity * order.price}}</td>
                  </tr>
                </tbody>
              </table>
              <div class="text-end">備註: <span>{{ orderItems.note }}</span></div>
              <div class="text-end">
                <h5>總計: <span>${{ orderItems.total }}</span></h5>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>
</template>
