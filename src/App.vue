<template>
<div id="app">

    <h1>Bitcoin Price Index</h1>

    <section v-if="errored">
        <p>We're sorry, we're not able to retrieve this information at the moment, please try back later</p>
    </section>

    <section v-else>

        <div v-if="loading">Loading</div>

        <div v-else v-for="(currency, index) in info" :key="index" class="currency">
            <span class="description"> {{ currency.description }} : </span>

            <span class="detail">
                <span class="symbol" v-html="currency.symbol"></span>
                {{currency.rate_float | currencyDecimal}}
            </span>

        </div>

    </section>
</div>
</template>

<script>
import axios from 'axios';

export default {
    name: 'app',
    data() {
        return {
            info: null,
            loading: true,
            errored: false
        }
    },
    filters: {
        currencyDecimal(val) {
            return val.toFixed(2)
        }
    },
    mounted() {
        axios
            .get('https://api.coindesk.com/v1/bpi/currentprice.json')
            .then(response => (this.info = response.data.bpi))
            .catch(error => {
                console.log(error)
                this.errored = true
            })
            .finally(() => this.loading = false)
    }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css?family=Poppins&display=swap');

body {
    background: linear-gradient(0deg, rgb(31, 31, 31) 24%, rgb(49, 49, 49) 98%);
    top: 0;
    left: 0;
    margin: 0;
    padding: 0;
    height: 100vh;
    width: 100vw;
    font-family: 'Poppins', sans-serif;
}

#app {
    background: #141414;
    color: white;
    width: 40%;
    height: 30%;
    left: 50%;
    top: 40%;
    transform: translate(-50%, -50%);
    position: absolute;
    padding: 30px;
    border-radius: 10px;
    border: 2px solid rgb(45, 223, 29);
    box-shadow: 0px 5px 15px 7px rgba(141, 255, 10, 0.25);
}

.symbol {
  color: rgb(134, 233, 69);;
}

.description {
  opacity: .75;
}

.currency {
  font-size: 18px;
  padding: 10px 0;
}

h1 {
  font-size: 35px;
  border-bottom: 2px solid rgb(137, 255, 58);
}
</style>
