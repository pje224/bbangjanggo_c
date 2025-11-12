<template>
  <!-- 회원가입 이용 약관 체크란 -->
  <div class="signupTerms-wrap">
    <!-- ✅약관 전체 동의 박스 -->
    <div class="checkAll-box">
      <div class="check-box">
        <div class="checkAll-label">
          <input type="checkbox" id="checkAll" v-model="allChecked" @click="toggleAll" />
          <label for="checkAll"><p>전체 동의하기</p> </label>
        </div>
        <span
          >실명 인증된 아이디로 가입, 위치기반서비스 이용약관(선택), 이벤트 및 혜택 정보 수신(선택)에 동의합니다. </span
        ><span class="terms-toggleBtn" @click="toggleTerms">{{ isExpended ? "접기" : "전체보기>" }}</span>
      </div>
    </div>

    <!-- ✅개별 이용약관 모음 -->
    <div class="terms-box" :class="{ expended: isExpended }">
      <div class="check-box" v-for="term in terms" :key="term.id">
        <input type="checkbox" :id="`term${terms.id}`" v-model="checkedTerms" :value="term.id" />
        <label :for="`term${term.id}`">{{ term.label }}</label>
        <div class="check-box-txt">
          <span v-html="term.content"></span>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, watch, computed } from "vue";

const checkedTerms = ref([]);
const allChecked = ref(false);
const errorMessage = ref("");

// 약관 배열 더미
const terms = [
  {
    id: 1,
    label: "빵장고 서비스 이용약관 (필수)",
    required: true,
    content: `<strong>제1조 (목적)</strong><br />이 약관은 빵장고(이하 "회사")가 제공하는 빵 보관 대행 및 관련 서비스(이하 "서비스")의 이용과 관련하여 회사와 이용자 간의 권리, 의무 및 책임사항을 규정함을 목적으로 합니다.`,
  },
  {
    id: 2,
    label: "개인정보 수집 및 이용 (필수)",
    required: true,
    content: `<strong>[빵장고 개인정보 수집 및 이용 동의 안내]</strong><br />개인정보보호법에 따라 빵장고 서비스에 회원가입을 신청하시는 분께 수집하는 개인정보의 항목을 안내드립니다.`,
  },
  {
    id: 3,
    label: "위치기반서비스 이용약관 (선택)",
    required: false,
    content: `<strong class="strongTxt">[빵장고 위치기반서비스 이용약관 동의 안내]</strong><br />본 약관은 이용자가 빵장고가 제공하는 위치기반서비스를 이용함에 있어 회사와 이용자 간의 권리를 규정합니다.`,
  },
  {
    id: 4,
    label: "이벤트 및 혜택 정보 수신 (선택)",
    required: false,
    content: `<strong>[이벤트 및 혜택 정보 수신 동의 안내]</strong><br />회사는 서비스 이용자에게 이벤트, 프로모션, 할인 혜택 등의 정보를 제공할 수 있습니다.`,
  },
];

// 필수 약관만 필터링
const requiredTerms = computed(() => terms.filter(t => t.required));

// 필수 약관이 모두 체크되었는지 확인
const isRequiredTermsChecked = computed(() => {
  return requiredTerms.value.every(term => checkedTerms.value.includes(term.id));
});

watch(checkedTerms, (newVal) => {
  allChecked.value = newVal.length === terms.length;
  // 체크 상태가 변경되면 에러 메시지 초기화
  if (errorMessage.value && isRequiredTermsChecked.value) {
    errorMessage.value = "";
  }
});

const toggleAll = () => {
  if (!allChecked.value) {
    checkedTerms.value = terms.map((t) => t.id);
    allChecked.value = true;
  } else {
    checkedTerms.value = [];
    allChecked.value = false;
  }
};

// 전체 동의 약관에서 개별약관을 펼치기(토글기능)
const isExpended = ref(false);
const toggleTerms = () => {
  isExpended.value = !isExpended.value;
};

// 회원가입 처리 함수
const handleSignup = () => {
  // 필수 약관 체크 검증
  if (!isRequiredTermsChecked.value) {
    errorMessage.value = "필수 약관에 모두 동의해주세요.";
    isExpended.value = true; // 약관 펼치기
    return;
  }
  
  // 필수 약관이 모두 체크되었으면 회원가입 진행
  errorMessage.value = "";
  alert("회원가입이 완료되었습니다!");
  // 실제 회원가입 로직 실행
  // ...
};

// 부모 컴포넌트에서 사용할 수 있도록 expose
defineExpose({
  isRequiredTermsChecked,
  handleSignup
});
</script>

<style scoped lang="scss">
@use "/src/assets/variables" as *;
@use "/src/assets/btn" as *;

// 약관 전체 레이아웃
.signupTerms-wrap {
  display: flex;
  flex-direction: column;
  gap: 25px;

  //   ✅약관 전체 동의 박스
  .checkAll-box {
    box-shadow: 1px 1px 4px rgba(0,0,0,0.1);
    background-color: #fff;
    border-radius: 15px;
    padding: 30px;
    .check-box {
      display: flex;
      flex-direction: column;
      align-items: flex-start;
      gap: 15px;

      .checkAll-label {
        display: flex;
        label {
          margin-left: 15px;
          p {
            font-family: "Cafe24Surround";
            color: $font-color;
            font-size: $f-a-q-text-font;
          }
        }
      }
    }
  }

  //   ✅개별 이용약관 모음

  .terms-box {
    display: flex;
    flex-direction: column;
    gap: 25px;
    position: relative;
    height: 0;
    overflow: hidden;
    transition: all 0.3s;
    &.expended {
      height: auto;
    }
    .check-box {
      input {
        margin-bottom: 25px;
        background-color: #fff;
      }
      label {
        font-family: "Cafe24Surround";
        color: $font-color;
        font-size: $f-a-q-text-font;

        margin-left: 15px;
        margin-bottom: 25px;
      }
      .check-box-txt {
        padding: 30px;
        background-color: #fff;
        box-shadow: 1px 1px 5px rgba(0, 0, 0, 0.2);
        border-radius: 10px;
        span {
          .strongTxt {
            margin-bottom: 20px;
          }
        }

        max-height: 200px;
        overflow-y: auto;
        scrollbar-width: thin;
        scrollbar-color: $sub-color transparent;
      }
    }
  }
}

.terms-toggleBtn {
  cursor: pointer;
  color: $sub-color;
  font-weight: bold;
  // text-decoration: underline;
}
</style>
