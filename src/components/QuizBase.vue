<template>
    <div class="container">
        <transition name="fade" appear mode="out-in">
            <div v-if="!is_running" key="start">
                <div class="alert alert-warning" v-if="lastResult > 0">
                    Twój ostatni wynik: {{lastResult}}
                </div>
                <div class="alert alert-primary">
                    <div>
                        Rozpocząć nową grę?
                        <button @click="startGame" style="margin-right: 10px" class="btn btn-primary">
                            Tak
                        </button>
                    </div>
                </div>
            </div>
            <div v-else key="game">
                <transition name="fade" appear mode="out-in">
                    <div v-if="!answered" key="question">
                        <p class="alert alert-warning">{{ question }} </p>
                        <div class="row">
                            <div
                                v-for="(select, index) in allAnswers"
                                :key="index"
                                class="col-lg-3"
                            >
                            <button class="btn btn-primary" @click.prevent="checkAnswer(select)">{{ select }}</button></div>
                            </div>
                        <div class="alert alert-success">
                            Twój wynik {{ score }}
                        </div>
                    </div>
                    <div v-else key="messages">
                        <div class="alert"
                             v-show="message.length"
                             :class="correct ? 'alert-success' : 'alert-danger'"
                        >
                            {{ message }}
                        </div>
                        <div class="alert alert-primary">Pozostało pytań: <b>{{ to_go + 1 }}</b></div>
                    </div>
                </transition>
            </div>
        </transition>
    </div>
</template>

<script>
    export default {
        name: "QuizBase",
        data() {
            return {
                answer: '',
                allAnswers: [],
                question: '',
                score: 0,
                message: '',
                points: 3,
                to_go: 3,
                is_running: false,
                lastResult: 0,
                answered: false,
                correct: false
            };
        },
        methods: {
            generateQuestion() {
                this.mathematics();
            },
            mathematics() {
                let equations = ['+', '-', '*', '/'];
                this.shuffleArray(equations);
                switch (equations[0]) {
                    case '+':
                        this.sum();
                        this.generateAnswers();
                        break;
                    case '-':
                        this.minus();
                        this.generateAnswers();
                        break;
                    case '*':
                        this.multiplication();
                        this.generateAnswers();
                        break;
                    case '/':
                        this.divide();
                        this.generateAnswers()
                }
            },
            sum() {
                let first = Math.floor(Math.random() * 100);
                let second = Math.floor(Math.random() * 100);
                this.answer = first + second;
                this.question =  first + ' + ' + second + ' ='
            },
            minus() {
                let first = 0;
                let second = 0;
                do {
                    first = Math.floor(Math.random() * 200);
                    second = Math.floor(Math.random() * 200);
                    this.answer = first - second;
                }
                while ( this.answer <= 0 );
                this.question =  first + ' - ' + second + ' ='
            },
            multiplication() {
                let first = Math.floor(Math.random() * 20);
                let second = Math.floor(Math.random() * 20);
                this.answer = first * second;
                this.question =  first + ' * ' + second + ' ='
            },
            divide() {
                let first =  Math.floor(Math.random() * 200) + 1;
                let second = Math.floor(Math.random() * 200) + 2;
                while (first % second !== 0) {
                    first = Math.floor(Math.random() * 200) + 1;
                    second = Math.floor(Math.random() * 200) + 2;
                }
                this.answer = first / second;
                this.question =  first + ' / ' + second + ' ='
            },
            generateAnswers() {
                this.allAnswers.push(this.answer);
                const possibilities = ['+', '-'];
                this.shuffleArray(possibilities);
                let wrongAnswer = 0;
                let counter = 0;
                while (this.allAnswers.length < 4) {
                    if (counter < 30) {
                    switch (possibilities[0]) {
                        case '+':
                            wrongAnswer = this.answer + Math.floor(Math.random() * 20);
                            break;
                        case '-':
                            wrongAnswer = this.answer - Math.floor(Math.random() * 20);
                            break;
                        }
                    } else {
                        wrongAnswer = Math.floor(Math.random() * 6)
                    }
                    if (!this.allAnswers.includes(wrongAnswer) && wrongAnswer > 0) {
                            this.allAnswers.push(wrongAnswer);
                    }
                    counter++;
                }
                this.shuffleArray(this.allAnswers);
            },
            checkAnswer(select) {
                if (+select === this.answer) {
                    this.message = 'Brawo! Poprawna odpowiedź';
                    this.correct = true;
                    this.changeQuestion();
                } else {
                    this.points--;
                    this.correct = false;
                    this.message = 'Spróbuj jeszcze raz!';
                    if (this.points === 0 ) {
                        this.changeQuestion();
                    }
                }
                this.answered = true;
                setTimeout(() => {
                    this.answered = false;
                },2000);
            },
            shuffleArray(array) {
                for (let i = array.length - 1; i > 0; i--) {
                    const j = Math.floor(Math.random() * (i + 1));
                    [array[i], array[j]] = [array[j], array[i]];
                }
            },
            changeQuestion() {
                this.score += this.points;
                this.points = 3;
                this.allAnswers = [];
                this.generateQuestion();
                this.to_go--;
                if (this.to_go < 0) {
                    setTimeout(() => {
                        this.gameOver();
                    },2000);
                }
            },
            gameOver() {
                this.lastResult = this.score;
                this.is_running = false;
                this.answer = '';
                this.allAnswers = [];
                this.score = 0;
                this.to_go = 2;
            },
            startGame() {
                this.is_running = true;
                this.generateQuestion();
            }
        }
    }
</script>

<style scoped>
div.alert {
    margin-top:  15px;
}
</style>