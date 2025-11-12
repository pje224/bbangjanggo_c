<template>
  <div class="reservation">
    <div class="inner">
      <h1>예약하기</h1>
      <div class="components_wrap">
        <!-- 지점 선택 -->
        <div class="location">
          <h2>지점 선택</h2>
          <Location ref="locationRef" @location-selected="handleLocationSelect" :showPins="false" />
        </div>
        <!-- 날짜 선택 -->
        <div class="calendar">
          <h2>날짜 선택</h2>
          <Calender ref="calendarRef" @date-selected="handleDateSelect" />
        </div>
      </div>
      <div class="storage_extra">
        <!-- 보관 방법 -->
        <div class="storage_method">
          <div class="method_title">
            <h2>보관 방법</h2>
            <p>(중복 선택 가능)</p>
          </div>
          <div class="option_list">
            <!-- 온도 -->
            <div class="temp">
              <p>온도</p>
              <div class="temp_option">
                <p
                  class="opt_btn"
                  :class="{ clicked: isSelected('temp', '냉보관') }"
                  @click="toggleOption('temp', '냉보관')"
                >
                  냉보관
                </p>
                <p
                  class="opt_btn"
                  :class="{ clicked: isSelected('temp', '상온보관') }"
                  @click="toggleOption('temp', '상온보관')"
                >
                  상온보관
                </p>
              </div>
            </div>
            <!-- 접수 방법 -->
            <div class="method">
              <p>접수 방법</p>
              <div class="method_option">
                <p
                  class="opt_btn"
                  :class="{ clicked: isSelected('method', '직접 맡길게요') }"
                  @click="toggleOption('method', '직접 맡길게요')"
                >
                  직접 맡길게요
                </p>
                <p
                  class="opt_btn"
                  :class="{ clicked: isSelected('method', '기사님께 맡길게요') }"
                  @click="toggleOption('method', '기사님께 맡길게요')"
                >
                  기사님께 맡길게요
                </p>
              </div>
            </div>
          </div>
        </div>
        <!-- 부가 서비스 -->
        <div class="extra_service">
          <h2>부가서비스</h2>
          <p>부가서비스</p>
          <div class="service_option">
            <p
              class="opt_btn"
              :class="{ clicked: isSelected('service', '선택 안함') }"
              @click="toggleOption('service', '선택 안함')"
            >
              선택 안함
            </p>
            <p
              class="opt_btn"
              :class="{ clicked: isSelected('service', '아이스팩 +1,000') }"
              @click="toggleOption('service', '아이스팩 +1,000')"
            >
              아이스팩 +1,000
            </p>
            <p
              class="opt_btn"
              :class="{ clicked: isSelected('service', '보냉백 +5,000') }"
              @click="toggleOption('service', '보냉백 +5,000')"
            >
              보냉백 +5,000
            </p>
          </div>
        </div>
      </div>

      <!-- 기사 보관 (기사님께 맡길게요 선택시만 표시) -->
      <div class="delivery" v-if="selectedMethod === '기사님께 맡길게요'">
        <h2>기사님께 맡길게요</h2>
        <div class="bakery_selection">
          <p class="store">방문 베이커리</p>
          <div class="custom-select" @click="toggleBakeryDropdown">
            <div class="select-trigger" :class="{ open: isBakeryDropdownOpen }">
              <div v-if="selectedBakeryName">
                <div class="label">
                  <p>{{ selectedBakeryName }}</p>
                  <span>{{ selectedBakeryAddress }}</span>
                </div>
              </div>
              <div v-else>
                <div class="label placeholder">방문할 베이커리를 선택해 주세요</div>
              </div>
            </div>
            <div class="select-options" :class="{ open: isBakeryDropdownOpen }">
              <div class="option" @click.stop="selectBakery('소베', '소베 (대구 중구 중앙대로79길 6 1층)')">
                <p>소베</p>
                <span>대구 중구 중앙대로79길 6 1층</span>
              </div>
              <div
                class="option"
                @click.stop="
                  selectBakery('따끈따끈베이커리', '따끈따끈베이커리 (대구 중구 중앙대로 395 1F 따끈따끈베이커리)')
                "
              >
                <p>따끈따끈베이커리</p>
                <span>대구 중구 중앙대로 395 1F 따끈따끈베이커리</span>
              </div>
              <div class="option" @click.stop="selectBakery('공주당', '공주당 (대구 중구 동성로 6-2)')">
                <p>공주당</p>
                <span>대구 중구 동성로 6-2</span>
              </div>
              <div class="option" @click.stop="selectBakery('네쥬', '네쥬 (대구 중구 남성로 55 neige 네쥬)')">
                <p>네쥬</p>
                <span>대구 중구 남성로 55 neige 네쥬</span>
              </div>
              <div class="option" @click.stop="selectBakery('윈드윈', '윈드윈 (대구 중구 달구벌대로405길 42 1층)')">
                <p>윈드윈</p>
                <span>대구 중구 달구벌대로405길 42 1층</span>
              </div>
              <div class="option" @click.stop="selectBakery('따따따', '따따따 (대구 중구 동덕로36길 127 1층)')">
                <p>따따따</p>
                <span>대구 중구 동덕로36길 127 1층</span>
              </div>
              <div class="option" @click.stop="selectBakery('고려베이커리', '고려베이커리 (대구 남구 자유6길 45-2)')">
                <p>고려베이커리</p>
                <span>대구 남구 자유6길 45-2</span>
              </div>
              <div class="option" @click.stop="selectBakery('르배', '르배 (대구 수성구 화랑로8길 11-11 1층)')">
                <p>르배</p>
                <span>대구 수성구 화랑로8길 11-11 1층</span>
              </div>
              <div
                class="option"
                @click.stop="selectBakery('화이트리에 성서점', '화이트리에 성서점 (대구 달서구 성서로 420 1층 106호)')"
              >
                <p>화이트리에 성서점</p>
                <span>대구 달서구 성서로 420 1층 106호</span>
              </div>
              <div class="option" @click.stop="selectBakery('보밀당', '보밀당 (대구 달서구 상화로7길 38 1층)')">
                <p>보밀당</p>
                <span>대구 달서구 상화로7길 38 1층</span>
              </div>
            </div>
          </div>
        </div>
        <div class="time_selection">
          <p class="store">방문 시간 (기본 4시간 적용)</p>
          <swiper
            class="timeSwiper"
            :slides-per-view="6.7"
            :space-between="10"
            :breakpoints="{
              320: { slidesPerView: 2.3, spaceBetween: 8 },
              // 380: { slidesPerView: 3.3, spaceBetween: 8 },
              650: { slidesPerView: 3.3, spaceBetween: 8 },
              768: { slidesPerView: 4.3, spaceBetween: 8 },
              795: { slidesPerView: 5.3, spaceBetween: 10 },
              1020: { slidesPerView: 6.7, spaceBetween: 10 },
            }"
          >
            <swiper-slide
              v-for="time in timeSlots"
              :key="time"
              class="opt_btn"
              :class="{ clicked: isSelected('time', time), highlight: isHighlightTime(time) }"
              @click="toggleOption('time', time)"
            >
              {{ time }}
            </swiper-slide>
          </swiper>
          <p>*Bread Time 할인 적용 (11시 , 15시)</p>
        </div>
      </div>

      <!-- 직접 보관 (직접 맡길게요 선택시만 표시) -->
      <div class="in_person" v-if="selectedMethod === '직접 맡길게요'">
        <h2>직접 맡길게요</h2>
        <p class="store">이용 가능 락커 (기본 4시간 적용)</p>
        <div class="locker_selection opt_btn clicked">
          <p>락커 256<span>/300</span></p>
        </div>
        <p class="store locker_notice">락커 사이즈 : 35*35*55cm (종이백 2개 / 케이크(1단) 1박스 보관 가능)</p>
      </div>

      <!-- 선택한 옵션 목록 (옵션이 있을 때만 표시) -->
      <div class="reserved_options_list">
        <h2 v-if="reservedOptions.length > 0">선택한 옵션</h2>
        <div v-for="(option, index) in reservedOptions" :key="index" class="reserved_option">
          <button class="remove_btn" @click="removeOption(index)">✕</button>
          <div class="selected_options">
            <h3>[{{ option.location }}] {{ option.temp }} / {{ option.method }}</h3>
            <p v-if="option.bakery">방문 빵집 : {{ option.bakery }}</p>
            <p>방문 일시 : {{ option.date }} [{{ option.time }}]</p>
            <p>부가서비스 : {{ option.service }}</p>
          </div>

          <div class="total_price">
            <div class="quantity">
              <button @click="changeQuantity(index, -1)">-</button>
              <input v-model="option.quantity" readonly />
              <button @click="changeQuantity(index, 1)">+</button>
            </div>
            <div class="price">
              <p>{{ calculatePrice(option).toLocaleString() }}원 <span>/4시간</span></p>
            </div>
          </div>
          <button class="remove_btn" @click="removeOption(index)">✕</button>
        </div>
        <button class="reserve_btn" :class="{ disabled: reservedOptions.length === 0 }" @click="goPayment">
          예약하기
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { onMounted, ref, computed, watch } from "vue";
import { Swiper, SwiperSlide } from "swiper/vue";
import { useRouter } from "vue-router";
import Location from "@/components/main/Location.vue";
import Calender from "@/components/Calender.vue";

