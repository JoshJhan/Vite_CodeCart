<template>
  <div class="counter-container">
    <!-- 數量 -1 按鈕 -->
    <button type="button" class="btn btn-light btn-sm" @click="onSubClick">
      -
    </button>
    <!-- 輸入框 -->
    <input
      type="number"
      class="fomr-control fomr-control-sm ipt-num"
      v-model.number="number"
    />
    <!-- 數量 +1 按鈕 -->
    <button type="button" class="btn btn-light btn-sm" @click="onAddClick">
      +
    </button>
  </div>
</template>

<script>
export default {
  name: "EsCounter",
  props: {
    num: {
      type: Number,
      default: 0,
    },
    min: {
      type: Number,
      default: NaN,
    },
  },
  data() {
    return {
      number: this.num,
    };
  },
  emits: ["numChange"],
  watch: {
    number(newVal) {
      const parseResult = parseInt(newVal);

      if (isNaN(parseResult) || parseResult < 1) {
        this.number = 1;
        return;
      }

      if (String(newVal).indexOf(".") !== -1) {
        this.number = parseResult;
        return;
      }
      //   console.log(this.number)
      this.$emit("numChange", this.number);
    },
  },
  methods: {
    onSubClick() {
      if (!isNaN(this.min) && this.number - 1 < this.min) return;
      this.number--;
    },
    onAddClick() {
      this.number++;
    },
  },
};
</script>

<style lang="less" scoped>
.counter-container {
  display: flex;
  // 按鈕樣式
  .btn {
    width: 25px;
  }
  // 輸入框樣式
  .ipt-num {
    width: 44px;
    text-align: center;
    margin: 0 4px;
  }
}
</style>