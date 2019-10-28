<template>
  <div>
    <select name id v-model="year">
      <option v-for="i in 200" :value="1900+i" :key="i">{{1900+i}}</option>
    </select>

    <select name id v-model="Month">
      <option v-for="i in 12" :value="i" :key="i+300">{{i}}</option>
    </select>
    <table>
      <tr>
        <th>一</th>
        <th>二</th>
        <th>三</th>
        <th>四</th>
        <th>五</th>
        <th>六</th>
        <th>日</th>
      </tr>
      <tr v-for="i in 6" :key="i">
        <td v-for="j in 7" :key="j+10">
          {{jisuan[(i-1)*7+(j-1)].gongli}}
          <span
            v-show="!jisuan[(i-1)*7+(j-1)].nongli.term"
          >{{jisuan[(i-1)*7+(j-1)].nongli.dayCn}}</span>
          <span v-show="jisuan[(i-1)*7+(j-1)].nongli.term">{{jisuan[(i-1)*7+(j-1)].nongli.term}}</span>
        </td>
      </tr>
    </table>
  </div>
</template>

<script>
import sl from "solarlunar";
export default {
  data() {
    return {
      year: 2019,
      Month: 9
    };
  },
  computed: {
    jisuan() {
      //数组内部是这个日历表的所有显示
      let arr = [];
      //计算这个月的1号是星期几
      let zhegeyue1haoxingqiji = new Date(
        this.year,
        this.Month - 1,
        1
      ).getDay();
      if (zhegeyue1haoxingqiji === 0) {
        zhegeyue1haoxingqiji = 7;
      }
      //计算上一个月有几天
      let shanggeyueyoujitian = new Date(
        this.year,
        this.Month - 1,
        0
      ).getDate();
      //计算这个月有几天
      let zhegeyueyoujitian = new Date(this.year, this.Month, 0).getDate();
      console.log(zhegeyue1haoxingqiji, shanggeyueyoujitian, zhegeyueyoujitian);
      //填充数组
      //这个月的1号是星期几，则需要填充上个月几天
      for (let i = 0; i < zhegeyue1haoxingqiji - 1; i++) {
        arr.unshift({
          gongli: shanggeyueyoujitian - i,
          nongli: sl.solar2lunar(
            this.year,
            this.Month - 1,
            shanggeyueyoujitian - i
          )
        });
      }
      //填充这个月的天数
      for (let i = 1; i <= zhegeyueyoujitian; i++) {
        arr.push({
          gongli: i,
          nongli: sl.solar2lunar(this.year, this.Month, i)
        });
      }
      //数组总长42个，填充剩下的
      let i = 1;
      while (arr.length < 42) {
        arr.push({
          gongli: i,
          nongli: sl.solar2lunar(this.year, this.Month + 1, i)
        });
        i++;
      }

      console.log(arr);
      return arr;
    }
  }
};
</script>

<style>
table,
tr,
td,
th {
  border: 1px solid black;
}
table {
  border-collapse: collapse;
}

td {
  padding: 10px 20px;
}
</style>