const router = useRouter();

// ref로 자식 컴포넌트 접근
const locationRef = ref(null);
const calendarRef = ref(null);

// 시간대 목록
const timeSlots = ref([
  "11:00 ~ 11:30",
  "11:30 ~ 12:00",
  "12:00 ~ 12:30",
  "12:30 ~ 13:00",
  "13:00 ~ 13:30",
  "13:30 ~ 14:00",
  "14:00 ~ 14:30",
  "14:30 ~ 15:00",
  "15:00 ~ 15:30",
  "15:30 ~ 16:00",
  "16:00 ~ 16:30",
  "16:30 ~ 17:00",
  "17:00 ~ 17:30",
  "17:30 ~ 18:00",
  "18:00 ~ 18:30",
  "18:30 ~ 19:00",
  "19:00 ~ 19:30",
  "19:30 ~ 20:00",
]);

// 특정 시간대인지 확인하는 함수
const isHighlightTime = (time) => {
  return time === "11:00 ~ 11:30" || time === "15:00 ~ 15:30";
};

// 현재 선택 중인 옵션 (임시)
const selectedTemp = ref("");
const selectedMethod = ref("");
const selectedService = ref("");
const selectedTime = ref("");
const selectedLocation = ref({ name: "", address: "" });
const selectedDate = ref("");
const selectedBakeryName = ref("");
const selectedBakeryAddress = ref("");

