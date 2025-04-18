<script setup></script>

<template>
  <!-- 문의하기 전체 레이아웃 -->
  <div class="inquire-wrap">
    <div class="inquire-container inner">
      <!-- 문의하기 왼쪽 영역 -->
      <div class="inquire-left">
        <!-- 1-1. 문의하기 타이틀 -->
        <h2 class="inquire-title">문의하기</h2>
        <!-- 1-2. 문의하기 정보 -->
        <div class="inquire-info">
          <p class="inquire-text">
            <span>짐꾼은 항상 주인 곁에 있습니다.</span>
            <span>문의주시면 검토 후 빠른 답변 드리겠습니다.</span>
          </p>
          <div class="inquire-call">
            <div class="iq-telecom">
              <div class="img-box">
                <img src="/images/lee/telecom.png" alt="전화기" />
              </div>
              <p>
                <span>상담전화</span>
                <span>1234-5678 (평일 : 09:00 - 18:00)</span>
              </p>
            </div>
            <div class="iq-kakao">
              <div class="img-box">
                <img src="/images/kang/kakao.png" alt="카카오" />
              </div>
              <p>
                <span>카카오톡 문의</span>
                <span>24시간 연중무휴</span>
              </p>
            </div>
          </div>
        </div>
      </div>
      <!-- 2. 문의하기 오른쪽 영역 -->
      <div class="inquire-right">
        <form class="inquire-form">
        <!-- 2-1. 문의유형 -->
          <div class="inquire-group">
          <label for="category">1. 문의유형</label>
            <select name="category" id="category" required>
            <option value="">선택하세요</option>
            <option value="reservation">예약 관련</option>
            <option value="service">서비스 관련</option>
            <option value="payment">결제 관련</option>
            <option value="cancellation">취소/환불</option>
            <option value="etc">기타 문의</option>
          </select>
          </div>
          <!-- 2-2. 이름 -->
          <div class="inquire-group">
          <label for="name">2. 이름</label>
          <input type="text" id="name" required>
          </div>
          <!-- 2-3. 이메일 -->
          <div class="inquire-group">
          <label for="name">3. 이메일</label>
          <input type="email" id="name" required>
          </div>
          <!-- 2-4. 연락처 -->
          <div class="inquire-group">
          <label for="name">4. 연락처</label>
          <input type="phone" id="name" required>
          </div>
          <!-- 2-5. 문의내용 -->
          <div class="inquire-group">
          <label for="name">5. 문의내용</label>
          <textarea id="content" rows="4"></textarea>
          </div>
          <!-- 2-6. 첨부파일 -->
          <div class="inquire-group">
          <label for="name">6. 첨부파일</label>
          <input type="file" id="name" required>
          </div>
          <!-- 2-7. 개인정보 동의 -->
          <div class="inquire-group">
            <label class="checkbox-label">
            <span>개인정보 수집 및 이용 동의 안내</span>
            <span>동의함</span>
            <input  type="radio" name="agree" required />
            <span>동의안함</span>
            <input  type="radio" name="agree" required />
          </label>
          </div>
          <!-- 2-8. 문의하기 버튼 -->
          <button class="inquire-btn">문의하기</button>
        </form>
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
@import "/src/assets/variables";
.inner {
  max-width: 1240px;
  margin: 0 auto;
  padding: 0 20px;
}
.inquire-wrap {
  width: 100%;
  background-color: $sub-color;
  .inquire-container {
    width: 100%;
    display: flex;
    // margin-top: 100px;
    padding-top: 100px;
    // 1. 문의하기 왼쪽 영역
    .inquire-left {
      display: flex;
      flex: 1;
      flex-direction: column;
      align-items: center;
      gap: 60px;
      // 1-1. 문의하기 타이틀
      .inquire-title {
        width: 100%;
        font-size: $title-font-L + 4px;
        font-weight: bold;
        text-align: center;
      }
      // 1-2. 문의하기 정보
      .inquire-info {
        display: flex;
        flex-direction: column;
        gap: 60px;
        .inquire-text {
          display: flex;
          flex-direction: column;
          gap: 5px;
          span {
            font-size: $text-font-XL;
            font-weight: bold;
            color: $font-light-gray;
          }
        }
        .inquire-call {
          display: flex;
          flex-direction: column;
          // align-items: center;
          gap: 40px;
          .iq-telecom {
            display: flex;
            align-items: center;
            gap: 20px;
            .img-box {
              width: 50px;
              height: 50px;
              img {
                width: 100%;
                height: 100%;
              }
            }
            p {
              display: flex;
              flex-direction: column;
              gap: 8px;
              span {
                color: $font-light-gray;
                font-size: $text-font-L;
                font-weight: bold;
              }
            }
          }
          .iq-kakao {
            display: flex;
            align-items: center;
            gap: 20px;
            .img-box {
              width: 50px;
              height: 50px;
              img {
                width: 100%;
                height: 100%;
              }
            }
            p {
              display: flex;
              flex-direction: column;
              gap: 8px;
              span {
                color: $font-light-gray;
                font-size: $text-font-L;
                font-weight: bold;
              }
            }
          }
        }
      }
    }
    // 2.문의하기 오른쪽 영역
    .inquire-right {
      flex: 1;
    }
  }
}
</style>
