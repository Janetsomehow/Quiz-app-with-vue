<template>
  <div id="quiz">
  <h1>{{ quiz.title }}</h1>
  <!-- index is used to check with current question index -->
  <div v-for="(question, index) in quiz.questions" v-bind:key="question.id">
    <!-- Hide all questions, show only the one with index === to current question index -->
    <div v-show="index === questionIndex">
      <h2>{{ question.text }}</h2>
      <ol>
        <li v-for="response in question.responses" v-bind:key="response.id">
          <label>
            <!-- The radio button has three new directives -->
            <!-- v-bind:value sets "value" to "true" if the response is correct -->
            <!-- v-bind:name sets "name" to question index to group answers by question -->
            <!-- v-model creates binding with userResponses -->
            <input type="radio" 
                   v-bind:value="response.correct" 
                   v-bind:name="index" 
                   v-model="userResponses[index]"> {{response.text}}
          </label>
        </li>
      </ol>
      <!-- The two navigation buttons -->
      <!-- Note: prev is hidden on first question -->
      <button v-if="questionIndex > 0" v-on:click="prev">
        prev
      </button>
      <button v-on:click="next">
        next
      </button>
    </div>
  </div>
  <div v-show="questionIndex === quiz.questions.length">
    <h2>
    Quiz finished
  </h2>
    <p>
      Total score: {{ score() }} / {{ quiz.questions.length }}
    </p>
  </div>
  </div>
 
  
</template>


<script>
const quiz = {
  title: 'My quiz',
  questions: [
    {
      text: "Who is the founder of twitter",
      responses: [
        {text: 'Mark Zuckerberg'},
        {text: 'Bill Gates'},
        {text: 'Jack', correct: true}, 
      ]
    }, {
      text: "What's the highest flying bird?",
      responses: [
        {text: 'The Ruppell Griffon Vulture', correct: true}, 
         {text: 'The Bald Eagle'}, 
        {text: 'The Pelligrine Falcon'}, 
      ]
    },{
      text: "All of these are German cars except",
      responses: [
        {text: 'BMW', correct: true}, 
         {text: 'Audi'}, 
        {text: 'Honda'}, 
      ]
    },{
    text: "Who was the first man on the moon",
      responses: [
        {text: 'Neil Armstrong', correct: true}, 
         {text: 'Isaac Newton'}, 
        {text: 'Buzz Aldrin'}, 
      ]
    
    } 
  ]
};
  
  
 new Vue({
  el: '#quiz',
  data: {
    quiz: quiz,
    // Store current question index
    questionIndex: 0,
    // An array initialized with "false" values for each question
    // It means: "did the user answered correctly to the question n?" "no".
    userResponses: Array(quiz.questions.length).fill(false)
  },
  // The view will trigger these methods on click
  methods: {
    
    next: function() {
      this.questionIndex++;
    },
    
    prev: function() {
      this.questionIndex--;
    },
    
    score: function() {
      return this.userResponses.filter(function(val) { return val }).length;
    }
  }
});

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
</style>
