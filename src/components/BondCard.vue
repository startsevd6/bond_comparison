<script setup>

</script>

<template>
  <div class="card">
    <h3>Название облигации</h3>
    <div class="benefits">
      <div class="benefit">
        <span>Доходность</span>
        <span><strong>число %</strong></span>
      </div>
      <div class="benefit">
        <span>Доход к погашению</span>
        <span><strong>число ₽</strong></span>
      </div>
      <div class="benefit">
        <span>Цена</span>
        <span><strong>число ₽</strong></span>
      </div>
    </div>
  </div>
</template>

<style scoped>
h3, span {
  color: #000000;
}

strong {
  font-size: 36px;
}

.card {
  display: flex;
  flex-direction: column;
  gap: 30px;
  margin: 20px;
  border-radius: 20px;
  padding: 20px;
  width: 45%;
  text-align: left;
  z-index: 1;
  background-color: #ffffff;

}

.benefits {
  display: flex;
  gap: 20px;
}

.benefit {
  display: flex;
  gap: 5px;
  flex-direction: column;
}
</style>