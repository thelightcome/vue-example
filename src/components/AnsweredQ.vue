<template>
  <div class="question">
    <span>{{ question.id + 1 }}. {{ question.text }}</span>
    <button v-on:click="open = !open">{{ question.answer ? 'change' : 'answer' }}</button>
  </div>
  <div class="answer" v-if="open">
    <input type="text" v-model="answer" v-on:keypress.enter="sendAnswer">
  </div>
  <div class="answer" v-else-if="question.answer">
    <p>{{ question.answer }}</p>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        open: false,
        answer: ''
      }
    },
    props: ['question'],
    emits: ['send'],
    methods: {
      sendAnswer() {
        this.open = false
        this.$emit('send', {
          id: this.question.id,
          answer: this.answer
        })
      }
    }
  }
</script>

<style scoped>
	.question {
		width: 100%;
		box-sizing: border-box;
		display: flex;
		justify-content: space-between;
		align-items: center;
		padding: 2px;
	}
	.question span {
		font-weight: bold;
	}
	.question button {
		border: none;
		text-transform: uppercase;
		font-size: 0.9rem;
		padding: 5px 8px;
		cursor: pointer;
		border-radius: 3px;
		background: lightgreen;
		transition: 0.3s;
	}
	.question button:hover {
		transform: translate(2px, 2px);
		box-shadow: inset 2px 2px 5px 2px rgba(0, 0, 0, 0.3);
	}
	.answer {
		padding: 5px;
		background: #f0f0f0;
	}
	.answer input {
		width: 100%;
		box-sizing: border-box;
		padding: 5px 10px;
	}
	.answer p {
		text-align: left;
		margin: 0;
	}
</style>