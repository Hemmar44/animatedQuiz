<template>
    <div class="container">
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
        <div class="alert alert-danger" v-show="message.length">
            {{ message }}
        </div>
        <div class="alert alert-primary"><b>{{ to_go }}</b></div>
    </div>
</template>

<script>
    const initData = {
            answer: '',
            allAnswers: [],
            question: '',
            score: 0,
            message: '',
            points: 3,
            to_go: 5
    };
    export default {
        name: "QuizBase",
        data() {
            return initData;
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
                this.to_go--;
                if (this.to_go < 0) {
                    this.gameOver();
                }
            },
            gameOver() {
                alert('Koniec');
            }
        }
    }
</script>

<style scoped>
div.alert {
    margin-top:  15px;
}
</style>