// 확정된 옵션 목록
const reservedOptions = ref([]);

// 옵션 토글 함수
const toggleOption = (category, value) => {
  if (category === "temp") {
    selectedTemp.value = value;
  } else if (category === "method") {
    selectedMethod.value = value;
    // 접수 방법 변경시 관련 선택 초기화
    selectedTime.value = "";

    selectedBakeryName.value = "";
    selectedBakeryAddress.value = "";
  } else if (category === "service") {
    selectedService.value = value;
  } else if (category === "time") {
    selectedTime.value = value;
  }
};

// 선택 여부 확인 함수
const isSelected = (category, value) => {
  if (category === "temp") return selectedTemp.value === value;
  if (category === "method") return selectedMethod.value === value;
  if (category === "service") return selectedService.value === value;
  if (category === "time") return selectedTime.value === value;
  return false;
};

// 모든 필수 항목이 선택되었는지 확인
const isAllRequiredSelected = computed(() => {
  const basicRequired =
    selectedLocation.value.name &&
    selectedDate.value &&
    selectedTemp.value &&
    selectedMethod.value &&
    selectedService.value;

  // 기사님께 맡길게요를 선택한 경우 베이커리, 시간 필수
  if (selectedMethod.value === "기사님께 맡길게요") {
    return basicRequired && selectedBakeryName.value && selectedTime.value;
  }

  return basicRequired;
});

