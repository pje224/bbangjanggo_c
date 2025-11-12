<template>
  <!-- 날짜 선택 -->
  <div class="calendar">
    <div class="calendar__month">
      <div class="cal-month__previous">〈</div>
      <div class="cal-month__current"></div>
      <div class="cal-month__next">〉</div>
    </div>
    <div class="calendar__head">
      <div class="cal-head__day"></div>
      <div class="cal-head__day"></div>
      <div class="cal-head__day"></div>
      <div class="cal-head__day"></div>
      <div class="cal-head__day"></div>
      <div class="cal-head__day"></div>
      <div class="cal-head__day"></div>
    </div>
    <div class="calendar__body">
      <div class="cal-body__week">
        <div class="cal-body__day"></div>
        <div class="cal-body__day"></div>
        <div class="cal-body__day"></div>
        <div class="cal-body__day"></div>
        <div class="cal-body__day"></div>
        <div class="cal-body__day"></div>
        <div class="cal-body__day"></div>
      </div>
      <div class="cal-body__week">
        <div class="cal-body__day"></div>
        <div class="cal-body__day"></div>
        <div class="cal-body__day"></div>
        <div class="cal-body__day"></div>
        <div class="cal-body__day"></div>
        <div class="cal-body__day"></div>
        <div class="cal-body__day"></div>
      </div>
      <div class="cal-body__week">
        <div class="cal-body__day"></div>
        <div class="cal-body__day"></div>
        <div class="cal-body__day"></div>
        <div class="cal-body__day"></div>
        <div class="cal-body__day"></div>
        <div class="cal-body__day"></div>
        <div class="cal-body__day"></div>
      </div>
      <div class="cal-body__week">
        <div class="cal-body__day"></div>
        <div class="cal-body__day"></div>
        <div class="cal-body__day"></div>
        <div class="cal-body__day"></div>
        <div class="cal-body__day"></div>
        <div class="cal-body__day"></div>
        <div class="cal-body__day"></div>
      </div>
      <div class="cal-body__week">
        <div class="cal-body__day"></div>
        <div class="cal-body__day"></div>
        <div class="cal-body__day"></div>
        <div class="cal-body__day"></div>
        <div class="cal-body__day"></div>
        <div class="cal-body__day"></div>
        <div class="cal-body__day"></div>
      </div>
      <div class="cal-body__week">
        <div class="cal-body__day"></div>
        <div class="cal-body__day"></div>
        <div class="cal-body__day"></div>
        <div class="cal-body__day"></div>
        <div class="cal-body__day"></div>
        <div class="cal-body__day"></div>
        <div class="cal-body__day"></div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { onMounted, ref } from "vue";
import moment from "moment";
import "moment/locale/ko";

// emit 정의
const emit = defineEmits(["date-selected"]);

moment.locale("ko");

let calendarInstance = null; // Calendar 인스턴스 저장

class Calendar {
  constructor(emitFn) {
    this.emitFn = emitFn; // emit 함수 저장
    this.monthDiv = document.querySelector(".cal-month__current");
    this.headDivs = document.querySelectorAll(".cal-head__day");
    this.bodyDivs = document.querySelectorAll(".cal-body__day");
    this.nextDiv = document.querySelector(".cal-month__next");
    this.prevDiv = document.querySelector(".cal-month__previous");
  }

  init() {
    moment.locale(window.navigator.userLanguage || window.navigator.language);

    this.month = moment();
    this.today = this.selected = this.month.clone();
    this.weekDays = ["일", "월", "화", "수", "목", "금", "토"];

    this.headDivs.forEach((day, index) => {
      day.innerText = this.weekDays[index];
    });

    this.nextDiv.addEventListener("click", (_) => {
      this.addMonth();
    });
    this.prevDiv.addEventListener("click", (_) => {
      this.removeMonth();
    });

    this.bodyDivs.forEach((day) => {
      day.addEventListener("click", (e) => {
        const clickedDay = parseInt(e.target.innerText, 10);
        if (Number.isNaN(clickedDay)) return;

        let baseMonth = this.month.clone();
        if (e.target.classList.contains("cal-day__month--next")) {
          baseMonth = baseMonth.add(1, "month");
        } else if (e.target.classList.contains("cal-day__month--previous")) {
          baseMonth = baseMonth.subtract(1, "month");
        }

        const clickedDate = baseMonth.clone().date(clickedDay);

        // 오늘 이전 날짜는 선택 불가
        if (clickedDate.isBefore(moment().startOf("day"))) {
          return;
        }

        this.selected = clickedDate;
        this.update();

        // 부모에게 선택된 날짜 전달
        this.emitFn("date-selected", this.selected.format("MM/DD"));
      });
    });

    this.update();
  }

  // 초기화 메서드 추가
  reset() {
    this.selected = this.today.clone();
    this.month = moment();
    this.update();
  }

  update() {
    this.calendarDays = {
      first: this.month.clone().startOf("month").startOf("week").date(),
      last: this.month.clone().endOf("month").date(),
    };

    this.monthDays = {
      lastPrevious: this.month.clone().subtract(1, "months").endOf("month").date(),
      lastCurrent: this.month.clone().endOf("month").date(),
    };

    this.monthString = this.month.clone().format("YYYY.MM");

    this.draw();
  }

