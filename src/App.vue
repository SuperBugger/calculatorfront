<template>
  <div id="app">
    <h1>Calculator App</h1>

    <div>
      <label>Enter expression:</label>
      <input v-model="expression" />
      <button @click="calculate">Calculate</button>
    </div>

    <div v-if="result !== null">
      <p>Result: {{ result }}</p>
    </div>

    <div v-if="errorMessage !== null">
      <p style="color: red;">Error: {{ errorMessage }}</p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      expression: "",
      result: null,
      errorMessage: null,
    };
  },
  methods: {
    calculate() {
      const apiUrl = "https://localhost:5001/api/calculator/evaluate-expression";

      fetch(apiUrl, {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({
          values: [],
          operators: [],
        }),
        mode: 'cors',
      })
          .then((response) => response.json())
          .then((data) => {
            if (data.errorMessage) {
              this.result = null;
              this.errorMessage = data.errorMessage;
            } else {
              this.result = data.result;
              this.errorMessage = null;
            }
          })
          .catch((error) => {
            console.error("Error:", error);
            this.result = null;
            this.errorMessage = "Failed to communicate with the server.";
          });
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

input {
  padding: 5px;
  margin-right: 10px;
}

button {
  padding: 5px 10px;
  cursor: pointer;
}
</style>
