<template>
  <table class="calendar">
    <thead>
      <tr>
        <td><button @click="changeMonth('dec')">-</button></td>
        <td colspan="5">
          <template v-if="month && year">
            {{`${month[lang]} ${year}`}}
          </template>
          <template v-else>
            Некорректная дата
          </template>
        </td>
        <td><button @click="changeMonth('inc')">+</button></td>
      </tr>
      <tr>
        <td :key="index" v-for="(day, index) in days[lang]">{{day}}</td>
      </tr>
    </thead>
    <tbody>
      <tr :key="index" v-for="(week, index) in weeksGroup">
        <td @click="selectDay(dayW)" class="calendar__day" :class="{'calendar__day--active' : dayW === day}" :key="index" v-for="(dayW, index) in week">
          {{dayW}}
        </td>
      </tr>
    </tbody>
  </table>
</template>

<script>
export default {
  name: 'Calendar',
  props: ['currentDate', 'lang'],
  data() {
    return {
      days: {
        Ru: ['Вс', 'Пн', 'Вт', 'Ср', 'Чт', 'Пт', 'Сб'],
        En: ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat']
      },
      weeksGroup: [],
      month: '',
      year: '',
      day: '',
      months: {
        0: {
          Ru: 'Январь',
          En: 'January'
        },
        1: {
          Ru: 'Февраль',
          En: 'February'
        },
        2: {
          Ru: 'Март',
          En: 'March'
        },
        3: {
          Ru: 'Апрель',
          En: 'April'
        },
        4: {
          Ru: 'Май',
          En: 'May'
        },
        5: {
          Ru: 'Июнь',
          En: 'June'
        },
        6: {
          Ru: 'Июль',
          En: 'July'
        },
        7: {
          Ru: 'Август',
          En: 'August'
        },
        8: {
          Ru: 'Сентрябрь',
          En: 'September'
        },
        9: {
          Ru: 'Октябрь',
          En: 'October'
        },
        10: {
          Ru: 'Ноябрь',
          En: 'November'
        },
        11: {
          Ru: 'Декабрь',
          En: 'December'
        },
      }
    }
  },
  methods: {
    getWeeksGroup(currentDate) {
      if(!currentDate) currentDate = new Date()
      const lastDayInMonth = new Date(currentDate.getFullYear(), currentDate.getMonth() + 1, 0).getDate()
      const firstDayInMonth = new Date(currentDate.getFullYear(), currentDate.getMonth(), 1).getDay()
      this.day = currentDate.getDate()
      this.month = this.months[currentDate.getMonth()]
      this.year = currentDate.getFullYear()
      const resultArr = []
      let weekCounter = 0
      resultArr[0] = []
      if(firstDayInMonth !== 0) {
        for (let i = 0; i < firstDayInMonth; i++) {
          resultArr[0].push('')
        }
      }
      for (let i = 0; i < lastDayInMonth; i++) {
        if(resultArr[weekCounter].length >= 7) {
          weekCounter++
          resultArr[weekCounter] = []
        }
        resultArr[weekCounter].push(i + 1)
      }
      this.weeksGroup = resultArr
    },
    changeMonth(type = 'inc') {
      const types = {
        January: 0,
        February: 1,
        March: 2,
        April: 3,
        May: 4,
        June: 5,
        July: 6,
        August: 7,
        September: 8,
        October: 9,
        November: 10,
        December: 11
      }
      this.getWeeksGroup(new Date(this.year, type === 'inc' ? types[this.month?.En] + 1 : types[this.month?.En] - 1, this.day))
    },
    selectDay(day) {
      if(day) this.day = day
    }
  },
  watch: {
    'currentDate'() {
      if(this.currentDate) this.getWeeksGroup(new Date(this.currentDate))
      else this.getWeeksGroup()
    }
  },
  mounted() {
    this.getWeeksGroup()
  }
}
</script>

<style scoped>
.calendar {
  width: 280px;
  border: 1px solid #ccc;
}
.calendar__day {
  cursor: pointer;
}
.calendar__day--active {
  outline: 1px solid blue;
}
</style>
