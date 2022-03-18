<template>
  <div class="app-container">
    <EsHeader title="購物車案例" />
    <EsGoods
      v-for="item in goodslist"
      :key="item.id"
      :id="item.id"
      :thumb="item.goods_img"
      :title="item.goods_name"
      :price="item.goods_price"
      :count="item.goods_count"
      :checked="item.goods_state"
      @stateChange="onGoodsStateChange"
      @countChange="onGoodsCountChange"
    />
    <EsFooter @fullChange="onFullStateChange" :amount="amount" :total="total" />
  </div>
</template>

<script>
import EsHeader from "./components/es-heaader/EsHeader.vue";
import EsFooter from "./components/es-footer/EsFooter.vue";
import EsGoods from "./components/es-goods/EsGoods.vue";
export default {
  name: "MyApp",
  data() {
    return {
      goodslist: [],
    };
  },
  created() {
    this.getGoodsList();
  },
  methods: {
    // 獲取商品列表數據的方法
    async getGoodsList() {
      const { data: res } = await this.$http.get("/api/cart");
      if (res.status !== 200) return alert("數據請求失敗!");
      this.goodslist = res.list;
    },
    // 監聽選中狀態變化的事件
    onFullStateChange(isFull) {
      // console.log(isFull);
      this.goodslist.forEach((x) => (x.goods_state = isFull));
    },
    // 監聽商品勾選狀態變化
    onGoodsStateChange(e) {
      const findResult = this.goodslist.find((x) => x.id === e.id);
      if (findResult) {
        findResult.goods_state = e.value;
      }
    },
    // 監聽商品數量變化
    onGoodsCountChange(e) {
      const findResult = this.goodslist.find((x) => x.id === e.id);
      if (findResult) {
        findResult.goods_count = e.value;
      }
    },
  },
  computed: {
    // 已勾選商品總價
    amount() {
      let a = 0;
      this.goodslist
        .filter((x) => x.goods_state)
        .forEach((x) => {
          a += x.goods_price * x.goods_count;
        });

      return a;
    },
    // 已勾選商品總數量
    total() {
      let t = 0;
      this.goodslist
        .filter((x) => x.goods_state)
        .forEach((x) => {
          t += x.goods_count;
        });
      return t;
    },
  },
  components: {
    EsHeader,
    EsFooter,
    EsGoods,
  },
};
</script>

<style lang="less" scoped>
.app-container {
  padding-top: 45px;
  padding-bottom: 50px;
}
</style>