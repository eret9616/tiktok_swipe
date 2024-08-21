<template>
  <!-- 通过纯css实现 -->
  <div class="page-container">
    <div class="inner-container">
      <div v-for="(page, index) in pages" :key="index" class="page">
        {{ page }}
      </div>
    </div>

    <!-- 前进按钮 -->
    <button class="floating-button next-button" @click="goNextPage">
      下一页
    </button>

    <!-- 后退按钮 -->
    <button class="floating-button prev-button" @click="goPrevPage">
      上一页
    </button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      currentPage: 0,
      pages: ["Page 1", "Page 2", "Page 3"], // 你可以用实际内容代替
    };
  },
  methods: {
    goNextPage() {
      if (this.currentPage < this.pages.length - 1) {
        this.currentPage++;
        this.scrollToCurrentPage();
      }
    },
    goPrevPage() {
      if (this.currentPage > 0) {
        this.currentPage--;
        this.scrollToCurrentPage();
      }
    },
    scrollToCurrentPage() {
      const container = this.$el.querySelector(".inner-container");
      const targetPage = container.children[this.currentPage];
      targetPage.scrollIntoView({ behavior: "smooth" });
    },
  },
};
</script>

<style scoped>
.page {
  height: 100vh;
  width: 100%;
  scroll-snap-align: start;
  box-sizing: border-box;
  border: 1px solid cyan;
}

.floating-button {
  position: absolute;
  z-index: 10;
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 5px;
}

.next-button {
  bottom: 20px;
  right: 20px;
}

.prev-button {
  bottom: 20px;
  left: 20px;
}

.page-container {
  position: relative;
  height: 100vh;
  overflow: hidden;
}

.inner-container {
  height: 100%;
  scroll-snap-type: y mandatory;
  overflow-y: scroll;
  scrollbar-width: none; /* Firefox */
}

/* 隐藏滚动条 */
.inner-container::-webkit-scrollbar {
  display: none; /* WebKit 浏览器 */
}

.page {
  height: 100vh;
  width: 100%;
  scroll-snap-align: start;
  box-sizing: border-box;
  border: 1px solid cyan;
}
</style>
