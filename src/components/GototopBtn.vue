<script setup>
import { ref, onMounted } from "vue";
import { useRouter } from "vue-router";

// gotop버튼
const router = useRouter();
const handleGoToReservation = () => {
  router.push("/reservation");
};
const smoothlyBtn = ref(null);
const topBtnWrap = ref(null);
const isFooterVisible = ref(false);

onMounted(() => {
  smoothlyBtn.value?.addEventListener("click", (e) => {
    e.preventDefault();
    window.scrollTo({
      top: 0,
      behavior: "smooth",
    });
  });

  // Intersection Observer 설정
  const observer = new IntersectionObserver(
    (entries) => {
      entries.forEach((entry) => {
        isFooterVisible.value = entry.isIntersecting;
      });
    },
    {
      threshold: 0.01,
      rootMargin: "-100px 0px 0px 0px",
    }
  );

  // 푸터 요소 관찰 시작
  const footer = document.querySelector("footer");
  if (footer) {
    observer.observe(footer);
  }
});
</script>
<template>
  <!-- gotop 버튼 -->
  <!-- <div class="topBtnWrap">
    <a href="#" class="topBtn" ref="smoothlyBtn">↑</a>
    <router-link to="isLoggedIn ? '/reservation' : '/login'" @click.prevent="handleGoToReservation" class="resBtn">
      <img src="/images/hong/gotopBtn-logo-w.png" alt="gotopBtn로고" />
      <p>고용하기</p>
    </router-link>
  </div> -->

<!-- 메인페이지 용 클릭 차단 -->
  <div class="topBtnWrap" ref="topBtnWrap" :class="{ 'footer-visible': isFooterVisible }">
    <a href="#" class="topBtn" ref="smoothlyBtn">↑</a>
    <div class="resBtn" style="cursor: pointer">
      <img src="/images/hong/gotopBtn-logo-w.png" alt="gotopBtn로고" />
      <span>고용하기</span>
    </div>
  </div>

<!-- 메인페이지 외 다른 페이지용들 -->
  <div class="topBtnWrap" ref="topBtnWrap" :class="{ 'footer-visible': isFooterVisible }">
    <a href="#" class="topBtn" ref="smoothlyBtn">↑</a>
    <div class="resBtn" style="cursor: pointer" @click.prevent="handleGoToReservation">
      <img src="/images/hong/gotopBtn-logo-w.png" alt="gotopBtn로고" />
      <span>고용하기</span>
    </div>
  </div>
  
  
</template>
<style lang="scss" scoped>
@import "/src/assets/variables";
// gotop 버튼
.topBtnWrap {
  position: fixed;
  right: 100px;
  bottom: 60px;
  z-index: 99999;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 10px;

  &.footer-visible {
    transform: translateY(-250px);
  }

  .topBtn {
    color: $primary-color;
    font-size: 40px;
    text-decoration: none;
    width: 70px;
    height: 70px;
    line-height: 70px;
    border-radius: 50%;
    background-color: $white;
    text-align: center;
    box-shadow: $info-boxShadow;
  }
  .resBtn {
    width: 70px;
    height: 70px;
    border-radius: 50%;
    background-color: $primary-color;
    text-align: center;
    box-shadow: $info-boxShadow;
    text-decoration: none;
    padding: 13.5px 0;
    span {
      display: inline-block;
      color: $white;
      font-size: 12px;
      margin-bottom: 2px;
    }
  }
}
</style>
