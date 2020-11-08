<template>
  <calendar 
    :options="calendar" 
    class="calendar" 
    @handleClickDay="handleClickDay" 
    @handlePrevMonth="handlePrevMonth"
    @handleNextMonth="handleNextMonth"
  />
</template>
<script>
import calendar from './components/calendar'
export default {
  name: 'canlendar',
  data () {
    return {
      calendar: {
          type: 'comb',
          headStyle: {
            todayBtn: 'right',
            combination: 'center',
            checkBtn: 'right',
          },
          viewStyle: {
            day: 'right'
          },
          calendarData: []
        },
    }
  },
  components: {
    calendar
  },
  methods: {
    handleClickDay () {

    },
    handlePrevMonth () {

    },
    handleNextMonth () {
      
    }
  }
}
</script>

<style>

</style>