  addMonth() {
    this.month.add(1, "month");

    this.update();
  }

  removeMonth() {
    this.month.subtract(1, "month");

    this.update();
  }

  draw() {
    this.monthDiv.innerText = this.monthString;

    let index = 0;

    if (this.calendarDays.first > 1) {
      for (let day = this.calendarDays.first; day <= this.monthDays.lastPrevious; index++) {
        this.bodyDivs[index].innerText = day++;

        this.cleanCssClasses(false, index);

        this.bodyDivs[index].classList.add("cal-day__month--previous");
      }
    }

    let isNextMonth = false;
    for (let day = 1; index <= this.bodyDivs.length - 1; index++) {
      if (day > this.monthDays.lastCurrent) {
        day = 1;
        isNextMonth = true;
      }

      this.cleanCssClasses(true, index);

      if (!isNextMonth) {
        if (day === this.today.date() && this.today.isSame(this.month, "day")) {
          this.bodyDivs[index].classList.add("cal-day__day--today");
        }

        if (day === this.selected.date() && this.selected.isSame(this.month, "month")) {
          this.bodyDivs[index].classList.add("cal-day__day--selected");
        }

        this.bodyDivs[index].classList.add("cal-day__month--current");

        // 비활성화: 오늘 이전 날짜
        const currentDate = this.month.clone().date(day);
        if (currentDate.isBefore(moment().startOf("day"))) {
          this.bodyDivs[index].classList.add("cal-day__day--disabled");
        }
      }

      this.bodyDivs[index].innerText = day++;
    }
  }

  cleanCssClasses(selected, index) {
    this.bodyDivs[index].classList.contains("cal-day__month--next") &&
      this.bodyDivs[index].classList.remove("cal-day__month--next");
    this.bodyDivs[index].classList.contains("cal-day__month--previous") &&
      this.bodyDivs[index].classList.remove("cal-day__month--previous");
    this.bodyDivs[index].classList.contains("cal-day__month--current") &&
      this.bodyDivs[index].classList.remove("cal-day__month--current");
    this.bodyDivs[index].classList.contains("cal-day__day--today") &&
      this.bodyDivs[index].classList.remove("cal-day__day--today");
    this.bodyDivs[index].classList.contains("cal-day__day--disabled") &&
      this.bodyDivs[index].classList.remove("cal-day__day--disabled");
    if (selected) {
      this.bodyDivs[index].classList.contains("cal-day__day--selected") &&
        this.bodyDivs[index].classList.remove("cal-day__day--selected");
    }
  }
}
onMounted(() => {
  calendarInstance = new Calendar(emit);
  calendarInstance.init();
});
// 초기화 함수
const reset = () => {
  if (calendarInstance) {
    calendarInstance.reset();
  }
};

// 부모 컴포넌트에서 접근 가능하도록 expose
defineExpose({
  reset,
});
</script>

<style lang="scss" scoped>
@use "../assets/variables" as *;


.calendar {
  // width: 350px;
  width: 100%;
  margin: 23px auto 0;
}

.calendar__month {
  font-size: 20px;
  font-weight: 800;
  padding: 10px 0;
  width: 100%;
  position: relative;
}

.cal-month__previous,
.cal-month__next {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  cursor: pointer;
  width: 30px;
  height: 30px;
  text-align: center;
  color: $sub-color;
}
.cal-month__previous {
  left: 20%;
}
.cal-month__next {
  right: 20%;
}

.cal-month__current {
  text-align: center;
  color: $font-color;
}

.cal-head__day,
.cal-body__day {
  display: flex;
  align-items: center;
  justify-content: center;
  // height: 50px;
  text-align: center;
  color: $font-color;
  font-family: "SpokaHanSansNeo";
  font-weight: 600;
}

.cal-body__week,
.calendar__head {
  display: grid;
  grid-template-columns: repeat(7, minmax(0, 1fr));
  height: 50px;
  width: 100%;
  column-gap: 0;
  // background-color: #fff;
}

.calendar__head {
  line-height: 50px;
  position: relative;
  width: 100%;
  max-width: 1000px;

  &:after {
    content: " ";
    position: absolute;
    bottom: 0;
    left: 0;
    right: 0;
    height: 1px;
    // background-color: #90caf9;
  }
}
.calendar__body {
  width: 100%;
}

.cal-body__day {
  color: transparent;
  line-height: 50px;
  text-align: center;
  cursor: pointer;
}

.cal-day__month--current {
  color: $sub-color;
}

// 오늘 날짜 테두리 원 스타일
.cal-day__day--today {
  border: 2px solid $font-color;
  border-radius: 50%;
  width: 44px;
  height: 44px;
  margin: auto;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: transparent;
  color: $font-color;
}

.cal-day__day--selected {
  background-color: $font-color;
  // box-shadow: 0 5px 10px -5px rgba(0, 0, 0, 0.75);
  border-radius: 50%;
  color: #fff;
  width: 44px;
  height: 44px;
  margin: auto; // 셀 중앙 정렬
  display: flex;
  align-items: center;
  justify-content: center;
}

// 과거 날짜 비활성화
.cal-day__day--disabled {
  color: #c5c5c5;
  pointer-events: none;
}
</style>
