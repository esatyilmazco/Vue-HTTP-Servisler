<template>
  <div class="container">
    <div class="row">
      <div class="col-md-6">
        <h3>Vue Resource ile HTTP Servisleri</h3>
        <hr>
        <div class="form-group">
          <input type="text" class="form-control" placeholder="Ürün Adı" v-model="productName">
          <br>
          <div class="buttons text-center">
            <button type="submit" class="btn btn-primary" @click="SaveUser()">Ekle</button>
            <button type="submit" class="btn btn-success" @click="GetByProduct()">Ürünleri getir</button>
          </div>
          <br>
          <div class="data text-success">
            <ul class="list-group list-unstyled">
              <li class="list-group-item" v-for="product in products">
                <strong> {{ product.dataProduct.productName }} </strong>
                <button class="btn btn-danger btn-sm" @click="DeleteProduct(product.key)" style="float: right;">Sil
                </button>
              </li>
            </ul>
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
      productName: null,
      products: []
    }
  },
  methods: {
    SaveUser() {
      this.$http.post("users.json", {productName: this.productName}).then(() => {
        alert(this.productName + " ürünü kaydedildi");
        this.productName = "";
      })

    },
    GetByProduct() {
      if (this.productName === ' ') {
        alert('Ürün bulunamadı,lütfen ürün ekleyiniz.');
      } else {
        this.$http.get("users.json").then((resp) => {
          let dataProduct = resp.data;
          for (let value in dataProduct) {
            this.products.push({
              key: value,
              dataProduct: dataProduct[value]
            });
          }
        })
      }


    },
    DeleteProduct(userArea) {
      this.$http.delete("users/" + userArea + ".json").then(() => {
        alert('Ürün Silindi');
      })
    }
  }
}
</script>

<style>
</style>