// 모든 필수 항목이 선택되면 자동으로 옵션 추가 (순서 무관)
watch(
  [selectedLocation, selectedDate, selectedTemp, selectedMethod, selectedService, selectedTime, selectedBakeryName],
  () => {
    if (isAllRequiredSelected.value) {
      addOption();
    }
  },
  { deep: true }
);

// 기사 선택 시 베이커리,시간 선택 후에도 체크
watch(selectedTime, (newTime) => {
  if (newTime && selectedMethod.value === "기사님께 맡길게요" && isAllRequiredSelected.value) {
    addOption();
  }
});
watch(selectedBakeryName, (newBakery) => {
  if (newBakery && selectedMethod.value === "기사님께 맡길게요" && isAllRequiredSelected.value) {
    addOption();
  }
});

// 옵션 추가 함수
const addOption = () => {
  const newOption = {
    location: selectedLocation.value.name,
    temp: selectedTemp.value,
    method: selectedMethod.value,
    date: selectedDate.value,
    time: selectedMethod.value === "직접 맡길게요" ? "자율" : selectedTime.value,
    bakery: selectedBakeryName.value || "",
    service: selectedService.value,
    quantity: 1,
  };

  reservedOptions.value.push(newOption);

  // ✅ 기존 로컬 데이터 불러와 누적 저장
  const saved = localStorage.getItem("reservedOptions");
  const existing = saved ? JSON.parse(saved) : [];
  existing.push(newOption);

  // ✅ 로컬스토리지에 누적 저장
  localStorage.setItem("reservedOptions", JSON.stringify(existing));

  // ✅ 화면에서도 즉시 반영
  reservedOptions.value = existing;

  // 선택 초기화
  resetCurrentSelection();

  console.log("✅ 예약 목록:", reservedOptions.value);

  // 선택 초기화
  resetCurrentSelection();
};

// 현재 선택 초기화
const resetCurrentSelection = () => {
  // console.log('초기화 시작');

  selectedTemp.value = "";
  selectedMethod.value = "";
  selectedService.value = "";
  selectedTime.value = "";
  selectedLocation.value = { name: "", address: "" };
  selectedDate.value = "";
  selectedBakeryName.value = "";
  selectedBakeryAddress.value = "";

  // 자식 컴포넌트 초기화
  if (locationRef.value) {
    locationRef.value.reset();
  }
  if (calendarRef.value) {
    calendarRef.value.reset();
  }
};

// 옵션 삭제
const removeOption = (index) => {
  reservedOptions.value.splice(index, 1);
  localStorage.setItem("reservedOptions", JSON.stringify(reservedOptions.value));
};

// 수량 변경
const changeQuantity = (index, amount) => {
  const newQuantity = reservedOptions.value[index].quantity + amount;
  if (newQuantity > 0 && newQuantity <= 5) {
    reservedOptions.value[index].quantity = newQuantity;
    localStorage.setItem("reservedOptions", JSON.stringify(reservedOptions.value));
  }
};

// 베이커리 선택
const selectBakery = (value, text) => {
  const match = text.match(/^(.*) \((.*)\)$/);
  if (match) {
    selectedBakeryName.value = match[1];
    selectedBakeryAddress.value = match[2];
  } else {
    selectedBakeryName.value = text;
    selectedBakeryAddress.value = "";
  }
  isBakeryDropdownOpen.value = false;
};

