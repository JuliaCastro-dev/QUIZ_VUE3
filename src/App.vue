<template>
  <div class="container">
    <template v-if="this.question">
      <h1 v-html="this.question"></h1>
      <template v-for="(answer, index) in this.answers" :key="index">
        <input
          :disabled="this.answerSubmitted"
          type="radio"
          name="options"
          :value="answer"
          v-model="this.chosenAnswer"
        />
        <label v-html="answer"></label><br />
      </template>
      <button
        v-if="!this.answerSubmitted"
        @click="this.submitAnswer"
        class="send"
        type="button"
      >
        Enviar
      </button>
      <section v-if="this.answerSubmitted" class="result">
        <h4 v-if="this.chosenAnswer == this.correctAnswer">
          &#10060;I'm sorry, you picked the wrong answer. The Correct is
          {{ this.correctAnswer }}
        </h4>
        <h4 v-else>
          &#9989;Congratulations
          {{ this.correctAnswer }}
        </h4>
        <button class="send" type="button">Próxima Questão</button>
      </section>
    </template>
  </div>
</template>

<script>
export default {
  name: "App",
  components: {},
  data() {
    return {
      question: undefined,
      incorrectAnswer: undefined,
      correctAnswer: undefined,
      chosenAnswer: undefined,
      answerSubmitted: false,
    };
  },
  methods: {
    submitAnswer() {
      if (!this.chosenAnswer) {
        alert("Pick onde of the options");
      } else {
        this.answerSubmitted = true;
        if (this.chosenAnswer == this.correctAnswer) {
          console("Parabens");
        } else {
          console("Errado");
        }
      }
    },
  },
  computed: {
    answers() {
      var answers = JSON.parse(JSON.stringify(this.incorrectAnswer));
      answers.splice(
        Math.round(Math.random() * answers.length),
        0,
        this.correctAnswer
      );
      return answers;
    },
  },
  created() {
    this.axios
      .get("https://opentdb.com/api.php?amount=1&category=18&difficulty=medium")
      .then((response) => {
        this.question = response.data.results[0].question;
        this.incorrectAnswer = response.data.results[0].incorrect_answers;
        this.correctAnswer = response.data.results[0].correct_answer;
      });
  },
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px auto;
  max-width: 960px;
  display: flex;
  align-content: center;
  flex-direction: column;

  .container {
    justify-content: center;
  }

  input[type="radio"] {
    margin: 12px 4px;
  }

  input[type="radio"]:checked {
    border: 1px solid coral;
  }

  label {
    font-size: 20px;
    margin: 20px;
    font-weight: 700;
    color: rgb(92, 80, 255);
  }

  label:hover {
    color: rgb(0, 106, 255);
    border: 1px solid #ffffff;
  }

  button {
    background-color: rgb(22, 192, 0);
    color: azure;
    border: none;
    padding: 10px;
    border-radius: 5px;
    font-weight: bold;
    font-size: 17px;
    width: fit-content;
    align-self: center;
    margin-top: 10px;
    padding: 8px;
    cursor: pointer;
  }
}
</style>
