<template>
  <view
    class="c-pagination shadow"
    hover-class="c-pagination-hover"
    :style="{top,left}"
    @tap="handleTap"
    @touchmove.stop.prevent="handdleMove($event)"
  >
    <text class="c-pagination-text">{{pageNum}}/{{Math.ceil(totalSize/(pageSize||1))||1}}</text>

    <view
      :style="{'animation-name':animate?'pagination-expand':'pagination-close'}"
      class="c-pagination-info shadow"
      :class="{'info-right':isRight, 'info-left':!isRight,}"
      v-if="isExpand"
    >
      <text class="c-pagination-info-text">共为您查询到{{totalSize}}条数据</text>
    </view>
  </view>
</template>

<script>
export default {
  props: {
    totalSize: {
      type: Number,
      default: 0
    },
    pageNum: {
      type: Number,
      default: 1
    },
    pageSize: {
      type: Number,
      default: 5
    }
  },
  data() {
    return {
      isExpand: false,
      animate: false,
      timer: null,
      top: "85%",
      left: "10%",
      isRight: true,
      screenWidth: 375
      // willClose:false,
    };
  },
  created() {
    this.screenWidth = uni.getSystemInfoSync().windowWidth;
    console.log(this.screenWidth);
  },
  computed: {
    isEnd() {
      return (
        this.pageNum >= (Math.ceil(this.totalSize / (this.pageSize || 1)) || 1)
      );
    }
  },
  watch: {
    isEnd: {
      handler(val) {
        if (val) {
          this.$emit("end");
          this.$emit("update:isEnd", true);
          console.log("This is the last page");
        } else {
          this.$emit("update:isEnd", false);
        }
      },
      totalSize(val) {
        if (val !== 0 && this.isEnd) {
          this.$emit("update:isEnd", true);
        }
      }
    }
  },
  methods: {
    handleTap() {
      if (this.timer) {
        return;
      }
      this.animate = !this.animate;
      if (this.isExpand) {
        this.timer = setTimeout(() => {
          (this.isExpand = false), (this.timer = null);
        }, 580);
      } else {
        this.isExpand = true;
      }
    },
    handdleMove(e) {
      // console.log(e)
      this.top = e.changedTouches[0].clientY - 30 + "px";
      this.left = e.changedTouches[0].clientx - 30 + "px";
      // console.log(this.left/this.screenWidth)
      this.isRight = (e.changedTouches[0].clientX - 30) / this.screenWidth <= 0.5;
    }
  }
};
</script>

<style>
.c-pagination {
  position: fixed;
  bottom: 60rpx;
  left: 60rpx;
  width: 80rpx;
  line-height: 80rpx;
  height: 80rpx;
  border-radius: 50%;
  background-color: #fff;
  color: #f56c6c;
  font-size: 24rpx;
  text-align: center;
  z-index: 1000;
}

.c-pagination-hover {
  background-color: #eee;
}

.c-pagination-text {
  position: relative;
  /* background-color: #fff; */
  z-index: 101;
}
.shadow {
  box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.1);
}
.c-pagination-info {
  position: absolute;
  /* z-index: -100; */
  /* 	top: 0;
		bottom: 0; */
  /* right: 0; */
  /* left: 100%; */
  background-color: #fff;
  font-size: 24rpx;
  color: #f56c6c;
  display: flex;
  justify-content: center;
  flex-direction: row;
  align-items: center;
  /* padding-left: 50%; */
  transition: all 1s ease;
  width: 400rpx;
  border-radius: 60rpx;
  top: 0;
  animation: pagination-expand 0.6s ease 1;
  /* 		animation: pagination-expand 1s ease 1; */
  overflow: hidden;
}

.info-right {
  left: 100%;
}

.info-left {
  right: 100%;
}

@keyframes pagination-expand {
  from {
    width: 0;
  }

  to {
  }
}

@keyframes pagination-close {
  from {
  }

  to {
    width: 0;
  }
}

/* 	@keyframes pagination {
		from {
			width: 0
		}

		to {
			width: 400rpx
		}
	} */

.c-pagination-info-text {
  padding: 0 20rpx;
  white-space: nowrap;
}
</style>