const isBakeryDropdownOpen = ref(false);

const toggleBakeryDropdown = () => {
  isBakeryDropdownOpen.value = !isBakeryDropdownOpen.value;
};

// 지점 선택 핸들러
const handleLocationSelect = (location) => {
  selectedLocation.value = { ...location };
};

// 날짜 선택 핸들러
const handleDateSelect = (date) => {
  selectedDate.value = date;
};

// 가격 정보
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

// 개별 옵션 가격 계산
const calculatePrice = (option) => {
  let price = 0;

  if (option.method === "직접 맡길게요") {
    if (option.temp === "냉보관") {
      price = prices.무인.냉보관;
    } else if (option.temp === "상온보관") {
      price = prices.무인.상온보관;
    }
  } else if (option.method === "기사님께 맡길게요") {
    if (option.temp === "냉보관") {
      price = prices.기사.냉보관;
    } else if (option.temp === "상온보관") {
      price = prices.기사.상온보관;
    }
  }

  price += prices.부가서비스[option.service] || 0;
  price *= option.quantity;

  return price;
};

// ✅ 결제 페이지로 이동 + 데이터 전달
const goToPayment = () => {
  // ✅ 결제 전에 로컬스토리지에 저장
  localStorage.setItem("reservedOptions", JSON.stringify(reservedOptions.value));

  // ✅ 결제 페이지로 이동
  router.push({ name: "payment" });
};
onMounted(() => {
  // 결제 완료 후 돌아온 경우 → 예약 초기화
  localStorage.removeItem("paymentDone");
  localStorage.removeItem("reservedOptions");
  reservedOptions.value = [];
  return;
});

// 예약하기 클릭시
const goPayment = () => {
  if (reservedOptions.value.length === 0) {
    alert("예약 옵션을 모두 선택해주세요");
    return;
  }
  reservedOptions.value.length === 0;
  if (confirm("이대로 예약을 진행하시겠습니까?")) {
    router.push("/payment");
  }
};
</script>

<style lang="scss" scoped>
@use "../assets/variables" as *;
@use "../assets/btn" as *;

h2 {
  font-size: $sub-font;
  font-family: "SpokaHanSansNeo";
  color: $font-color;
  padding: 0 0 20px;
}
p {
  font-family: "SpokaHanSansNeo";
}

// 전체 레이아웃
.reservation {
  background-color: $bg-color;
  padding-bottom: 50px;
  h1 {
    text-align: center;
    color: $point-color;
    font-family: "Cafe24Surround";
    font-size: 35px;
    padding: 50px 0;
  }
  // @media (max-width: 875px) {
  //   .extra_service {
  //     > p {
  //       display: none;
  //       visibility: hidden;
  //     }
  //   }
  // }
  @media (max-width: 940px) {
    .components_wrap {
      flex-direction: column;
      align-items: center; // 중앙 정렬
      gap: 40px;

      .location,
      .calendar {
        flex: none; // 기존 flex-basis 무시
        width: 100%; // 부모 폭 가득
        // max-width: 500px;
      }
    }
  }
  @media (max-width: 614px) {
    .components_wrap {
      gap: 40px;
    }
  }
  @media (max-width: 537px) {
    .storage_extra {
      flex-direction: column;
    }
    .option_list {
      width: 100%;
      flex-direction: column;
      // gap: 20px;

      .temp_option,
      .method_option {
        display: flex;
        flex-wrap: wrap;
        justify-content: center;
        gap: 6px;

        .opt_btn {
          flex: 1 1 45%;
          text-align: center;
          padding: 12px 0;
          border-radius: 10px;
          font-size: $notice-text-font;
          min-width: 130px;
          // max-width: 160px;
        }
      }
    }

    .extra_service {
      width: 100%;
      justify-content: center;
      // > p {
      //   display: none;
      //   visibility: hidden;
      // }
      .service_option {
        display: flex;
        // flex-wrap: wrap;
        justify-content: center;
        gap: 6px;

        .opt_btn {
          flex: 1;
          text-align: center;
          padding: 12px 0;
          border-radius: 10px;
          font-size: $notice-text-font;
          // min-width: 130px;
          // max-width: 160px;
        }
      }
    }
  }
  @media (max-width: 430px) {
    & {
      padding: 0 0 30px !important;
    }
    h1 {
      font-size: 30px;
      padding: 30px 0;
    }
    h2 {
      font-size: $f-a-q-text-font;
    }
    p {
      font-size: $notice-text-font;
    }
    .method_title {
      p {
        font-size: $notice-text-font;
        font-weight: 500;
      }
    }
    .delivery,
    .in_person {
      padding: 25px;
    }
    .store {
      font-size: $notice-text-font !important;
    }
    .timeSwiper {
      font-size: $notice-text-font;
    }
    .locker_notice {
      font-size: $mobile-notice-font !important;
    }
    .select-trigger {
      padding: 8px 12px !important;
      &::after {
        width: 10px !important;
        height: 10px !important;
        right: 14px !important;
        margin-top: -4px !important;
      }
      .label {
        gap: 2px !important;
        font-size: $notice-text-font !important;
      }
    }
    .select-options {
      .option {
        padding: 8px 12px !important;
      }
    }
    .reserve_btn {
      margin-top: 30px;
    }
  }
}

