<template>
  <div class="calendar">
    <calendarHeader
      :headOptions="headOptions"
      @handlePrevMonth="handlePrevMonth"
      @handleNextMonth="handleNextMonth"
      @handleToday="handleToday"
    />
    <ul class="calendar-week clear">
      <li v-for="(item, index) in weekList" :key="index" class="week-item">
        {{ item }}
      </li>
    </ul>
    <ul class="calendar-view clear">
      <li
        v-for="(item, index) in visibleCalendar"
        :key="index"
        class="date-view"
        :class="[
          { 'month-class': !isCurrentMonth(item.date) },
          { handleDay: item.clickDay },
        ]"
        @click="handleClickDay(item)"
      >
        <div>
          <div class="modelFixed" ref="child" v-show="isShowMultiple">
            <input hidden v-model="nowDate" />
            <input
              type="text"
              class="input_text"
              v-model="newAddText"
              placeholder="添加日程标题"
            /><br />
            <button
              style="
                margin-top: 150px;
                margin-left: 40%;
                width: 50px;
                border-radius: 10px;
              "
              @click="addNewList(item.date)"
            >
              添加
            </button>
            <button
              style="margin-left: 40%; width: 50px; border-radius: 10px"
              @click="cancel()"
            >
              取消
            </button>
          </div>
        </div>
        <span class="add" id="add" @click="setMaskShow(item.date)"></span>
        <span
          class="date-day"
          :style="dayStyle"
          :class="[
            { 'opacity-class': !isCurrentMonth(item.date) },
            { todayBg: isCurrentDay(item.date) },
          ]"
        >
          {{ item.day }}
        </span>
        <span class="calendar-num">
          {{ item.calendarNum }}
        </span>
        <ul></ul>
        <span v-for="(list, index) in schedule" v-bind:key="list.id">
          {{ list.title }}
          <button v-on:click="lists.splice(index, 1)">删除</button>
        </span>
      </li>
    </ul>
  </div>
</template>
<script>
import calendarHeader from "./calendar_head";
import * as utils from "../assets/api/utils";
import "../assets/css/reset.css";
export default {
  props: {
    options: Object,
  },
  components: {
    calendarHeader,
  },
  data() {
    let { year, month, day } = utils.getNewDate(new Date());
    return {
      headOptions: {
        type: this.options.type,
        style: this.options.headStyle,
        date: '',
      },
      weekList: [
        "星期日",
        "星期一",
        "星期二",
        "星期三",
        "星期四",
        "星期五",
        "星期六",
      ],
      time: { year, month, day },
      calendarList: [],
      isShowMultiple: false,
      schedule: [], //日程
      newAddText: "",
      nowDate: "",
    };
  },
  computed: {
    dayStyle: function () {
      return {
        textAlign: this.options.viewStyle.day,
      };
    },
    visibleCalendar() {
      let calendatArr = [];
      let { year, month } = utils.getNewDate(
        utils.getDate(this.time.year, this.time.month, 1)
      );
      let currentFirstDay = utils.getDate(year, month, 1);
      let weekDay = currentFirstDay.getDay();
      let startTime = currentFirstDay - weekDay * 24 * 60 * 60 * 1000;
      let monthDayNum;
      if (weekDay == 5 || weekDay == 6) {
        monthDayNum = 42;
      } else {
        monthDayNum = 35;
      }
      for (let i = 0; i < monthDayNum; i++) {
        calendatArr.push({
          date: new Date(startTime + i * 24 * 60 * 60 * 1000),
          year: year,
          month: month + 1,
          day: new Date(startTime + i * 24 * 60 * 60 * 1000).getDate(),
          clickDay: false,
        });
      }
      this.headOptions.date = ` ${year} 年 ${utils.month(month)}`;
      return calendatArr;
    },
  },
  methods: {
    //添加日程信息
    // addNewList(date) {
    //   if (this.nowDate == date.getTime()) {
    //     this.schedule.push({
    //       title: this.newAddText,
    //       id:date
    //     });
    //     this.newAddText = "";
    //   }
    // },
    setMaskShow(date) {
      this.nowDate = date.getTime();
      this.isShowMultiple = !this.isShowMultiple;
    },
    cancel() {
      this.isShowMultiple = false;
    },
    isCurrentMonth(date) {
      let { year: currentYear, month: currentMonth } = utils.getNewDate(
        utils.getDate(this.time.year, this.time.month, 1)
      );
      let { year, month } = utils.getNewDate(date);
      return currentYear == year && currentMonth == month;
    },
    isCurrentDay(date) {
      let {
        year: currentYear,
        month: currentMonth,
        day: currentDay,
      } = utils.getNewDate(new Date());
      let { year, month, day } = utils.getNewDate(date);
      return currentYear == year && currentMonth == month && currentDay == day;
    },
    handleNextMonth() {
      let nextMonth = utils.getDate(this.time.year, this.time.month, 1);
      nextMonth.setMonth(nextMonth.getMonth() + 1);
      this.time = utils.getNewDate(nextMonth);
      this.headOptions.date = `${utils.month(this.time.month)} ${
        this.time.year
      }`;
      this.$emit("handleNextMonth");
    },
    handlePrevMonth() {
      let prevMonth = utils.getDate(this.time.year, this.time.month, 1);
      prevMonth.setMonth(prevMonth.getMonth() - 1);
      this.time = utils.getNewDate(prevMonth);
      this.headOptions.date = `${utils.month(this.time.month)} ${
        this.time.year
      }`;
      this.$emit("handlePrevMonth");
    },
    handleToday() {
      this.time = utils.getNewDate(new Date());
      this.returnDate();
      this.$emit("handleToday");
    },
    handleClickDay(item) {
      this.$forceUpdate();
      this.$emit("handleClickDay", item);
      this.calendarList.map((x) => {
        x.clickDay = false;
      });
      this.$set(item, "clickDay", true);
    },
  },
  created() {
    this.calendarList = this.visibleCalendar;
    this.calendarType = this.options.calendarType;
  },
};
</script>

