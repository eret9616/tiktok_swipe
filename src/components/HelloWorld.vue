<template>
  <div
    class="page-container"
    @touchstart="handleTouchStart"
    @touchmove="handleTouchMove"
    @touchend="handleTouchEnd"
    @mousedown="handleMouseStart"
    @mousemove="handleMouseMove"
    @mouseup="handleMouseEnd"
  >
    <div class="inner-container" :style="getContainerStyle()">
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
      startY: 0,
      deltaY: 0,
      pages: ["Page 1", "Page 2", "Page 3"], // 你可以用实际内容代替
      isDragging: false,
    };
  },
  methods: {
    handleTouchStart(event) {
      this.startY = event.touches[0].clientY;
      this.isDragging = true;
    },
    handleTouchMove(event) {
      if (this.isDragging) {
        const currentY = event.touches[0].clientY;
        this.deltaY = currentY - this.startY;
      }
    },
    handleTouchEnd() {
      this.handleEnd();
    },
    handleMouseStart(event) {
      this.startY = event.clientY;
      this.isDragging = true;
    },
    handleMouseMove(event) {
      if (this.isDragging) {
        const currentY = event.clientY;
        this.deltaY = currentY - this.startY;
      }
    },
    handleMouseEnd() {
      this.handleEnd();
    },
    handleEnd() {
      const threshold = 50; // 滑动阈值，单位像素

      this.isDragging = false;

      if (this.deltaY > threshold && this.currentPage > 0) {
        this.currentPage--;
      } else if (
        this.deltaY < -threshold &&
        this.currentPage < this.pages.length - 1
      ) {
        this.currentPage++;
      }

      this.deltaY = 0; // 重置deltaY
    },
    getContainerStyle() {
      const basePosition = -this.currentPage * 100;
      const translateY =
        basePosition +
        (this.isDragging ? (this.deltaY / window.innerHeight) * 100 : 0);

      return {
        transform: `translateY(${translateY}%)`,
        transition: this.isDragging ? "none" : "transform 0.3s ease",
      };
    },
    goNextPage() {
      if (this.currentPage < this.pages.length - 1) {
        this.currentPage++;
      }
    },
    goPrevPage() {
      if (this.currentPage > 0) {
        this.currentPage--;
      }
    },
  },
};
</script>

<style scoped>
.page {
  box-sizing: border-box;
  border: 1px solid cyan;
  box-sizing: border-box;
}
.page-container {
  position: relative;
  height: 100vh;
  overflow: hidden;
}

.inner-container {
  height: 100%;
}

.page {
  height: 100vh;
  width: 100%;
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
</style>