// 컴포넌트 (지점 선택 / 달력)
.components_wrap {
  width: 100%;
  display: flex;
  justify-content: space-between;
  flex-wrap: wrap;
  gap: 15px;
  .location,
  .calendar {
    flex: 1 1 45%; /* basis를 45%로 주면 여유 생김 */
    min-width: 300px; /* 이 폭 이하로 줄어들면 wrap 작동 */
  }
  .calendar {
    h2 {
      padding-left: 23px;
    }
    @media (max-width: 940px) {
      h2 {
        padding-left: 0;
      }
    }
  }
}

// 예약하기 버튼
.reserve_btn {
  @include btn-style;
  display: block;
  text-align: center;
  width: 100%;
  margin-top: 50px;
  font-size: $desc-text-font;
}

// 옵션 버튼
.opt_btn {
  @include option-btn;
}

.storage_extra,
.option_list,
.temp_option,
.method_option,
.service_option {
  display: flex;
  gap: 10px;
}

// 보관 방법, 부가서비스
.storage_extra {
  justify-content: space-between;
  flex-wrap: wrap;
  gap: 40px;
}

// 보관 방법 제목
.method_title {
  display: flex;
  align-items: end;
  gap: 10px;
  p {
    font-size: $desc-text-font;
    color: $font-color;
    padding: 0 0 20px;
    font-weight: 600;
  }
}

// 보관 방법 옵션 - 온도 / 접수 방법
.option_list {
  gap: 40px;
  flex-wrap: wrap;
  // justify-content: space-between;
  .temp,
  .method {
    > p {
      color: $font-color;
      padding-bottom: 20px;
      font-weight: 500;
    }
    .opt_btn {
      // margin-bottom: 15px;
      font-size: $desc-text-font;
      color: $font-color;
    }
  }
}

// 부가서비스
.extra_service {
  > p {
    color: $font-color;
    padding-bottom: 20px;
    font-weight: 500;
  }
}

// 기사님께 맡길게요 / 직접 맡길게요
.delivery,
.in_person {
  width: 100%;
  background-color: #fff;
  border-radius: 10px;
  box-shadow: 1px 1px 6px rgba(0, 0, 0, 0.1);
  padding: 30px;
  margin-top: 20px;
  .store {
    color: $font-color;
    font-size: $desc-text-font;
    padding: 0 0 20px;
    font-weight: 500;
  }
  .time_selection {
    margin-top: 20px;
    .timeSwiper {
      text-align: center;

      .swiper-slide {
        &.highlight {
          color: $point-color;
        }
      }
    }
    p:last-child {
      padding: 8px 0 0;
      font-size: $mobile-notice-font;
      color: $point-color;
    }
  }
}

