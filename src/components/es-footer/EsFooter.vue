<template>
  <div class="footer-container">
    <!-- 全選 -->
    <div class="form-check">
      <input
        class="form-check-input"
        type="checkbox"
        id="fullCheck"
        :checked="isfull"
        @change="onCheckBoxChange"
      />
      <label class="form-check-label" for="fullCheck">全選</label>
    </div>

    <!-- 合計區 -->
    <div>
      <span>合計：</span>
      <span class="amount">${{ amount.toFixed(2) }}</span>
    </div>

    <!-- 結算按鈕 -->
    <button
      type="button"
      class="btn btn-primary btn-settle"
      :disabled="total === 0"
    >
      結帳 ( {{ total }} )
    </button>
  </div>
</template>

<script>
export default {
  name: "EsFooter",
  emits: ["fullChange"],
  props: {
    // 已勾選商品總價
    amount: {
      type: Number,
      default: 0,
    },
    // 已勾選商品總數
    total: {
      type: Number,
      default: 0,
    },
    // 全選狀態
    isfull: {
      type: Boolean,
      default: false,
    },
  },
  methods: {
    onCheckBoxChange(e) {
      this.$emit("fullChange", e.target.checked);
    },
  },
};
</script>

<style lang="less" scoped>
.footer-container {
  position: fixed;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 50px;
  background-color: #fff;
  border-top: 1px solid #efefef;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 10px;
}

.amount {
  font-weight: bold;
  color: red;
}

.btn-settle {
  min-width: 90px;
  height: 38px;
  border-radius: 19px;
}
</style>