<style lang="less">
.calendar {
  padding: 23px 30px 30px;
  width: 100%;
  height: 100%;
  background: #f9fafc;
  box-sizing: border-box;
  .calendar-week {
    width: 100%;
    height: 46px;
    line-height: 46px;
    border-right: none;
    .week-item {
      float: left;
      width: 14.285%;
      text-align: center;
      font-size: 14px;
      color: #424953;
      font-weight: 600;
    }
  }
  .model {
    width: 100%;
    height: 100%;
    position: fixed;
    top: 0;
    left: 0;
    z-index: 999;
  }
  .modelFixed {
    width: 200px;
    height: 200px;
    position: absolute;
    top: 120px;
    left: 500px;
    padding: 5px;
    background: #ffffff;
    box-shadow: 3px 2px 5px #7777;
    .le-schedule-panel-color-point {
      display: block;
      width: 8px;
      height: 8px;
    }
    .input_text {
      border: none;
    }
  }
  .calendar-view {
    width: 100%;
    border-left: 1px solid #e4e7ea;
    .date-view {
      float: left;
      width: 14.285%;
      height: 80px;
      border-right: 1px solid #e4e7ea;
      border-bottom: 1px solid #e4e7ea;
      cursor: pointer;
      .date-day {
        padding: 8px 8px 0;
        display: block;
        font-size: 14px;
        float: right;
      }
      .calendar-num {
        margin-top: 6px;
        display: block;
        width: 100%;
        text-align: center;
        font-size: 30px;
        color: #01050e;
      }
    }
    .date-view:hover {
      .add {
        display: inline-block;
      }
    }
    .add {
      display: none;
      width: 30px;
      height: 30px;
      background: url("../assets/img/add.jpg") no-repeat;
      background-size: contain;
    }
    .opacity-class {
      opacity: 0.5;
    }
    .month-class {
      background-color: rgba(218, 217, 217, 0.986);
      background-size: 20px 20px;
    }
    .todayBg {
      background-color: blueviolet;
      width: 40px;
      height: 40px;
      border-radius: 20px;
      text-align: center;
    }
    .handleDay {
      .date-day {
        color: #114bdd !important;
      }
      .calendar-num {
        color: rgb(170, 166, 166) !important;
      }
    }
  }
}
</style>