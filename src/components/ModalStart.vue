<template>
    <transition name="scale" mode="out-in">
        <section v-if="show" class="start modalComponent">
            <div class="is-flex-column">
                <i class="fas fa-question-circle mb-2"></i>
                <h1 class="mb-3">Quiz</h1>
                <b-button @click="start" class="secondaryBtn">Start</b-button>
            </div>
        </section>
    </transition>
</template>

<script>
    import eventBus from '../eventBus'
    export default {
        data() {
            return {
                show: true
            }
        },
        methods: {
            start() {
                this.show = false
                eventBus.$emit('startQuiz', true)
            }
        }
    }
</script>