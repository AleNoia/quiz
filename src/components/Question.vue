<template>

    <b-card class="cardQuestion">

        <b-card-text class="header">
            Question <span>{{ questionIndex }}</span> • <span>{{ remaining - 1 }}</span> remaining
        </b-card-text>

        <b-card-text class="question">
            {{ questionText }}
        </b-card-text>

        <b-card-text class="answers">
            <div v-for="(option, index) in this.answers" :key="option.text" class="mb-1">
                <input class="radioOption" type="radio" :id="option.text" name="options" v-show="false">
                <label  
                    :for="option.text"
                    class="answer" 
                    @click.right="answer"
                    @click="optionSelected = option.correct">
                    <span class="letter mr-2">{{ index | toLetters }}</span>
                    <p>{{ option.text }}</p>
                </label>
            </div>
        </b-card-text>

        <div class="buttonGroup">
            <b-button variant="outline-primary" class="mr-1" @click="answerLater" :disabled="remaining === 0">
                Answer Later
            </b-button>
            <b-button class="answerBtn" @click="answer" :disabled="remaining === 0">Answer</b-button>
        </div>

    </b-card>

</template>

<script>
    import Questions from '../assets/useful/questions'
    import eventBus from '../eventBus'

    export default {
        data() {
            return {
                arrayQuestions: [],
                questionText: '',
                remaining: undefined,
                questionIndex: 1,
                optionSelected: undefined,
                answers: [],
            }
        },
        filters: {
            // Inserting letters in answers
            toLetters(value) {
                if (value === 0) return 'A'
                if (value === 1) return 'B'
                if (value === 2) return 'C'
                if (value === 3) return 'D'
                if (value === 4) return 'E'
            }
        },
        methods: {
            // Shuffling arrays
            shuffle(array) {
                return array.sort(() => Math.random() - 0.5);
            },

            // Generating the questions
            generateQuestion() {
                this.arrayQuestions = this.shuffle(Questions);
                console.log(this.arrayQuestions)
                this.insertQuestion()
            },

            //Skip to next question and insert the text and the answers
            insertQuestion() {
                this.questionText = this.arrayQuestions[0].text
                this.answers = this.shuffle(this.arrayQuestions[0].answers)
            },

            // Answering
            answer() {
                let correct
                if (this.optionSelected == true) correct = true;
                else if (this.optionSelected == undefined) {
                    alert('responda')
                    return
                } else correct = false;

                this.arrayQuestions.splice(0, 1);
                this.optionSelected = undefined;
                this.questionIndex += 1;
                eventBus.$emit('dataAnswer', correct)
                this.remaining = this.remaining - 1
                correct = undefined;
                this.insertQuestion();

            },

            // Answer later
            answerLater() {
                this.arrayQuestions.push(this.arrayQuestions.splice(0, 1)[0]);
                this.insertQuestion();
            },

        },

        created() {
            this.generateQuestion(); // Generating question on startup
            this.remaining = this.arrayQuestions.length
            eventBus.$emit('dataQuestions', this.arrayQuestions.length)
        },

        watch: {
            // questionIndex(){
            //     if(this.questionIndex === 11) {
            //         // this.questionIndex = 10
            //     }
            // }
        },
    }
</script>