// 방문 베이커리
.custom-select {
  position: relative;
  width: 100%;

  .select-trigger {
    width: 100%;
    border: 2px solid $font-color;
    border-radius: 8px;
    padding: 15px 20px;
    background: #fff;
    cursor: pointer;
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-family: "SpokaHanSansNeo";
    font-size: $desc-text-font;
    transition: all 0.25s ease;
    position: relative;

    .label {
      display: flex;
      flex-direction: column;
      gap: 4px;
      width: 100%;
      p {
        font-size: $notice-text-font;
        line-height: 1.2;
        color: #111;
      }
      span {
        font-size: $mobile-notice-font;
        color: #949494;
        line-height: 1.2;
      }
      &.placeholder {
        color: #9fa3a7;
        font-size: $desc-text-font;
      }
    }

    &::after {
      content: "";
      position: absolute;
      right: 20px;
      top: 50%;
      margin-top: -6px;
      width: 12px;
      height: 12px;
      background: url(/images/pje/arrow.png) no-repeat center/12px;
      transition: transform 0.25s ease;
    }

    &.open::after {
      transform: rotate(180deg);
    }
  }

  .select-options {
    position: absolute;
    top: 100%;
    left: 0;
    right: 0;
    background: white;
    box-shadow: 1px 1px 8px rgba(0, 0, 0, 0.1);
    border-top: none;
    max-height: 200px;
    overflow-y: auto;
    z-index: 5;
    opacity: 0;
    visibility: hidden;
    transform: translateY(-10px);
    transition: all 0.25s ease;

    scrollbar-width: none; /* Firefox */
    -ms-overflow-style: none; /* IE, Edge */

    &::-webkit-scrollbar {
      display: none; /* Chrome, Safari, Opera */
    }

    &.open {
      opacity: 1;
      visibility: visible;
      transform: translateY(0);
    }

    .option {
      padding: 15px 20px;
      cursor: pointer;
      font-family: "SpokaHanSansNeo";
      font-size: $notice-text-font;
      color: #111;
      transition: background-color 0.25s;

      &:hover {
        background-color: #f5f5f5;
      }

      &:last-child {
        border-radius: 0 0 6px 6px;
      }

      span {
        font-size: $mobile-notice-font;
        color: #949494;
      }
    }
  }
}

.in_person {
  .locker_selection {
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 153.85px;
    // margin: 20px 0;
    p {
      span {
        color: #949494;
      }
    }
  }
  .locker_notice {
    padding: 20px 0;
    font-size: $notice-text-font;
  }
}

.reserved_options_list {
  margin-top: 40px;

  // 선택한 옵션
  .reserved_option {
    display: flex;
    justify-content: space-between;
    align-items: center;
    flex-wrap: wrap;
    width: 100%;
    background-color: #fff;
    border-radius: 10px;
    box-shadow: 1px 1px 6px rgba(0, 0, 0, 0.1);
    padding: 15px;
    position: relative;
    margin-bottom: 20px;

    .remove_btn {
      position: absolute;
      top: 15px;
      right: 15px;
      background: transparent;
      border: none;
      font-size: $mobile-notice-font;
      color: #949494;
      cursor: pointer;
      padding: 5px;
      line-height: 1;

      &:hover {
        color: $font-color;
      }
    }

    .selected_options {
      margin-bottom: 15px;
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

    .total_price {
      width: 100%;
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 50px;

      .quantity {
        display: flex;
        gap: 20px;
        button {
          border: none;
          background-color: transparent;
          cursor: pointer;
          font-size: 18px;
          color: $font-color;
        }
        input {
          width: 40px;
          padding: 8px 12px;
          text-align: center;
          border: none;
          border-radius: 8px;
          background-color: $main-color;
        }
      }

      .price {
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
  }
}
</style>
