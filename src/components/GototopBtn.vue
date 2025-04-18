<script setup>
import { computed } from 'vue'
import { useRouter } from 'vue-router'

// 예시: 로그인 여부 판단 방식 (실제 사용에 맞게 수정!)
const isLoggedIn = computed(() => {
  return localStorage.getItem('accessToken') !== null
})

const router = useRouter()

const handleGoToReservation = () => {
  if (isLoggedIn.value) {
    router.push('/reservation')
  } else {
    router.push('/login')
  }
}
</script>
<template>
    <!-- gotop 버튼 -->
    <div class="topBtnWrap">
        <a href="#" class="topBtn" ref="smoothlyBtn">↑</a>
        <router-link to="isLoggedIn ? '/reservation' : '/login'" @click.prevent="handleGoToReservation" class="resBtn">
          <img src="/images/hong/gotopBtn-logo-w.png" alt="gotopBtn로고" />
          <p>고용하기</p>
        </router-link>
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
    p {
      color: $white;
      font-size: 12px;
      margin-bottom: 2px;
    }
  }
}
</style>