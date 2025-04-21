<script setup>
import { ref, onMounted } from 'vue';
import StarRatingInput from '@/components/StarRatingInput.vue';
import { useRouter } from 'vue-router';
const router = useRouter();
const STORAGE_KEY = 'userReviews';
const NAME_KEY = 'userName';
import ErrorModal from '@/components/ErrorModal.vue';
const showErrorModal = ref(false);
const errorMessage = ref('');
const newReview = ref({
  name: '',
  title: '',
  content: '',
  rating: 0,
  images: [],
});

const reviews = ref([]);

//모달 닫으면 리뷰 페이지로 이동
function handleModalClose() {
  showErrorModal.value = false;
  router.push('/review');
}
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
// 알림
const handleReviewPost = () => {
  errorMessage.value = '리뷰가 등록 되었습니다.';
  showErrorModal.value = true;
};
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

  handleReviewPost();
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
  <ErrorModal v-if="showErrorModal" :message="errorMessage" @close="handleModalClose" />
  <div class="review-wrap">
    <div class="review-form">
      <div class="review-form-left">
        <h2 class="review-title">고객 후기</h2>
        <p>고객님의 소중한 후기를 남겨주세요.</p>
      </div>
      <div class="review-form-right">
        <form @submit.prevent="submitReview">
          <div class="post-group">
            <label for="name"><span>1. 제목</span></label>
            <input v-model="newReview.title" placeholder="제목" class="input-title" />
          </div>
          <div class="post-group">
            <label for="name"><span>2. 이름</span></label>
            <div class="name-rating-box">
              <input v-model="newReview.name" placeholder="이름" class="input-name" />
              <StarRatingInput v-model="newReview.rating" class="input-rating" />
            </div>
          </div>
          <div class="post-group">
            <label for="name"><span>3. 리뷰내용</span></label>
            <textarea v-model="newReview.content" placeholder="리뷰 내용을 입력해 주세요" class="input-content" />
          </div>
          <div class="post-group">
            <label for="name"><span>4. 이미지 업로드</span></label>
            <input type="file" multiple accept="image/*" @change="handleImageUpload" />
          </div>

          <div class="image-preview">
            <img
              v-for="(img, idx) in newReview.images"
              :key="idx"
              :src="img"
              alt="업로드된 이미지"
              class="preview-img"
            />
          </div>
          <button type="submit" class="submitBtn" @click="handleReviewPost">리뷰 등록</button>
        </form>
      </div>
    </div>
  </div>
</template>
<style lang="scss" scoped>
@import '/src/assets/variables';
.review-wrap {
  width: 100%;
  background-color: $sub-color;
}
.review-form {
  max-width: 1240px;
  margin: auto;
  font-family: $font-family;
  display: flex;
  padding-top: 100px;
  padding-bottom: 50px;
  @media screen and (max-width: 768px) {
    flex-direction: column;
    gap: 70px;
  }
}

//왼쪽
.review-form-left {
  display: flex;
  flex: 1;
  flex-direction: column;
  align-items: center;
  gap: 60px;
  // 고객 후기
  .review-title {
    width: 100%;
    font-size: $title-font-L + 4px;
    font-weight: bold;
    text-align: center;
  }
  p {
    font-size: $text-font-XL;
    font-weight: bold;
    color: $font-light-gray;
  }
}
//오른쪽
.review-form-right {
  width: 100%;
  position: relative;
  flex: 1;
  form {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    gap: 25px;
    // 인풋
    input {
      border-radius: 10px;
      outline: none;
      border: none;
      color: $font-light-gray;
      font-size: $text-font-S;
    }
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
      resize: none;
      font-family: inherit; // 부모 폰트 상속
    }
    .post-group {
      width: 100%;
      max-width: 500px;
      display: flex;
      flex-direction: column;
      gap: 5px;
      label {
        span {
          font-weight: bold;
          font-size: $text-font-M;
          color: $font-light-gray;
        }
      }
    }

    .submitBtn {
      max-width: 240px;
      width: 100%;
      font-size: 14px;
      font-weight: bold;
      padding: 6px 12px;
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
