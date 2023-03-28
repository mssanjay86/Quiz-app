<template>
    <div class="container">
        <h1 class="heading">Quiz app</h1>

        <div v-if="!showResults" class="questions">
            <div class="question-child">
                <p class="question-text">{{ quiz[questionCount].id }}. {{ quiz[questionCount].question }}</p>
                <div class="option-styles" v-for="item, index in quiz[questionCount].options" :key="index">
                    <input name="ans1" type="radio" id="ans1" @click="setAnswer(
                        {
                            optionIndex: index,
                            option: item,
                            quizId: questionCount
                        }
                    )"><label class="margin-left" for="ans1">{{ item }}</label>
                </div>

            </div>


        </div>

        <div v-else class="results">
            <p>You got {{ noOfCorrect }} out of 5</p>
            <p v-if="noOfCorrect >= 3">Congratulations</p>
            <p v-else>Better luck next time.</p>
        </div>



        <div class="button-container">
            <button v-if="showBtn" @click="nextQuestion()">Next</button>
            <button v-if="showBtn">Reset</button>
            <button v-if="displayShowResultsBtn" @click="displayResults">Show Results</button>
        </div>
    </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';

interface answers {
    optionIndex: number,
    option: string,
    quizId: number
}
const tempAnswer = ref<answers>()
const answerData = ref<answers[]>([]);

const setAnswer = ((Data: answers) => {
    tempAnswer.value = Data
})

const pushAnswer = ((val: answers) => {

    answerData.value.push(val);

})
const quiz = ref([
    {
        id: 1,
        question: "Which of the following type of variable is visible everywhere in your JavaScript code?",
        options: ["Global variable", "Local variable", "Both of the above", "None of the above"],
        correctAnswer: "Global variable"

    },
    {
        id: 2,
        question: "Which built-in method combines the text of two strings and returns a new string?",
        options: ["append()", "concat()", "attach()", "None of the above",],
        correctAnswer: "concat()"
    },
    {
        id: 3,
        question: "Which of the following function of String object is used to match a regular expression against a string?",
        options: ["concat()", "match()", "search()", "replace()",],
        correctAnswer: "match()"
    }, {
        id: 4,
        question: "Which of the following methods is used to access HTML elements using Javascript?",
        options: ["getElementById()", "getElementsByClassName()", "Both A and B", "None of the above",],
        correctAnswer: "Both A and B"
    }, {
        id: 5,
        question: "How can a datatype be declared to be a constant type?",
        options: ["var", "let", "const", "None of the above",],
        correctAnswer: "const"
    }
])
const questionCount = ref(0);
const showBtn = ref(true);
const displayShowResultsBtn = ref(false);
const showResults = ref(false);


const nextQuestion = () => {
    if (tempAnswer.value && quiz.value.length >= answerData.value.length - 1) {

        pushAnswer(tempAnswer.value)
    }
    if (questionCount.value < quiz.value.length - 1) {
        questionCount.value += 1;
    } else {
        showBtn.value = false;
        displayShowResultsBtn.value = true;

    }
}
const noOfCorrect = ref<number>(0);
const displayResults = () => {
    showResults.value = true;
    displayShowResultsBtn.value = false;


    answerData.value.forEach((eachAnswer) => {
        if (quiz.value[eachAnswer.quizId].correctAnswer == eachAnswer.option) {

            noOfCorrect.value++;
        }

    })

}

</script>
