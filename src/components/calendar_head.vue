<template>
  <div class="calendar-header clear">
    <div class="calendar-box" v-if="this.headOptions.type == 'comb'">
      <div class="calendar-content" :style="{'text-align': this.headStyle.combination}">
        <span class="calendar-prev" @click="handlePrevMonth"></span>
        <span class="calendar-headDate"> {{this.headOptions.date}} </span>
        <span class="calendar-next" @click="handleNextMonth"></span>
      </div>
      <button class="calendar-today" @click="handleToday()"> 今天 </button>
    </div>
    <div class="calendar-box" v-else>
      <span class="calendar-headDate"> {{this.headOptions.date}} </span>
      <span class="calendar-today dispersion-today" :style="{float: this.headStyle.todayBtn}" @click="handleToday()"> 今天 </span>
      <div class="calendar-content dispersion" :style="{float: this.headStyle.checkBtn}">
        <span class="calendar-prev" @click="handlePrevMonth"></span>
        <span class="calendar-next" @click="handleNextMonth"></span>
      </div>
     
    </div>
  </div>
</template>

<script>
  export default {
    props: {
      headOptions: Object
    },
    data () {
      return {
        headStyle: ''
      }
    },
    mounted() {
      this.headStyle = this.headOptions.style;
    },
    methods: {
          // 下一个月
      handleNextMonth () {
        this.$emit('handleNextMonth');
      },
      // 上一个月
      handlePrevMonth () {
        this.$emit('handlePrevMonth');
      },
      // 回到今天
      handleToday () {
        this.$emit('handleToday');
      }
    }
  }
</script>
<style lang="less">
  .calendar-header {
    margin-bottom: 10px;
    width: 100%;
    .calendar-box {
      position: relative;
      height: 32px;
      line-height: 32px;
      .calendar-content {
        width: 100%;
        .calendar-prev,
        .calendar-next {
          display: inline-block;
          vertical-align: middle;
          width: 8px;
          height: 11px;
          background: url('../assets/img/prev.png') no-repeat;
          background-size: contain;
          cursor: pointer;
        }
        .calendar-next {
          background: url('../assets/img/next.png') no-repeat;
          background-size: contain;
        }
      }
      .dispersion{
        width: initial;
        display: inline;
      }
      .calendar-headDate {
        vertical-align: middle;
        margin: 0 12px;
        font-size: 18px;
        color: #424953;
        -webkit-user-select: none;
        -webkit-touch-callout: none; 
        -moz-user-select: none;
        -ms-user-select: none; 
        user-select: none;
      }
      .calendar-today {
        position: absolute;
        top: 0;
        right: 0;
        width: 80px;
        height: 30px;
        text-align: center;
        border: 1px solid #E4E7EA;
        border-radius: 4px;
        font-size: 14px;
        color: black;
      }
      button:hover{
      border: 1px solid #1bc78554;
      cursor: pointer;
      color:#1bc78554;
      }
      .dispersion-today{
        position: inherit;
      }
      .calendar-center {
        margin: 0 auto;
      }
      .calendar-left {
        float: left;
      }
      .calendar-right {
        float: right;
      }
    }
  }
</style>