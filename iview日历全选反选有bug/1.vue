<template>
    <Row>
        <Col span="12">
            <DatePicker 
              type="date" multiple 
              :value="value" 
              :options="options1" 
              @on-change="change"
              placeholder="Select date" 
              style="width: 400px">
          </DatePicker>
        </Col>
        
    </Row>
</template>
<script>
    export default {
        data () {
            return {
              value: '2019-07-01,2019-07-02',
                options1: {
                    shortcuts: [
                        {
                            text: '选择整月',
                            value () {
                                return new Date();
                            },
                            onClick: (picker) => {
                              console.log(picker)
                              let year =  picker.datePanelLabel.labels[0].label
                              year = year.substring(-1, year.length-1)
                              
                              let month = picker.datePanelLabel.labels[1].label.substring(-1)  
                              month = month.substring(-1, month.length-1)
                              this.value = [...new Set(this.formatterDate(picker.value).concat(this.getDateStr(year,month)))]
                              console.log(this.value)
                                this.$Message.info('Click today');
                            }
                        },
                       
                    ]
                },
               
            }
        },
      methods: {
         getDateStr(y,m) {
	          var curDate = new Date();
            var curMonth = curDate.getMonth() + 1;
          	var year = y? y : curDate.getFullYear();
            var mon = m? m : curMonth ;
	           mon = mon > 9 ? mon  : '0' + mon
	          var dateCount = new Date(year, mon, 0).getDate()     
            console.log(dateCount)
	          var dateArray = []; 
	            for(var i= 1; i <= dateCount; i++) {
		                let day = i> 9? i : '0' + i
		                let _date = year + '-' + mon + '-' + day
		                dateArray.push(_date)
	            }
             
	           return dateArray
        },
        formatterDate(dateArray) {
         return dateArray.map(v => {
            let m = (new Date(v).getMonth() + 1) >9 ? (new Date(v).getMonth() + 1) :  '0' + (new Date(v).getMonth() + 1) 
            let res = new Date(v).getFullYear() + '-' + m + '-' + (new Date(v).getDate() >9 ? new Date(v).getDate() : '0' + new Date(v).getDate() )
            return res
          })
        
        },
        change(v) {
          console.log(v)
        }
      }
    }
</script>