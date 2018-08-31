<template>
    <div class="container">
        <div class="row justify-content-center">
             <div class="col-lg-6">
                 <div v-for="(question, qIndex) in questions" class="text-left form-check">
                     <!--<input class="form-check-input" type="radio" name="exampleRadios" id="exampleRadios1" value="option1" checked>-->
                     <label class="form-check-label">
                         {{ question.question_text }}
                     </label>
                     <div v-for="(choice, index) in question.choice" class="form-check">
                         <input class="text-left form-check-input" v-model="answer[qIndex].answer" type="radio" :name="question.question_id" :id="choice.choice_description+qIndex" :value="choice.alphabet" checked>
                         <label class="text-left form-check-label" :for="choice.choice_description+qIndex">
                             {{ choice.choice_description }}
                         </label>
                     </div>
                     <br>
                     <br>
                 </div>
             </div>
        </div>
    </div>
</template>
<script>
    import axios from 'axios'
    export default {
        props: ['backend'],
        data() {
          return {
              questions: [],
              answer: []
          }
        },
        computed: {

        },
        methods: {
            getQuestion: function() {
                axios.get(this.backend+'/api/question')
                    .then(res => {
                        this.questions = res.data.data
                        console.log(this.questions)
                        this.questions.forEach(question => {
                            this.answer.push({question_id: question.question_id, answer: ''});
                        })
                    })
                    .catch(err => {
                        console.log(err)
                    })
            }
        },
        mounted() {
            console.log(this.backend+'/api/question')
            this.getQuestion()
        }
    }
</script>