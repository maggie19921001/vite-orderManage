<template>
  <div id="app">
    <div class="container mt-5">
      <div class="row">
        <div class="col-md-4">
          <div class="list-group">
            <div v-for="item in products" :key="item.id">
              <a href="#" class="list-group-item list-group-item-action" @click.prevent="addToCart(item)">
                <div class="d-flex w-100 justify-content-between">
                  <h5 class="mb-1">{{ item.name }}</h5>
                  <small>${{ item.price }}</small>
                </div>
                <p class="mb-1">{{ item.description }}</p>
              </a>
            </div>
          </div>
        </div>
        <div class="col-md-8">
          <table class="table">
            <thead>
              <tr>
                <th scope="col" width="50">操作</th>
                <th scope="col">品項</th>
                <th scope="col">描述</th>
                <th scope="col" width="90">數量</th>
                <th scope="col">單價</th>
                <th scope="col">小計</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="item in cart" :key="item.id">
                <td><button type="button" class="btn btn-sm" @click="removeFromCart(item)">x</button></td>
                <td>{{ item.name }}</td>
                <td><small>{{ item.description }}</small></td>
                <td>
                  <select class="form-select" v-model="item.quantity" @change="updateTotal">
                    <option v-for="i in 10" :key="i" :value="i">{{ i }}</option>
                  </select>
                </td>
                <td>${{ item.price }}</td>
                <td>${{ item.price * item.quantity }}</td>
              </tr>
            </tbody>
          </table>
          <div class="text-end mb-3">
            <h5>總計: <span>${{ total }}</span></h5>
          </div>
          <textarea class="form-control mb-3" rows="3" placeholder="備註" v-model="note"></textarea>
          <div class="text-end">
            <button class="btn btn-primary" @click="placeOrder" :disabled="loading">
              <span v-if="loading">Loading...</span>
              <span v-else>送出</span>
            </button>
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
                  <tbody>
                    <tr v-for="item in order.items" :key="item.id">
                      <td>{{ item.name }}</td>
                      <td>{{ item.quantity }}</td>
                      <td>${{ item.price * item.quantity }}</td>
                    </tr>
                  </tbody>
                </table>
                <div class="text-end">備註: <span>{{ order.note }}</span></div>
                <div class="text-end">
                  <h5>總計: <span>${{ order.total }}</span></h5>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      products: [
        { id: 1, name: '珍珠奶茶', description: '香濃奶茶搭配QQ珍珠', price: 50, stock: 20 },
        { id: 2, name: '冬瓜檸檬', description: '清新冬瓜配上新鮮檸檬', price: 45, stock: 18 },
        { id: 3, name: '翡翠檸檬', description: '綠茶與檸檬的完美結合', price: 55, stock: 34 },
        { id: 4, name: '四季春茶', description: '香醇四季春茶，回甘無比', price: 45, stock: 10 },
        { id: 5, name: '阿薩姆奶茶', description: '阿薩姆紅茶搭配香醇鮮奶', price: 50, stock: 25 },
        { id: 6, name: '檸檬冰茶', description: '檸檬與冰茶的清新組合', price: 45, stock: 20 },
        { id: 7, name: '芒果綠茶', description: '芒果與綠茶的獨特風味', price: 55, stock: 18 },
        { id: 8, name: '抹茶拿鐵', description: '抹茶與鮮奶的絕配', price: 60, stock: 20 }
      ],
      cart: [],
      note: '',
      total: 0,
      loading: false,
      order: {
        items: [],
        note: '',
        total: 0
      }
    }
  },
  methods: {
    addToCart(item) {
      const existingItem = this.cart.find(i => i.id === item.id);
      if (existingItem) {
        existingItem.quantity++;
      } else {
        this.cart.push({ ...item, quantity: 1 });
      }
      this.updateTotal();
    },
    removeFromCart(item) {
      const index = this.cart.indexOf(item);
      this.cart.splice(index, 1);
      this.updateTotal();
    },
    updateTotal() {
      this.total = this.cart.reduce((total, item) => total + (item.price * item.quantity), 0);
    },
    placeOrder() {
      this.loading = true;
      setTimeout(() => {
        this.order.items = this.cart.map(item => ({
          id: item.id,
          name: item.name,
          quantity: item.quantity,
          price: item.price
        }));
        this.order.note = this.note;
        this.order.total = this.total;

        this.cart = [];
        this.note = '';
        this.total = 0;

        this.loading = false;
        alert('訂單送出成功!');
      }, 2000);
    }
  }
}
</script>