<template>
  <div class="flow-txt">
    <div class="flowNotice" ref="marqueeContainer">
      <div class="flowNotice-wrap" ref="marqueeContent">
        <!-- 원본 텍스트 -->
        <div class="flowNotice-content">
          <p>WELCOME EVENTS 웰컴이벤트 참여시 쿠폰증정</p>
          <p>대구 가을 축제한정 10% 할인</p>
          <p>11AM / 3PM BREAD TIME SALE</p>
          <p>스탬프 모으고 무료쿠폰 받자</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from "vue";
import { useRouter } from "vue-router";

const router = useRouter();
const marqueeContainer = ref(null);
const marqueeContent = ref(null);
let animationId = null;
let scrollPosition = 0;
const scrollSpeed = 0.5; // 속도 조절 (숫자가 클수록 빠름)

onMounted(() => {
  // 복제본 생성
  const content = marqueeContent.value.querySelector('.flowNotice-content');
  const clone1 = content.cloneNode(true);
  const clone2 = content.cloneNode(true);
  
  marqueeContent.value.appendChild(clone1);
  marqueeContent.value.appendChild(clone2);
  
  // 애니메이션 시작
  animate();
});

const animate = () => {
  scrollPosition -= scrollSpeed;
  
  // 하나의 콘텐츠 너비만큼 이동하면 리셋
  const contentWidth = marqueeContent.value.querySelector('.flowNotice-content').offsetWidth + 100; // 100은 gap
  
  if (Math.abs(scrollPosition) >= contentWidth) {
    scrollPosition = 0;
  }
  
  marqueeContent.value.style.transform = `translateX(${scrollPosition}px)`;
  
  animationId = requestAnimationFrame(animate);
};

onUnmounted(() => {
  if (animationId) {
    cancelAnimationFrame(animationId);
  }
});
</script>

<style lang="scss" scoped>
@use "/src/assets/variables" as *;
@use "/src/assets/btn" as *;

.flow-txt {
width: 100%;
  .flowNotice {
    width: 100%;
    overflow: hidden;
    background: $font-color;
    padding: 10px;
    color: $bg-color;
    font-size: $mobile-notice-font;
    position: relative;
    
    .flowNotice-wrap {
      display: flex;
      width: fit-content;
      gap: 100px;
      will-change: transform;

      .flowNotice-content {
        display: flex;
        gap: 100px;
        white-space: nowrap;
        flex-shrink: 0;

        p {
          flex-shrink: 0;
        }
      }
    }
  }
}
</style>