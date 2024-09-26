<template>
  <CommissionSettings @commission-saved="updateCommission"></CommissionSettings>

  <BondCard
      v-for="bond in bonds"
      :key="bond.code"
      :title="bond.title"
      :code="bond.code"
      :percent="bond.percent"
      :ytm="bond.ytm"
      :price="bond.price"
  ></BondCard>
  <h6>Не является индивидуальной инвестиционной рекомендацией</h6>
</template>

<script>

import BondCard from "@/components/BondCard.vue";
import CommissionSettings from "@/components/CommissionSettings.vue";
import axios from "axios";

export default {
  name: 'App',
  data() {
    return {
      bonds: [
        {
          title: 'Название облигации',
          code: 'SU26222RMFS8',
          percent: 'загрузка',
          ytm: 'загрузка',
          price: 'загрузка'
        },
      ],
      commission: ""
    }
  },
  components: {
    CommissionSettings,
    BondCard
  },
  mounted() {
    this.fetchDataFromServer(this.bonds[0]);
  },
  methods: {
    updateCommission(value) {
      this.commission = value;
      this.fetchDataFromServer(this.bonds[0])
    },
    async fetchDataFromServer(bond) {
      try {
        const xmlStringName = await axios.get('https://iss.moex.com/iss/securities.xml?q=' + bond.code)
        const parser = new DOMParser();
        const xmlDocName = parser.parseFromString(xmlStringName.data, "text/xml")
        const rowsName = xmlDocName.getElementsByTagName('row')
        bond.title = rowsName[0].getAttribute('shortname')

        let TODAY_DATE = new Date
        let day = TODAY_DATE.getDay().toString()
        if (day === '1') {
          TODAY_DATE = new Date(Date.now()-(86400000*3))
        } else if (day === '7') {
          TODAY_DATE = new Date(Date.now()-(86400000*2))
        }
        console.log(TODAY_DATE)
        let year = TODAY_DATE.getFullYear().toString()
        let month = (TODAY_DATE.getMonth()+1).toString()
        day = TODAY_DATE.getDay().toString()
        let date = (TODAY_DATE.getDate()-1).toString()
        console.log(year, month, day, date)
        console.log('https://iss.moex.com/iss/securities/' + bond.code + '/aggregates.json?date=' + year + '-' + month + '-' + date)
        const xmlStringPrice = await axios.get('https://iss.moex.com/iss/securities/' + bond.code + '/aggregates.json?date=' + year + '-' + month + '-' + date)
        console.log(xmlStringPrice)
        let volume = xmlStringPrice.data.aggregates.data[0][5]
        let value = xmlStringPrice.data.aggregates.data[0][6]

        let commission = this.commission
        if (value !== 0) {
          bond.price = Number((volume / value * (1+(commission/100))).toFixed(2))
        }

        const xmlStringYtm = await axios.get('https://iss.moex.com/iss/securities/' + bond.code + '.xml')
        const xmlDocYtm = parser.parseFromString(xmlStringYtm.data, "text/xml")
        const rowsYtm = xmlDocYtm.getElementsByTagName('row')
        bond.ytm = Number(Number((Number(rowsYtm[15].getAttribute('value')) + Number(rowsYtm[20].getAttribute('value')) - bond.price)).toFixed(2))
        bond.percent = Number((Number(bond.ytm / Number(rowsYtm[13].getAttribute('value')))).toFixed(2))
      } catch (error) {
        console.error('Ошибка при получении данных', error);
      }
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
