<template>
    <div class="container">
        <div class="row justify-content-center text-left">
             <div class="col-lg-6">
                 <label for="kad">Nombor kad matriks</label>
                 <input v-model="user_id" class="form-control form-control-md" id="kad" type="text" placeholder="CB180xxx">
                 <br>
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
                 <button @click="submit" class="btn btn-success btn-block">I'm done!</button>
                 <br>
                 <br>
             </div>
        </div>
    </div>
</template>
<script>
    import axios from 'axios'
    import swal from 'sweetalert2'
    export default {
        props: ['backend'],
        data() {
          return {
              questions: [],
              answer: [],
              user_id: ''
          }
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
            },
            submit: function() {
                axios.post(this.backend+'/api/submit', {
                    data: this.answer,
                    user_id: this.user_id
                })
                    .then(res => {
                        console.log(res)
                        if(res.data.result >= 60) {
                            swal(
                                'Congrats!',
                                'You scored <strong>'+res.data.result+'%</strong> overall!',
                                'success'
                            )
                        } else if(res.data.result < 60 && res.data.result >= 21) {
                            swal(
                                'You can do better!',
                                'You scored <strong>'+res.data.result+'%</strong> overall!',
                                'success'
                            )
                        } else if(res.data.result <= 21) {
                            swal(
                                'Try to pay more attention in class!',
                                'You scored <strong>'+res.data.result+'%</strong> overall!',
                                'success'
                            )
                        }
                    })
                    .catch(err => {
                        console.log(err.response.status)
                        if(err.response.status === 422) {
                            swal(
                                'Incomplete form!',
                                'Please complete the form!',
                                'error'
                            )
                        }
                    })
                    .finally(res => {
                        this.answer = []
                        this.questions.forEach(question => {
                            this.answer.push({question_id: question.question_id, answer: ''});
                        })
                    })
            }
        },
        mounted() {
            console.log(this.backend+'/api/question')
            this.getQuestion()
        }
    }
</script>