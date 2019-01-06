<template>
    <div class="container">
        <p class="alert alert-warning">{{ question }} </p>
        <div class="row">
            <div
                v-for="select in allAnswers"
                :key="select"
                class="col-lg-3"
            >
                <button class="btn btn-primary" @click.prevent="checkAnswer(select)">{{ select }}</button></div>
            </div>
        <div>
            {{ score }}
        </div>
        <div v-show="message.length">
            {{ message }}
        </div>
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
                points: 3
            }
        },
        created() {
            this.generateQuestion();
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
                let first = 0;
                let second = 0;
                do {
                    first = Math.floor(Math.random() * 200) + 1;
                    second = Math.floor(Math.random() * 200) + 2;
                    this.answer = first / second;
                }
                while ( first % second !== 0 );
                this.question =  first + ' / ' + second + ' ='
            },
            generateAnswers() {
                this.allAnswers.push(this.answer);
                const possibilities = ['+', '-'];
                this.shuffleArray(possibilities);
                while (this.allAnswers.length < 4) {
                    let wrongAnswer = 0;
                    switch (possibilities[0]) {
                        case '+':
                            wrongAnswer = this.answer + Math.floor(Math.random() * 20);
                            break;
                        case '-':
                            wrongAnswer = this.answer - Math.floor(Math.random() * 20);
                            break;
                    }
                    if (!this.allAnswers.includes(wrongAnswer) && wrongAnswer > 0) {
                        this.allAnswers.push(wrongAnswer);
                    }
                }
                this.shuffleArray(this.allAnswers);
            },
            checkAnswer(select) {
                if (+select === this.answer) {
                    this.changeQuestion();
                } else {
                    this.points--;
                    if (this.points === 0 ) {
                        return this.changeQuestion();
                    }
                    this.message = 'Spróbuj jeszcze raz!'
                }
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
                this.message = '';
                this.generateQuestion();
            }
        }
    }
</script>

<style scoped>

</style>