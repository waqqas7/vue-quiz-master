<template>
    <div class="question-box-container">
        <b-jumbotron>
            <template slot="lead">
                {{ currentQuestion.question }}
            </template>

            <hr class="my-4" />

            <b-list-group>
                <b-list-group-item v-for="(answer, index) in shuffledAnswers" :key="index" @click.prevent="selectAnswer(index)" :class="answerClass(index)">
                    {{ answer }}
                </b-list-group-item>
            </b-list-group>

            <b-button @click="prev(); decre();" variant="success">
                Prev Question
            </b-button>
            <b-button variant="primary" @click="submitAnswer" :disabled="selectedIndex === null || answered || att[c]">
                Submit
            </b-button>
            <b-button @click="next(); incre();" variant="success">
                Next Question
            </b-button>
        </b-jumbotron>
    </div>
</template>

<script>
    import _ from 'lodash'

    export default {
        props: {
            currentQuestion: Object,
            next: Function,
            prev: Function,
            increment: Function
        },
        data: function() {
            return {
                selectedIndex: null,
                correctIndex: null,
                shuffledAnswers: [],
                answered: false,
                c: 0,
                att: [false, false, false, false, false, false, false, false, false, false]
            }
        },
        watch: {
            currentQuestion: {
                immediate: true,
                handler() {
                    this.selectedIndex = null
                    this.answered = false
                    this.shuffleAnswers()
                }
            }
        },
        methods: {
            selectAnswer(index) {
                this.selectedIndex = index
            },
            submitAnswer() {
                let isCorrect = false

                if (this.selectedIndex === this.correctIndex) {
                    isCorrect = true
                }
                this.answered = true

                this.increment(isCorrect)

                if (this.att[this.c] == false) {
                    this.att[this.c] = true
                }
            },
            incre() {
                if (this.c < 9)
                    this.c++
            },
            decre() {
                if (this.c > 0)
                    this.c--
            },
            shuffleAnswers() {
                let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
                this.shuffledAnswers = _.shuffle(answers)
                this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
            },
            answerClass(index) {
                let answerClass = ''

                if (!this.answered && this.selectedIndex === index) {
                    answerClass = 'selected'
                } else if (this.answered && this.correctIndex === index) {
                    answerClass = 'correct'
                } else if (this.answered &&
                    this.selectedIndex === index &&
                    this.correctIndex !== index
                ) {
                    answerClass = 'incorrect'
                }

                return answerClass
            }
        }
    }

</script>

<style scoped>
    .list-group {
        margin-bottom: 15px;
    }

    .list-group-item:hover {
        background: #EEE;
        cursor: pointer;
    }

    .btn {
        margin: 0 5px;
    }

    .selected {
        background-color: lightblue;
    }

    .correct {
        background-color: lightgreen;
    }

    .incorrect {
        background-color: red;
    }

</style>
