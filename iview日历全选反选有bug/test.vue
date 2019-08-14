<template>
  <Row>
    <Col span="24">
        <span>被选中的年份/月份：{{curYearMonths}} </span>
    </Col>
    <Col span="24" style="margin-bottom: 20px;">
        <span>未选中的日期：</span>
    </Col>
    <Col span="24">
      <DatePicker
        type="date"
        multiple
        :value="value1"
        :options="options1"
        @on-change="change"
        placeholder="Select date"
        style="width: 1000px"
      ></DatePicker>
    </Col>
  </Row>
</template>
<script>
export default {
  data() {
    return {
      value1: "",
      switchYears:'',
      switchMonth:"",
      curYearMonths:"",
      options1: {
        shortcuts: [
            {
                text: "整月选中",
                //value () {
                //  return new Date();
                //},
                onClick: picker => {
                    console.log(picker);
                    console.log("年和月",picker.datePanelLabel.labels);
                    this.curYearMonths = picker.datePanelLabel.labels[0].label + picker.datePanelLabel.labels[1].label
                    let years = picker.datePanelLabel.labels[0].label;
                    let months = picker.datePanelLabel.labels[1].label;

                    this.switchYears = years.substring(0, years.length - 1);
                    this.switchMonth = months.substring(0,months.length - 1);
                    console.log("切换的年月日",this.curYearMonths);

                    this.value1 = [...new Set(this.formatterDate(picker.value).concat(this.getDateStr(this.switchYears,this.switchMonth)))]
                    // this.value1 = this.getDateStr();
                    console.log("this.value1", this.value1);


                    this.$Message.info("整月已选中！");
                }
            },
            {
                text: '反选',
                value () {
                    // const date = new Date();
                    // date.setTime(date.getTime() - 3600 * 1000 * 24);
                    // return date;
                },
                onClick: (picker) => {
                    this.value1 = "";
                    this.$Message.info('已全部取消！');
                }
            },
        ]
      }
    };
  },
  methods: {
    change(v, s) {
      console.log("change",v, s);
    },
    getDateStr() {
        let newYears =  this.switchYears;
        let newMonths = this.switchMonth;
        //获取当前月份(现在是7月)的总天数: 
        // return new Date(newYears, newMonths, 0).getDate();
        console.log("1111",new Date(newYears, newMonths, 0).getDate());
        //获取当前月的每一天并显示出来
        var dayArry = [];
        var day = new Date(newYears, newMonths, 0).getDate();
        for (var i = 1; i <= day; i++) {

            let day1 = i > 9 ? i : "0" + i;
            let _date = newYears + "-" + newMonths + "-" + day1;
            dayArry.push(_date);
        }
        return dayArry.join(",");
        console.log("dayArry",dayArry)

    },
    formatterDate(dayArry) {
        return dayArry.map(v => {
        let m = (new Date(v).getMonth() + 1) >9 ? (new Date(v).getMonth() + 1) :  '0' + (new Date(v).getMonth() + 1) 
        let res = new Date(v).getFullYear() + '-' + m + '-' + (new Date(v).getDate() >9 ? new Date(v).getDate() : '0' + new Date(v).getDate() )
        return res
        })
    
    },
    // getDateStr() {
    //   var curDate = new Date();
    //   var curMonth = curDate.getMonth();
    //   var year = curDate.getFullYear();
    //   var mon = curMonth + 1 > 9 ? curMonth + 1 : "0" + (curMonth + 1);

    //   curDate.setMonth(curMonth + 1);
    //   curDate.setDate(0);
    //获取当前月份(现在是7月)的总天数: 
    //   var dateCount = curDate.getDate();

    //获取当前月的每一天并显示出来
    //   var dateArray = [];
    //   for (var i = 1; i <= dateCount; i++) {
    //     let day = i > 9 ? i : "0" + i;
    //     let _date = year + "-" + mon + "-" + day;
    //     dateArray.push(_date);
    //   }
    //   return dateArray.join(",");
    //   // 	console.log(dateArray.join(','))
    // }
  }
};
</script>

