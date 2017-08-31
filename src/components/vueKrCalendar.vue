<template>
  <div class="calendar">
    <div class="cal-panel-box">
      <cal-panel @cur-day-changed="handleChangeCurDay" @month-changed="handleMonthChanged" :tip="tip"></cal-panel>
    </div>
    <cal-events>
      <div class="to-do-list" v-for="(item, index) in todoList">
        <h2 class="title">{{item.title}}</h2>
        <p class="text">{{item.text}}</p>
        <p class="time">{{item.time}}</p>
      </div>
    </cal-events>
  </div>
</template>

<script>
import CalPanel from './cal-panel';
import CalEvents from './cal-events';
const dateObj = new Date();
const padding = (num) => {
  if (num <= 9) {
    return `0${num}`;
  } return num;
};
export default {
  name: 'calendar',
  data () {
    return {
      tip: [`${dateObj.getFullYear()}-${padding(dateObj.getMonth() + 1)}-01`,`${dateObj.getFullYear()}-${padding(dateObj.getMonth() + 1)}-22`],
      todoList: [{
        title:'Title-1',
        text:'long description',
        time:'2017-08-31'
      },{
        title:'Title-2',
        text:'Life is too short to do mediocre work, and it is definitely too short to build shitty things',
        time:'2017-09-21'
      }]
    }
  },
  components: {
    CalPanel,
    CalEvents,
  },
  methods: {
    handleChangeCurDay(data) { // 日期切换
      console.log(data);
    },
    handleMonthChanged(data) { // 月份切换
      console.log(data);
    },
  }
}
</script>

<style lang="less" scoped>
.calendar{
  width: 60%;
  margin: 0 auto;
  .cal-panel-box{
    background: #efefef;
    border-radius: 5px;
  }
  .to-do-list{
    margin-bottom: 10px;
    background: #efefef;
    padding: 5px;
    border-radius: 5px;
    .title{
      font-size: 16px;
    }
    .text{
      font-size: 14px;
      color: #666;
      line-height: 28px;
    }
    .time{
      font-size: 14px;
      color: #999;
    }
  }
}
</style>
