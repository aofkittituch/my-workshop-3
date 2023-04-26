<template>
  <div id="app">
    <div class="container">
      <h2>Workshop #4 - ตะกร้าสินค้า</h2>
      <hr />
      <div class="row">
        <div class="col-md-2" v-for="item in products">
          <div class="card h-100">
            <img :src="item.image" class="card-img-top img-fluid" />
            <div class="card-body">
              <h4 class="card-title">{{ item.name }}</h4>
              <p class="card-text">ราคา {{ item.price }} บาท</p>
            </div>
            <div class="card-footer">
              <button class="btn btn-primary" @click="addCart(item)">
                หยิบลงตะกร้า
              </button>
            </div>
          </div>
        </div>
        <div class="col-md-8" v-if="carts != 0">
          <h4>ตะกร้าสินค้า <i class="fa-solid fa-cart-shopping"></i></h4>
          <hr />
          <table class="table">
            <thead class="thead-light">
              <tr>
                <th scope="col">ภาพ</th>
                <th scope="col">ชื่อ</th>
                <th scope="col">ราคา</th>
                <th scope="col">จำนวน</th>
                <th scope="col">ยอดรวม</th>
                <th scope="col">ลบ</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="product in carts">
                <td>
                  <img :src="product.image" width="100" />
                </td>
                <td>{{ product.name }}</td>
                <td>{{ product.price }}</td>
                <td>
                  <i
                    class="fa-solid fa-minus qty-minus"
                    @click="minusQty(product)"
                  ></i>
                  {{ product.qty }}
                  <i
                    class="fa-solid fa-plus qty-plus"
                    @click="plusQty(product)"
                  ></i>
                </td>
                <td>{{ product.total }}</td>
                <td>
                  <button
                    class="btn btn-danger"
                    @click="removeProduct(product)"
                  >
                    <i class="fa-solid fa-trash"></i>
                  </button>
                </td>
              </tr>
            </tbody>
          </table>
          <h4 align="right">ยอดชำระเงินทั้งหมด {{ Total() }} บาท</h4>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      carts: [],
      coffee: 0,
      milkcoffee: 0,
      products: [
        {
          id: 1,
          name: "Latte",
          price: 50,
          image:
            "https://www.starbucks.co.th/stb-media/2020/08/42.ICED-LATTE1080.png",
          active: false,
        },
        {
          id: 2,
          name: "Americano",
          price: 40,
          image:
            "https://www.starbucks.co.th/stb-media/2020/08/40.Iced-Caffe-Americano1080.png",
          active: false,
        },
      ],
    };
  },
  methods: {
    addCart(item) {
      if (item.id == 1) {
        //buy coffee
        this.coffee += 1;
        if (this.coffee <= 1) {
          //buy 1
          this.pushData(item);
        } else {
          //buy 1 more
          var found = this.findIndex(item);
          this.carts[found].qty += 1;
          this.carts[found].total =
            this.carts[found].qty * this.carts[found].price;
        }
      } else {
        //buy milkcoffee
        this.milkcoffee += 1;
        if (this.milkcoffee <= 1) {
          //buy 1
          this.pushData(item);
        } else {
          //buy 1 more
          var found = this.findIndex(item);
          this.carts[found].qty += 1;
          this.carts[found].total =
            this.carts[found].qty * this.carts[found].price;
        }
      }
    },
    pushData(item) {
      this.carts.push({
        id: item.id,
        name: item.name,
        price: item.price,
        image: item.image,
        qty: 1,
        total: item.price,
      });
    },
    findIndex(item) {
      for (var i = 0; i < this.carts.length; i++) {
        if (this.carts[i].id == item.id) {
          return i; //สินค้าที่ค้นเจอในตะกร้า
        }
      }
      return -1;
    },
    minusQty(product) {
      product.qty -= 1;
      if (product.qty <= 1) {
        product.qty = 1;
      }
      product.total = product.price * product.qty;
    },
    plusQty(product) {
      product.qty += 1;
      product.total = product.price * product.qty;
    },
    removeProduct(product) {
      if (confirm("คุณต้องการลบหรือไม่ ?")) {
        var index = this.carts.indexOf(product);
        this.carts.splice(index, 1);
        if (product.id == 1) {
          this.coffee = 0;
        } else {
          this.milkcoffee = 0;
        }
      }
    },
    Total() {
      var sum = 0;
      this.carts.forEach((item) => {
        sum += item.total;
      });
      return sum;
    },
  },
};
</script>

<style scoped>
h2,
h3 {
  text-align: center;
  padding-top: 3rem;
}
.qty-minus {
  cursor: pointer;
  margin-right: 15px;
}
.qty-plus {
  cursor: pointer;
  margin-left: 15px;
}
</style>
