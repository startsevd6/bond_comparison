<template>
  <div class="card">
    <span>Комиссия: </span>
    <input type="text" v-model="commission" @input="validateCommission" placeholder="Введите значение" />
    <span>%</span>
    <button @click="getCommission">Сохранить</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      commission: ""
    }
  },
  methods: {
    validateCommission() {
      if (this.commission.includes(",")) {
        alert("Нельзя писать запятые, используйте точки")
        this.commission = this.commission.replace(",", "")
      }
    },
    getCommission() {
      this.$emit('commission-saved', this.commission)
    }
  }
}
</script>

<style scoped>
.card {
  display: flex;
  flex-direction: row;
  gap: 30px;
  margin: 20px;
  border-radius: 20px;
  padding: 20px;
  width: 45%;
  text-align: left;
  z-index: 1;
  background-color: #ffffff;

}
</style>