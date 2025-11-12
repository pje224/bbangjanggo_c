<template>
  <div class="complete">
    <div class="inner">
      <div class="complete_title">
        <h1>예약완료</h1>
        <img src="/images/pje/check_icon.png" alt="체크" />
      </div>
      <div class="alram">
        <p>빵은 저희에게 맡기고, 편하게 여행하세요!</p>
        <p>접수/픽업 시, <span>QR코드나 예약번호</span>를 꼭 지참해 주세요!</p>
      </div>
      <div class="reserve_complete">
        <div class="qr_number">
          <!-- QR이미지 -->
          <img src="/images/pje/qr_code.png" alt="예약큐알" />
          <p>예약번호 : 21323344</p>
        </div>
        <div class="reserve_info">
          <div class="name">
            <p>이름</p>
            <p>김빵장</p>
          </div>
          <div class="number">
            <p>전화번호</p>
            <p>010-1234-1234</p>
          </div>
          <div class="reserved_options_list" v-if="reservedOptions.length > 0">
            <p>선택한 옵션</p>
            <div class="selected_options">
              <div v-for="(option, index) in reservedOptions" :key="index" class="reserved_option">
                <h3>[{{ option.location }}] {{ option.temp }} / {{ option.method }}</h3>
                <p v-if="option.bakery">방문 빵집 : {{ option.bakery }}</p>
                <p>방문 일시 : {{ option.date }} [{{ option.time }}]</p>
                <p>부가서비스 : {{ option.service }}</p>
                <p class="quantity">수량 : {{ option.quantity }}</p>
              </div>
            </div>
          </div>
          <div class="price">
            <p>총 결제 금액</p>
            <div class="total_price">
              <p>{{ totalPrice.toLocaleString() }}원 <span>/4시간</span></p>
            </div>
          </div>
        </div>
      </div>
      <p class="alram">보관 시작ㆍ픽업 시간 임박시 푸시 알림을 보내드려요!</p>
      <div class="btns">
        <RouterLink to="/" class="move_btn">홈으로</RouterLink>
        <RouterLink to="/mypage" class="move_btn">예약 내역 확인</RouterLink>
      </div>
    </div>
  </div>
</template>
<script setup>
import { ref, onMounted, computed } from "vue";
import { useRouter } from "vue-router";

// 로컬스토리지에서 예약 정보 불러오기
const reservedOptions = ref([]);
onMounted(() => {
  const saved = localStorage.getItem("reservedOptions");
  if (saved) {
    reservedOptions.value = JSON.parse(saved);
  }
});

// 가격 정보 (같은 기준)
const prices = {
  무인: {
    냉보관: 3000,
    상온보관: 2500,
  },
  기사: {
    냉보관: 4000,
    상온보관: 3000,
  },
  부가서비스: {
    "아이스팩 +1,000": 1000,
    "보냉백 +5,000": 5000,
    "선택 안함": 0,
  },
};

// 개별 가격 계산 함수
const calculatePrice = (option) => {
  let price = 0;
  if (option.method === "직접 맡길게요") {
    price = option.temp === "냉보관" ? prices.무인.냉보관 : prices.무인.상온보관;
  } else if (option.method === "기사님께 맡길게요") {
    price = option.temp === "냉보관" ? prices.기사.냉보관 : prices.기사.상온보관;
  }
  price += prices.부가서비스[option.service] || 0;
  return price * option.quantity;
};

// 총 금액 계산
const totalPrice = computed(() => reservedOptions.value.reduce((sum, item) => sum + calculatePrice(item), 0));
</script>

<style lang="scss" scoped>
@use "../assets/variables" as *;
@use "../assets/btn" as *;

p {
  font-family: "SpokaHanSansNeo";
  color: $font-color;
  font-size: $notice-text-font;
}

.complete {
  background-color: $bg-color;
  padding-bottom: 50px;
  .complete_title {
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 50px 0 0;
    h1 {
      text-align: center;
      color: $point-color;
      font-family: "Cafe24Surround";
      font-size: 35px;
    }
    img {
      width: 35px;
      height: 35px;
    }
  }

  .reserve_complete {
    width: 100%;
    background-color: #fff;
    border-radius: 10px;
    box-shadow: 1px 1px 6px rgba(0, 0, 0, 0.1);
    padding: 30px;
    // margin-top: 20px;
    .qr_number {
      width: 40%;
      max-width: 180px;
      margin: auto;
      text-align: center;
      padding-bottom: 20px;
      img {
        width: 100%;
        display: block;
      }
    }
  }
  .btns {
    display: flex;
    gap: 20px;
    .move_btn {
      @include btn-style;
      display: block;
      text-align: center;
      width: 100%;
    }
  }
  @media (max-width: 520px) {
    .reserved_options_list {
      display: block;
    }
  }
  @media (max-width: 430px) {
    & {
      padding: 0 0 30px;
    }
    .complete_title {
      padding: 30px 0 0;
      h1 {
        font-size: 30px !important;
      }
      img {
        width: 30px;
        height: 30px;
      }
    }

    h2 {
      font-size: $f-a-q-text-font;
    }
    p {
      font-size: $mobile-notice-font;
    }
    .reserve_complete {
      padding: 25px;
    }
    .reserved_options_list {
      display: block;
      .reserved_option {
        h3 {
          font-size: $notice-text-font !important;
        }
      }
    }

    .total_price {
      p {
        font-size: 18px !important;
        span {
          font-size: $mobile-notice-font !important;
        }
      }
    }
  }
}
.reserve_info {
  border-top: 1px solid rgba(186, 142, 95, 0.3);
  padding-top: 20px;
  .name,
  .number {
    display: flex;
    justify-content: space-between;
    margin-bottom: 15px;
    font-weight: 500;
  }
}
// 선택한 옵션
.reserved_options_list {
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  > p {
    font-weight: 500;
  }
}
// 선택한 옵션

.selected_options {
  text-align: end;
  .reserved_option {
    width: 100%;
    padding: 15px 0 20px;
    position: relative;
    h3 {
      font-size: $desc-text-font;
      margin-bottom: 5px;
      color: $font-color;
    }

    p {
      font-size: $mobile-notice-font;
      color: #666;
    }
  }
}
.quantity {
  font-size: $mobile-notice-font;
  color: #666;
}
// 총 결제 금액
.price {
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-top: 1px solid rgba(186, 142, 95, 0.3);
  padding-top: 20px;

  .total_price {
    p {
      font-size: $f-a-q-text-font;
      font-weight: bold;
      color: $font-color;

      span {
        font-size: $notice-text-font;
        font-weight: normal;
        color: #949494;
      }
    }
  }
}
.alram {
  text-align: center;
  padding: 30px 0;
  span {
    font-weight: 600;
  }
}
</style>
