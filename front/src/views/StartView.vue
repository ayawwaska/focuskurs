<template>
    <div>
        <div class="header clearfix" >
 Zadzwonimy do Ciebie w ciągu 26 sekund.
        </div>
    <label class="form-label clearfix" for="form-number">
 Wprowadź numer
    </label>
    <input v-model="number"
 class="form-number clearfix" id="form-number"/>
    <div class="call-button" @click="call()">
 Zadzwoń teraz
    </div>
    </div>
</template>


<script>
export default {
 data() {
     return {
         number: ''
    }
 },
 methods: {
    async call() {
  await fetch(`${process.env.VUE_APP_SERVER_URL}/call`, {    method: "POST",
    headers: { "Content-type": "application/json; charset=UTF-8" },    body: JSON.stringify({number: this.number}),
    mode: 'no-cors'  })
    .then(response => {      if (response.status !== 404) {
        response.json();      }
    })    .then(responseJson => {
      if (responseJson?.id) {        this.$router.push({name: 'Ringing', params: {callsId: responseJson.id}});
      } else {        // this.$router.push({name: 'Error'});
      }    })
    .catch(() => {      // this.$router.push({name: 'Error'});
    });
    
    }
  },
}
</script>