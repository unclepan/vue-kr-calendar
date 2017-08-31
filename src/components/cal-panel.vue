<template>
  <div class="cal-wrapper">
    <div class="cal-header">
      <div class="l" @click="preMonth"><div class="arrow-left icon">&nbsp;</div></div>
      <div class="title">{{year + '年' + month  + '月'}}</div>
      <div class="r" @click="nextMonth"><div class="arrow-right icon">&nbsp;</div></div>
    </div>
    <div class="cal-body">
      <div class="weeks">
        <span v-for="(dayName, index) in dayNames" class="item" :key="index">{{dayName}}</span>
      </div>
      <div class="dates" >
        <div v-for="(item, index) in dayList" class="item" :key="index">
          <p class="date-num" :class="{selected: selectedDay===item.format}" @click="handleChangeCurday(item)">{{item.showDate}}</p>
          <span v-if="today===item.format" class="is-today"></span>
          <span v-if="selectedDay===item.format" class="is-event"></span>
          <span v-if="tip.find((t) => t === item.format)" class="is-tip"></span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { getMonthData, format } from './date';

const dateObj = new Date();
export default {
  name: 'cal-panel',
  props: {
    tip: Array
  },
  data() {
    return {
      selectedDay: format(dateObj), // 选中某天
      dayList: [], // 日期
      dayNames: ['周一', '周二', '周三', '周四', '周五', '周六', '周日'], // 周
      year: '', // 当前年
      month: '', // 当前月
    };
  },
  computed: {
    today() {
      return format(dateObj);// 今天
    },
  },
  mounted() {
    this.metDatList();
  },
  methods: {
    metDatList(year, month) {
      const data = getMonthData(year, month);
      const list = data.days.map((item) => {
        const a = new Date(data.year, data.month - 1, item.date);
        const b = format(a);
        return { format: b, ...item };
      });
      this.year = data.year;
      this.month = data.month;
      this.dayList = list;
    },
    preMonth() {
      let year = this.year;
      let month;
      month = this.month - 1;
      if (month === 0) {
        month = 12;
        year--;
      }
      this.metDatList(year, month);
      this.$emit('month-changed', `${this.year}/${this.month}`);
    },
    nextMonth() {
      const year = this.year;
      const month = this.month + 1;
      this.metDatList(year, month);
      this.$emit('month-changed', `${this.year}/${this.month}`);
    },
    handleChangeCurday(date) {
      const changeDate = new Date(this.year, this.month - 1, date.date);
      this.selectedDay = format(changeDate);
      this.$emit('cur-day-changed', this.selectedDay);
    },
  },
};
</script>

<style lang="less" scoped>
  @textContent: #626E7C;
  @textTip: #AEB7C0;
  @base-orange: #694EC4;
  @borderColor: #E1E6EA;
  .cal-wrapper{
    width: 100%;
    padding: 10px;
    margin-bottom: 20px;
  }
  .cal-wrapper{
    .cal-header{
      position: relative;
      width: 100%;
      font-weight: bolder;
      overflow: hidden;
	    padding-bottom:0px;
      &>div{
        float: left;
        line-height: 20px;
        padding: 10px 0;
      }
      .title{
        width: 60%;
        text-align: center;
        font-size: 13px;
        color: @textContent;
      }
      .l{
        text-align: left;
        width: 20%;
        cursor: pointer;
        user-select: none;
        -webkit-tap-highlight-color: rgba(0, 0, 0, 0);
      }
      .r{
        text-align: right;
        width: 20%;
        cursor: pointer;
        user-select: none;
        -webkit-tap-highlight-color: rgba(0, 0, 0, 0);
      }
    }
    .cal-body{
      width: 100%;
      .weeks{
        width: 100%;
        overflow: hidden;
        text-align: center;
        .item{
          line-height: 40px;
          float: left;
          width: 14.285%;
          font-size: 13px;
          color: @textTip;
        }
      }
      .dates{
        width: 100%;
        overflow: hidden;
        text-align: center;
        .item{
          position: relative;
          float: left;
          display: block;
          width: 14.285%;
          -webkit-tap-highlight-color: rgba(0, 0, 0, 0);
          .date-num{
            line-height: 36px;
            font-size: 13px;
            color: @textContent;
            position: relative;
            z-index: 3;
            cursor: pointer;
            width: 42px;
            margin: 0 auto;
          }
          .selected{
            color: #fff;
          }
          .is-event{
            content: '';
            background-color: @base-orange;
            border-radius: 50%;
            width: 26px;
            height: 26px;
            position: absolute;
            left: 50%;
            top: 50%;
            z-index: 1;
            margin-left: -13px;
            margin-top: -14px;
          }
          .is-tip{
            content: '';
            background-color: @base-orange;
            border-radius: 50%;
            opacity: .8;
            width: 6px;
            height: 6px;
            position: absolute;
            left: 50%;
            top: 50%;
            z-index: 2;
            margin-left: -3px;
            margin-top: 14px;
          }
          .is-today{
            content: '';
            border: 1px solid @base-orange;
            border-radius: 50%;
            width: 24px;
            height: 24px;
            position: absolute;
            left: 50%;
            top: 50%;
            z-index: 1;
            margin-left: -13px;
            margin-top: -14px;
          }
        }
      }
    }
  }
  .arrow-left.icon {
    color: #000;
    position: absolute;
    left: 6%;
    margin-top: 10px;
  }
  .arrow-left.icon:before {
    content: '';
    position: absolute;
    left: 1px;
    top: -5px;
    width: 10px;
    height: 10px;
    border-top: solid 1px currentColor;
    border-right: solid 1px currentColor;
    -webkit-transform: rotate(-135deg);
            transform: rotate(-135deg);
  }
  .arrow-right.icon {
    color: #000;
    position: absolute;
    right: 6%;
    margin-top: 10px;
  }
  .arrow-right.icon:before {
    content: '';
    position: absolute;
    right: 1px;
    top: -5px;
    width: 10px;
    height: 10px;
    border-top: solid 1px currentColor;
    border-right: solid 1px currentColor;
    -webkit-transform: rotate(45deg);
            transform: rotate(45deg);
  }
</style>
