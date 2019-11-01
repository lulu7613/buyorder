<template>
  <div id="app">
    <div class="container">
      <div class="pre-order" v-if="preOrder > 0">
        <p>預購數量 : {{ preOrder }} 件</p>
      </div>
      <div class="product-img"></div>
      <div class="product-detail">
        <h2 class="text border-primary" v-if="time >= 60">
          倒數時間:
          <span class="pointText">{{ time }} 秒</span>
        </h2>
        <h2 class="text border-orange" v-if="time > 0 && time < 60">
          倒數時間:
          <span class="pointText">{{ time }} 秒</span>
        </h2>
        <h2 class="text border-secondary" v-if="time === 0">
          倒數時間:
          <span class="pointText">{{ time }} 秒</span>
        </h2>

        <h1 class="text pointText bg-primary">凝視 T-Shirt</h1>
        <h3 class="text bg-primary">
          剩餘數量:
          <span class="pointText">{{ count }} 件</span>
        </h3>
        <button
          class="btn btn-block btn-outline-primary mx-auto"
          v-if="count > 0 && time > 0"
          @click="openModal(false)"
        >購買</button>
        <button
          class="btn btn-block btn-outline-primary mx-auto"
          v-if="count === 0 && time > 0"
          @click="openModal(true)"
        >預購</button>
        <button
        class="btn btn-block mx-auto disabled" disabled v-if="time === 0">停售</button>
      </div>

      <!-- modal -->
      <div class="modal" :style="show">
        <div class="modal-content bg-primary">
          <div class="modal-head">
            <div class="close-icon" @click="closeModal">
              <i class="fas fa-times"></i>
            </div>
            <h4 class="modal-title" v-if="count > 0 && time > 0">我要購買</h4>
            <h4 class="modal-title" v-else>我要預購</h4>
          </div>
          <div class="modal-body">
            <button class="btn count-action" @click="minus(1)">
              <i class="fas fa-minus fa-2x"></i>
            </button>
            <div class="count-number">{{ buy }}</div>
            <button class="btn count-action" @click="plus(1)">
              <i class="fas fa-plus fa-2x"></i>
            </button>
          </div>
          <div class="modal-footer mx-auto">
            <button class="btn btn-block btn-outline-primary mx-auto" @click="check()">確認</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'app',
  components: {
  },

  data() {
    return {
      time: 70, // 倒數計時
      count: 10, // 剩餘數量
      buy: 1, // 購買
      isPreOrder: false, // 是否是預購
      preOrder: 0, // 預購

      show: {
        bottom: '-450px',
      },
    };
  },

  watch: {
    time() {
      if (this.time === 0) {
        this.closeModal();
      }
    },
  },

  methods: {
    openModal(isPreOrder) {
      this.isPreOrder = isPreOrder;
      this.show.bottom = 0;
      this.buy = 1;
    },

    closeModal() {
      this.show.bottom = '-450px';
    },

    plus(num) {
      // 不是預購
      if (!this.isPreOrder) {
        if (this.buy < this.count) {
          this.buy = this.buy + num;
        }
      } else if (this.buy < 100) {
        this.buy = this.buy + num;
      }
    },

    minus(num) {
      if (this.buy > 0) {
        this.buy = this.buy - num;
      }
    },

    check() {
      this.closeModal();
      if (this.count > 0) {
        this.count = this.count - this.buy;
      } else {
        this.preOrder = this.preOrder + this.buy;
      }
    },
  },

  created() {
    // 倒數計時
    const starCountdown = window.setInterval(() => {
      this.time = this.time - 1;
      if (this.time === 0) {
        window.clearInterval(starCountdown);
      }
    }, 1000);
    return starCountdown;
  },
};
</script>

<style lang="scss">
@import "~@/assets/scss/all";
</style>
