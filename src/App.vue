<template>
  <div id="app">
    <div class="container">
      <h2>Workshop #3 - คำนวณราคาสินค้า</h2>
      <hr />
      <div class="row">
        <div class="col-md-4" v-for="item in products">
          <div class="card h-100">
            <img :src="item.image" class="card-img-top img-fluid" />
            <div class="card-body">
              <h4 class="card-title">{{ item.name }}</h4>
              <h3 class="card-text">{{ item.price }} บาท</h3>
            </div>
            <div class="card-footer">
              <input
                type="checkbox"
                class="form-control"
                @change="Seleted(item)"
              />
            </div>
          </div>
        </div>
        <h3>ยอดชำระเงิน {{ Total() }} บาท</h3>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      products: [
        {
          id: 1,
          name: "Ice Latte",
          price: 50,
          image:
            "https://www.starbucks.co.th/stb-media/2020/08/42.ICED-LATTE1080.png",
          active: false,
        },
        {
          id: 2,
          name: "Ice Americano",
          price: 40,
          image:
            "https://www.starbucks.co.th/stb-media/2020/08/40.Iced-Caffe-Americano1080.png",
          active: false,
        },
        {
          id: 3,
          name: "Ice Mocca",
          price: 60,
          image:
            "https://www.starbucks.co.th/stb-media/2020/08/41.Iced-Caffe-Mocha1080.png",
          active: false,
        },
      ],
    };
  },
  methods: {
    Seleted(item) {
      item.active = !item.active;
    },
    Total() {
      var sum = 0;
      this.products.forEach((item) => {
        if (item.active) {
          sum += item.price;
        }
      });
      return sum;
    },
  },
};
</script>

<style>
h2,
h3 {
  text-align: center;
  padding-top: 3rem;
}
</style>
