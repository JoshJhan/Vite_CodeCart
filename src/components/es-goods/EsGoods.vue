<template>
  <div class="goods-container">
    <!-- 左側圖區域 -->
    <div class="left">
      <div class="form-check">
        <input
          class="form-check-input"
          type="checkbox"
          :id="id"
          :checked="checked"
          @change="onChangeBoxChange"
        />
        <label class="form-check-label" :for="id">
          <!-- 商品縮圖 -->
          <img :src="thumb" alt="商品圖片" class="thumb" />
        </label>
      </div>
    </div>

    <!-- 右側訊息區 -->
    <div class="right">
      <!-- 商品名稱 -->
      <div class="top">{{ title }}</div>
      <div class="bottom">
        <!-- 商品價格 -->
        <div class="price">NT. {{ price.toFixed(2) }}</div>
        <!-- 商品數量 -->
        <div class="count">
          <EsCounter :num="count" :min="1" @numChange="getNumber" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import EsCounter from "../es-counter/EsCounter.vue";

export default {
  name: "EsGoods",
  props: {
    // 商品的id
    id: {
      type: [String, Number],
      required: true,
    },
    // 商品圖片
    thumb: {
      type: String,
      required: true,
    },
    // 商品名稱
    title: {
      type: String,
      required: true,
    },
    // 商品價格
    price: {
      type: Number,
      required: true,
    },
    // 數量
    count: {
      type: Number,
      required: true,
    },
    // 選取狀態
    checked: {
      type: Boolean,
      required: true,
    },
  },
  emits: ["stateChange", "countChange"],
  methods: {
    onChangeBoxChange(e) {
      //   console.log(e.target.checked)
      this.$emit("stateChange", {
        id: this.id,
        value: e.target.checked,
      });
    },
    // 監聽數量變化
    getNumber(num) {
      //   console.log(num);
      this.$emit("countChange", {
        id: this.id,
        value: num,
      });
    },
  },
  components: {
    EsCounter,
  },
};
</script>

<style lang="less" scoped>
.goods-container {
  + .goods-container {
    border-top: 1px solid #efefef;
  }
  display: flex;
  padding: 10px;
  // 左側
  .left {
    margin-right: 10px;
    // 商品圖片
    .thumb {
      display: block;
      width: 100px;
      height: 100px;
      background-color: #efefef;
    }
  }
  .right {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    flex: 1;
    .top {
      font-weight: bold;
    }
    .bottom {
      display: flex;
      justify-content: space-between;
      align-items: center;
      .price {
        color: red;
        font-weight: bold;
      }
    }
  }
}

.form-check-input {
  margin-top: 3.4em;
}
</style>