<template>
  <header>
    <a href="/" class="logo">Vue-example</a>
    <nav>
      <ul class="navigator">
        <li>
          <a href="home" v-on:click.prevent="page = 'home'">Home</a>
        </li>
        <li>
          <a href="about" v-on:click.prevent="page = 'about'">About</a>
        </li>
      </ul>
    </nav>
  </header>
  <div class="page" v-if="page === 'home'">
    <h1>Home Page</h1>
    <form>
        <input type="text" v-model="question" placeholder="Ask your question?">
        <label>{{ inputError }}</label>
        <button @click.prevent="fetched">Request</button>
    </form>
    <h4 v-if="questions.length">Your question</h4>
    <ol class="question-list">
      <li v-for="question in questions" :key="question">
        <span>{{ question.text }}</span>
      </li>
    </ol>
  </div>
  <div class="page" v-else-if="page === 'about'">
    <h1>About Me</h1>
    <ul class="answered-questions">
      <li v-for="question in questions" :key="question.id">
        <AnsweredQ v-bind:question="question" v-on:send="saveAnswer($event)" />
      </li>
    </ul>
  </div>
  <div class="page" v-else>
    <h1>Page Not Founded</h1>
  </div>
  <div class="alert" v-if="alertMessage">
    <p>Ooops!</p>
    <p>{{ alertMessage }}</p>
    <button v-on:click="alertMessage = ''">OK</button>
  </div>
</template>

<script>
import AnsweredQ from './components/AnsweredQ'

export default {
  name: 'App',
  data() {
    return {
      page: 'home',
      question: '',
      questions: [
        {
          id: 0,
          text: 'What is your name?',
          answer: 'Nurgeldi'
        },
        {
          id: 1,
          text: 'How your old?',
          answer: '24'
        },
      ],
      inputError: '',
      alertMessage: ''
    }
  },
  methods: {
    async fetched() {
      try {
        if (!this.question) return
        if (!(/\?$/.test(this.question))) {
          this.inputError = 'Did you forget to put it "?"'
          setTimeout(() => {this.inputError = ''}, 2000)
          return
        }

        this.questions.push({
          id: this.questions.length,
          text:  this.question,
          answer: ''
        })
        this.question = ''
      }
      catch (e) {
        this.alertMessage = `${e}`
      }
    },
    saveAnswer(data) {
      this.questions.map((elem) => {
        if (elem.id === data.id) {
          elem.answer = data.answer
        }
        return elem
      })
    }
  },
  components: {
    AnsweredQ
  }
}

</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  height: 100%;
}
body {
  width: 100%;
  height: 100vh;
  padding: 0;
  margin: 0;
  font-size: 16px;
}
ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
ol {
  margin: auto;
}
a {
  text-decoration: none;
  color: #020202;
}
a:hover {
  color: #020202;
}
h1 {
  margin: 0;
  margin-bottom: 32px;
  font-size: 1.8rem;
}
h4 {
  margin: 12px;
}
header {
  width: 100%;
  min-height: 65px;
  padding: 5px 12px;
  box-sizing: border-box;
  display: flex;
  align-content: center;
  align-items: center;
  justify-content: space-between;
  flex-wrap: wrap;
}
.logo {
  text-transform: uppercase;
  font-weight: bold;
}
.navigator li {
  display: inline-block;
  margin: 15px;
}
.navigator li a {
  display: block;
  padding: 4px 8px;
  transition: 0.3s;
}
.navigator li a:hover {
  box-shadow: 5px 2px 10px 3px rgba(0, 0, 0, 0.3);
}
.page {
  padding: 15px 12px;
  overflow-y: auto;
  text-align: center;
}
form {
  margin: auto;
  width: 100%;
  max-width: 450px;
  box-sizing: border-box;
  padding: 0 10px;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center;
}
form input {
  width: 100%;
  font-size: 1rem;
  box-sizing: border-box;
  padding: 3px 25px 3px 10px;
  margin-bottom: 25px;
}
form input ~ label {
  margin-bottom: 12px;
}
form button {
  padding: 9px 15px;
  border-radius: 5px;
  border: 1px solid transparent;
  cursor: pointer;
  color: #020202;
  text-transform: uppercase;
  font-weight: 500;
  transition: 0.3s;
}
form button:focus {
  border-color: #020202; 
}
form button:hover {
  box-shadow: 2px 2px 4px 2px rgba(0, 0, 0, 0.4);
}
.question-list {
  overflow-y: auto;
  display: inline-block;
}
.alert {
  position: fixed;
  top: 35%;
  left: 50%;
  transform: translate(-50%, -50%);
  border: 2px solid #c0c0c0;
  background: #f0f0f0;
  padding: 16px 25px;
  text-align: center;
  width: 90%;
  max-width: 350px;
  min-height: 160px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
}
.alert button {
  padding: 6px 12px;
  cursor: pointer;
  text-align: center;
  line-height: 25px;
  vertical-align: middle;
  text-transform: uppercase;
}
.answered-questions {
  width: 100%;
  max-width: 320px;
  margin: 0 auto;
}
.answered-questions li {
  width: 100%;
  margin-bottom: 5px;
}
</style>
