<template>
  <div id="app">
    <h1>Stefan Gligorevic's Currency Exchange App</h1>
    <div class="container">
      <div>
        <h4>1 {{ currency_one }} equals</h4>
        <h2>{{ amountTwo }} {{ currency_two }}</h2>
      </div>

      <div>
        <input
          type="number"
          name="input-one"
          id="input-one"
          v-model="amountOne"
          @input="calculateExchange()"
        />

        <select
          name="first-currency"
          id="first-currency"
          @change="calculateExchange()"
          v-model="currency_one"
        >
          <option v-for="option in options" :value="option.value" :key="option.value">{{ option.label }}</option>
        </select>
      </div>
    
      <div>
        <input
          type="number"
          id="amount-two"
          placeholder="0"
          v-model="amountTwo"
          disabled
        />

        <select
          id="currency-two"
          @change="calculateExchange()"
          v-model="currency_two"
        >
          <option v-for="option in options" :value="option.value" :key="option.value">{{ option.label }}</option>
        </select>
      </div>

    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      currency_one: "USD",
      currency_two: "EUR",
      rate: "",
      amountOne: 1,
      amountTwo: 0,
      options: [
        {
          "value": "EUR",
          "label": "Euro"
        },
        {
          "value": "GBP",
          "label": "British Pound"
        },
        {
          "value": "JPY",
          "label": "Japanese Yen"
        },
        {
          "value": "NOK",
          "label": "Norwegian Krone"
        },
        {
          "value": "RSD",
          "label": "Serbian Dinar"
        },
        {
          "value": "USD",
          "label": "US Dollar"
        }
      ]
    };
  },

  methods: {
    calculateExchange() {
      fetch(
        `https://v6.exchangerate-api.com/v6/${"9809b1cec3fb53ce2b3f3f6a"}/latest/${this.currency_one}`
      )
        .then((res) => res.json())
        .then((data) => {
          this.rate = data.conversion_rates[this.currency_two];
          this.amountTwo = this.amountOne * this.rate.toFixed(2);
        });
    }
  },

  /* Good to have fetch calls in the mounted hook,
     since it allows data to be accessed immediately after the first render
  */
  mounted() {
    this.calculateExchange();
  }
};
</script>

<style>
#app {
  display: flex;
  flex-direction: column;
  align-content: center;
  align-items: center;
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
  width: 100%;
  height: 100%;
}

h4 {
  color: gray;
}

.container {
  width: 50%;
  height: 100%;
  text-align: center;
}

select {
  padding: 5px;
  margin: 5px;
  height: 33px;
  border: 1px solid rgba(0, 0, 0, 0.5);
  background: none;
  outline: none;
}

input {
  padding: 5px;
  font-size: 18px;
  border: 1px solid rgba(0, 0, 0, 0.5);
  outline: none;
}
</style>
