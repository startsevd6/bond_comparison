<template>
  <a :href="linkMOEX + code" class="card">
    <div class="container">
      <h3>{{ title }}</h3>
      <h4>{{ code }}</h4>
    </div>
    <div class="benefits">
      <div class="benefit">
        <span>Доходность в день</span>
        <span><strong>{{ percent }} ₽</strong></span>
      </div>
      <div class="benefit">
        <span>Доход к погашению</span>
        <span><strong>{{ ytm }} ₽</strong></span>
      </div>
      <div class="benefit">
        <span>Цена</span>
        <span><strong>{{ price }} ₽</strong></span>
      </div>
    </div>
  </a>
</template>

<script>
export default {
  props: {
    title: {
      type: String,
      required: true
    },
    code: {
      type: String,
      required: true
    },
    percent: {
      type: Number,
      required: true
    },
    ytm: {
      type: Number,
      required: true
    },
    price: {
      type: Number,
      required: true
    },
  },
  data() {
    return {
      linkMOEX: 'https://www.moex.com/ru/issue.aspx?code='
    }
  }
}
</script>

<style scoped>
a {
  text-decoration: none;
}

h3, h4, span {
  color: #000000;
}

h4 {
  color: rgba(0, 0, 0, 0.8);
  align-self: flex-end
}

strong {
  font-size: 36px;
}

.container {
  display: flex;
  gap: 20px;
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