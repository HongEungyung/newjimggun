<script setup>
import { ref, onMounted } from 'vue';
import StarRatingInput from '@/components/StarRatingInput.vue';
import { useRouter } from 'vue-router';
const router = useRouter();
const STORAGE_KEY = 'userReviews';
const NAME_KEY = 'userName';

const newReview = ref({
  name: '',
  title: '',
  content: '',
  rating: 0,
  images: [],
});

const reviews = ref([]);

// 1. mount 시 이름 로드
onMounted(() => {
  const savedName = localStorage.getItem(NAME_KEY);
  if (savedName) {
    newReview.value.name = savedName;
  }

  const savedReviews = localStorage.getItem(STORAGE_KEY);
  if (savedReviews) {
    reviews.value = JSON.parse(savedReviews);
  }
});

function submitReview() {
  if (
    !newReview.value.name ||
    !newReview.value.title ||
    !newReview.value.content ||
    newReview.value.rating === 0 ||
    newReview.value.images.length === 0
  ) {
    alert('모든 항목을 입력해 주세요! (이미지 1장 이상 필수)');
    return;
  }

  // 이름 저장
  localStorage.setItem(NAME_KEY, newReview.value.name);

  // 리뷰 저장
  reviews.value.push({ ...newReview.value });
  localStorage.setItem(STORAGE_KEY, JSON.stringify(reviews.value));

  // 입력 초기화 (단, 이름은 유지)
  const nameCache = newReview.value.name;
  newReview.value = {
    name: nameCache,
    title: '',
    content: '',
    rating: 0,
    images: [],
  };

  // 알림
  alert('리뷰가 등록되었습니다!');
  // 리뷰페이지로 이동
  router.push('/review');
}
//사진 등록
function handleImageUpload(event) {
  const files = event.target.files;
  if (!files.length) return;

  const totalImages = newReview.value.images.length + files.length;

  if (totalImages > 3) {
    alert('이미지는 최대 3장까지만 업로드할 수 있습니다.');
    return;
  }

  for (let i = 0; i < files.length; i++) {
    const reader = new FileReader();
    reader.onload = (e) => {
      newReview.value.images.push(e.target.result);
    };
    reader.readAsDataURL(files[i]);
  }
}
// 사진 개수 조건 체크
</script>
<template>
  <div class="review-form">
    <form @submit.prevent="submitReview">
      <input v-model="newReview.title" placeholder="제목" class="input-title" />
      <div class="name-rating-box">
        <input v-model="newReview.name" placeholder="이름" class="input-name" />
        <StarRatingInput v-model="newReview.rating" class="input-rating" />
      </div>

      <textarea v-model="newReview.content" placeholder="리뷰 내용을 입력해 주세요" class="input-content" />
      <input type="file" multiple accept="image/*" @change="handleImageUpload" />

      <div class="image-preview">
        <img v-for="(img, idx) in newReview.images" :key="idx" :src="img" alt="업로드된 이미지" class="preview-img" />
      </div>
      <button type="submit" class="submitBtn">리뷰 등록</button>
    </form>
  </div>
</template>
<style lang="scss" scoped>
@import '/src/assets/variables';
.review-form {
  max-width: 1240px;
  margin: auto;

  font-family: $font-family;
  // min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}
form {
  max-width: 600px;
  width: 100%;
  margin: 70px auto;
  display: flex;
  flex-direction: column;
  gap: 25px;
  align-items: center;

  // 인풋
  .input-title {
    max-width: 600px;
    width: 90%;
    font-size: 16px;
    padding: 11.5px 10px;
  }
  .name-rating-box {
    display: flex;
    gap: 30px;
    max-width: 600px;
    width: 90%;

    .input-name {
      width: 100%;
      font-size: 16px;
      padding: 11.5px 10px;
    }
    .input-rating {
      padding: 8.5px 8.5px;
    }
  }
  .input-content {
    max-width: 600px;
    width: 90%;
    height: 400px;
    padding: 11.5px 10px;
  }
  .submitBtn {
    max-width: 600px;
    width: 90%;
    font-size: 16px;
    padding: 11.5px 10px;
    background-color: $primary-color;
    color: $white;
    border: none;
    border-radius: 6px;
    cursor: pointer;
  }
  // 이미지 미리보기
  .image-preview {
    display: flex;
    gap: 10px;
    flex-wrap: wrap;

    .preview-img {
      width: 100px;
      height: 100px;
      object-fit: cover;
      border-radius: 6px;
      border: 1px solid $font-light-gray;
    }
  }
}
//반응형
@media screen and (max-width: 768px) {
  // gotop 버튼
  .topBtnWrap {
    display: none !important;
  }
}
@media screen and (max-width: 510px) {
  form {
    .name-rating-box {
      flex-direction: column;
      gap: 10px;
    }

    .input-name {
      width: 100%;
    }

    .input-rating {
      width: 100%;
      padding: 0px 8.5px;
    }

    .input-content {
      height: 250px;
    }
  }
}
</style>
