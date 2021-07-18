<template>
    <transition name="scale" mode="out-in">
        <section v-if="show" class="result modalComponent">
            <b-container fluid="md">
                <h1 class="mb-4">Results</h1>
                <div class="content">
                    <div class="percentage is-flex">
                        <span class="value">{{ percentage }}<span class="simbolPercentage">%</span></span>
                        <p>Percentage of correct answers</p>
                    </div>
                    <div class="groupComponent mb-4">
                        <div class="component">
                            <i class="fas fa-check-circle mr-1"></i>
                            <div>
                                <span>{{ correctAnswers }}</span>
                                <p>Correct answers</p>
                            </div>
                        </div>
                        <div class="component">
                            <i class="fas fa-times-circle mr-1"></i>
                            <div>
                                <span>{{ incorrectAnswers }}</span>
                                <p>Incorrect answers</p>
                            </div>
                        </div>
                        <div class="component">
                            <i class="fas fa-list-ul mr-1"></i>
                            <div>
                                <span>{{ dataQuestionsLength }}</span>
                                <p>Total questions</p>
                            </div>
                        </div>
                    </div>
                    <b-button @click="restart" class="secondaryBtn">Restart</b-button>
                </div>
            </b-container>
        </section>
    </transition>
</template>

<script>
    import eventBus from '../eventBus'
    export default {
        data() {
            return {
                show: false,
                correctAnswers: Number,
                incorrectAnswers: Number,
                dataQuestionsLength: Number,
            }
        },
        methods:{
            restart(){
                eventBus.$emit('restartApp', true)
                this.show = false;
            }
        },
        computed: {
            percentage(){
                let result = (this.correctAnswers * 100)/this.dataQuestionsLength
                return result.toFixed(0)
            }
        },
        created() {
            eventBus.$on('showResults', (data) => {
                this.show = true
                this.correctAnswers = data.correctAnswers.length;
                this.incorrectAnswers = data.incorrectAnswers.length;
                this.dataQuestionsLength = data.dataQuestionsLength
                console.log('show')
            })
        }
    }
</script>