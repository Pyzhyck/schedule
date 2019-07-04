<template>
  <div class="schedule">
    <div class="title">
      <h3>Графік винесення сміття на <span>{{month}}</span></h3>
    </div>
    <div class="table" :class="colsToHighlight" @mouseout="setHighlightCols()">
      <div class="row">
        <div class="col col--person"></div>
        <div class="col" 
          v-for="day in daysInMonth" 
          :key="day" 
          :class="{
            'weekend' : isWeekend(day)
          }"
          @mouseover="setHighlightCols(day)">
          <h5>{{day}}</h5>
        </div>
      </div>
      <div class="row" v-for="(person, key) in sortedTeam" :key="key">
        <div class="col col--person">
          <h5>{{person.name}}</h5>
        </div>
        <div class="col" 
          ref="col"
          v-for="day in daysInMonth" 
          :key="day"
          :class="{
            'weekend' : isWeekend(day)
          }"
          @mouseover="setHighlightCols(day)"></div>
      </div>
    </div>
  </div>
</template>

<script>
import moment from 'moment';

export default {
  name: 'Schedule',
  props: {
    team: Array
  },
  data() {
    return {
      colsToHighlight: ''
    }
  },
  created() {
    moment.locale('uk');
  },
  mounted() {
    for(let row = 0; row < this.team.length; row++) {
      for(let col = row; col < this.daysInMonth; col+=this.team.length) {
        if(this.$refs.col[col + row * this.daysInMonth]) {
          this.$refs.col[col + row * this.daysInMonth].style.backgroundColor = "#7f8c8d";
        }
      }
    }
    this.setHighlightCols(this.currentDay);
  },
  computed: {
    month() {
      return moment().format('MMMM');
    },
    daysInMonth() {
      this.cols = moment().daysInMonth();
      return this.cols
    },
    sortedTeam() {
      return this.team.sort((a, b) => a.name.localeCompare(b.name));
    },
    currentDay() {
      return moment().day();
    }
  },
  methods: {
    isWeekend(day) {
      let weekday = moment().day(day).format('ddd');
      if(weekday == 'нд' || weekday == 'сб') return true;
      return false;
    },
    setHighlightCols(day) {
      if(day) {
        this.colsToHighlight = 'cols-' + day;
      } else {
        this.colsToHighlight = 'cols-' + this.currentDay;
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.schedule {
  width: calc(100% - 30px);
  max-width: 1360px;
  margin: 0 auto;
}
.title {
  text-align: center;
  margin: 1rem 0 3rem;
  span {
    text-transform: capitalize;
  }
}

@for $i from 1 through 31 {
  .table.cols-#{$i} {
    .col {
      &:nth-child(#{$i + 1}) {
        background-color: #27ae60;
        * {
          color: #2c3e50;
        }
      }
    }
  }
}

.table {
  border: 2px solid #3498db;
  overflow: auto;
  .row {
    display: flex;
    min-width: 1350px;
    border-bottom: 1px solid #7f8c8d;
    transition: all .3s ease-in-out;
    &:hover {
      background-color: #27ae60;
      * {
        color: #2c3e50;
      }
    }
    &:last-child {
      border-bottom: none;
    }
    .col {
      width: 20px;
      min-width: 20px;
      text-align: center;
      padding: .5rem;
      border-right: 1px solid #7f8c8d;
      transition: all .3s ease-in-out;
      &.weekend {
        background-color: #c0392b !important;
        * {
          color: #bdc3c7 !important;
        }
      }
      &:last-child {
        border-right: none;
      }
      &--person {
        width: 100%;
        text-align: left;
      }
    }
  }
}
h1, h2, h3, h4, h5, h6 {
  margin: 0;
  padding: 0;
}
h1 {
  font-size: 3rem;
}
h2 {
  font-size: 2.5rem;
}
h3 {
  font-size: 2rem;
}
h4 {
  font-size: 1.5rem;
}
h5 {
  font-size: 1rem;
}
h6 {
  font-size: 0.5rem;
}
</style>
