<!-- HTML -->
<template>
  <div id="app">
    <main>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="Type a disease"
          v-model="query"
          @keypress="fetchLexigram"
        />
      </div>
      <div class="lexigram-wrap" v-if="matches.length > 0">
        <div class="match-box">Match</div>

        <div class="lexigram-box">
          <div class="label">{{ matches[0].label }}</div>
          <div
            class="classification"
            v-for="{ id, label } in matches[0].classification.subs"
            :key="id"
          >
            {{ label }}
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<!-- SCRIPTS -->
<script>
export default {
  name: "App",
  data() {
    //fetching API from https://docs.lexigram.io/
    return {
      api_key:
        "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhdSI6Imx4ZzphcGkiLCJzYyI6WyJrZzpyZWFkIiwiZXh0cmFjdGlvbjpyZWFkIl0sImFpIjoiYXBpOmUyZjI4Mzc2LTJlMDAtMzdjZi0yODQyLWQ5MzY4MzYwZjM5YSIsInVpIjoidXNlcjpmNGNjMTIwNy04N2UwLThkZjMtMmI3Yi01NTRmZjhjZjAxZjgiLCJpYXQiOjE2Njc2MjAzNDl9.ekDWy7NFkC_FrKuSEFLhasjripN1mGl_cC3Q3Nt8w9U",
      url_base: "https://api.lexigram.io/v4/",
      query: "",
      matches: [],
    };
  },
  methods: {
    fetchLexigram(e) {
      if (e.key == "Enter") {
        fetch(`${this.url_base}/extract/entities`, {
          method: "POST",
          headers: {
            Authorization: `Bearer ${this.api_key}`,
            "Content-Type": "application/json",
          },
          body: JSON.stringify({
            text: this.query,
            withContext: true,
            withMatchLogic: "ignore-length",
            withText: true,
          }),
        })
          .then((res) => {
            return res.json();
          })
          .then(this.setResults);
      }
    },
    setResults(results) {
      this.matches = results.matches;
    },
  },
};
</script>

<!-- STYLING -->
<style>
@import url("https://fonts.googleapis.com/css2?family=Montserrat&display=swap");

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: "Montserrat", sans-serif;
}

#app {
  background-image: url("./assets/alma-bg.png");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

main {
  min-height: 100vh;
  padding: 25px;

  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.75)
  );
}

.search-box {
  width: 100%;
  margin-bottom: 30px;
}

.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;

  color: #313131;
  font-size: 20px;
  appearance: none;
  border: none;
  outline: none;
  background: none;

  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.2);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}

.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.5);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;
}

.match-box {
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  /* text-shadow: 1px 3px rgba(0, 0, 0, 0.25); */
}

.lexigram-box {
  text-align: center;
}

.lexigram-box .label {
  display: inline-block;
  padding: 10px 25px;
  color: aliceblue;
  font-size: 95px;
  font-weight: 500;
  background-color: rgba(255, 255, 255, 0.2);
  border-radius: 16px;
  margin: 30px 0px;
}

.lexigram-box .classification {
  color: #fff;
  font-size: 40px;
  font-weight: 500;
}
</style>
