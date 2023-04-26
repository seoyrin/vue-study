<template>
    <div>
        <h1>{{result}}</h1>
        <form @submit.prevent="onSubmitForm"> <!-- v-on:submit + e.preventDefault와 동일한 표현 :: @submit.prevent -->
            <input ref="answer" minlength="4" maxlength="4" v-model="value" />
            <button>입력</button>
        </form>
        <div>시도 {{tries.length}}</div>
        <ul>
            <li v-for="t in tries">
                <div>{{t.try}}</div>
                <div>{{t.result}}</div>
            </li>
        </ul>
    </div>
</template>

<script>
    // 4자리 랜덤숫자 뽑기
    /*  methods: {
            getNumbers: () => { ~~ }
        } => 메소드로도 사용 가능 :: 사용할 땐, this.getNumbers()
        
        getNumbers()는 화면이랑 크게 관련이 없음 -> 분리할 수 있는 함수이기 때문에 methods로 엮지 않는다.
        const getNUmbers = () ~ 는 다른 컴포넌트에서도 사용 가능 !
        =>data() { ~ } 와 methods: { ~ } 안에는 현재 컴포넌트(화면)과 밀접한 연관이 있는 것만 넣어준다. 
    */
    const getNumbers = () => {
        const candidates = [1, 2, 3, 4, 5, 6, 7, 8, 9];
        const array = [];
        for (let i = 0; i < 4; i += 1) {
            const chosen = candidates.splice(Math.floor(Math.random() * (9 - i)), 1)[0];
            array.push(chosen);
        }

        console.log('### 정답 >>> ' + array);
        return array;
    };

    /*
        여기서 export default한것을 main.js에서 import로 가져올 수 있다.
        cf)node에서는 module.exports와 require사용 (webpack.config.js 참조)
    */
    export default { 
        data() {
            return {
                answer: getNumbers(), // ex) [1,5,3,4]
                tries: [], // 시도 수
                value: '', // 입력
                result: '',  // 결과
            }
        },
        methods: {
            onSubmitForm() {
                if (this.value === this.answer.join('')) { // 정답 맞췄으면
                    // value(숫자) , answer(배열) -> 다른 형식을 비교하려면 join 사용
                    this.tries.push({
                        try: this.value,
                        result: '홈런'
                    });
                    // 데이터 초기화 
                    this.result = '홈런';
                    alert('홈런 >>> 게임을 다시 시작합니다.');
                    this.value = '';
                    this.tries = [];
                    this.$refs.answer.focus();
                }else { // 정답이 틀렸으면 
                    if(this.tries.length >= 9) { // 10번째 시도
                        this.result = `10번 넘게 틀려서 실패! 답은 ${this.answer.join(',')}였습니다!`;
                        alert('실패 >>> 게임을 다시 시작합니다.');
                        // 데이터 초기화 
                        this.value = '';
                        this.answer = getNumbers();
                        this.tries = [];
                        this.$refs.answer.focus();
                    }

                    let strike = 0;
                    let ball = 0;
                    /*
                        데이터와 변수의 차이
                        -데이터 : 화면에 보여지는 것
                        -변수 : 화면과 관련없는 것
                    */
                    const answerArray = this.value.split('').map(v => parseInt(v)); // 숫자를 숫자 배열로 바꿈 (1234 -> [1,2,3,4])
                    for(let i=0; i<4; i += 1) {// 반복문으로 한숫자씩 비교
                        if(answerArray[i] === this.answer[i]) { // 숫자 자릿수 모두 정답
                            strike++;
                        }else if(this.answer.includes(answerArray[i])) { // 숫자만 정답
                            ball++;
                        }
                    }

                    this.tries.push({
                        try: this.value,
                        result: `${strike} 스트라이크, ${ball} 볼입니다.`,
                    });
                    this.value = '';
                    this.$refs.answer.focus();
                }
            }
        }
    };
</script>

<style>

</style>