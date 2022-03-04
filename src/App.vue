<template>
<div>
  <form>
    <div class="center">
      <label><h1 class="mainQ">What's your favorite JavaScript Framework?</h1></label>
      <input type="radio" value="Vue" v-model="checked" /> Vue
      <input type="radio" value="React" v-model="checked" /> React
      <input type="radio" value="Angular" v-model="checked" /> Angular
    </div>
    <button type="button" :disabled="!checked" @click="vote">vote</button>
    <div class="center">
  
    <h1>results</h1>
    <p v-for="(value, key) of results" :key="key">{{ key }}: {{ value }}</p>
  </div>
  </form>
  <div class="center">
    <p><span v-for="(value, key) of votes" :key="key"> {{ value.text_field }}<br/></span></p>
  </div>
 </div> 
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      checked: "",
      results: {},
      votes: [],
    };
  },
  methods: {
    vote() {
      if (!localStorage.getItem("vote-result")) {
        localStorage.setItem("vote-result", JSON.stringify({}));
      }
      const { checked } = this;
      const results = JSON.parse(localStorage.getItem("vote-result"));
      if (!Object.prototype.hasOwnProperty.call(results, checked)) {
        results[checked] = 0;
      }
      results[checked]++;
      localStorage.setItem("vote-result", JSON.stringify(results));
      this.results = JSON.parse(localStorage.getItem("vote-result"));
    },
  },
  mounted: function() {
    fetch('http://beta.sheehanweb.com/index.php/wp-json/vote/v1/post')
      .then((r) => r.json())
      .then((res) => this.votes = res.map(x => x.acf));
      console.log('the component is now mounted');
  }
};

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
.mainQ {
  padding-top:50px;
  /* font-size:5rem; */
}
.center {
  text-align:center;
  margin:0 auto;
  
}
button {
  border: none;
  padding:20px 40px;
  margin:20px auto;
  display:block;
}
</style>
