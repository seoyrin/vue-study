<template>
    <div>
        <div> 당첨 숫자</div>
        <div id="결과창">
            <lotto-ball v-for="ball in winBalls" :key="ball" :number="ball"></lotto-ball>
                                                        <!-- :number와 v-bind:number와 같은 표현 -->
        </div>
        <div>보너스</div>
        <lotto-ball v-if="bonus" :number="bonus" ></lotto-ball>
        <button v-if="redo" @click="onClickRedo">한 번 더!</button>
    </div>
</template>

<script>
    import LottoBall from  './LottoBall'; // 자식컴포넌트

    // 로또 숫자 랜덤으로 뽑기
    function getWinNumbers() {
        const candidate = Array(45).fill().map((v, i) => i + 1);
        const shuffle = [];
        while (candidate.length > 0) {
        shuffle.push(candidate.splice(Math.floor(Math.random() * candidate.length), 1)[0]);
        }
        const bonusNumber = shuffle[shuffle.length - 1];
        const winNumbers = shuffle.slice(0, 6).sort((p, c) => p - c);
        return [...winNumbers, bonusNumber];
    }

    const timeouts = [];
    export default { 
        components: { // 자식컴포넌트 정의
            'lotto-ball': LottoBall
            /*
                components: { LottoBall, }, 과 같은 표현 
                ==> 이렇게 쓰면 자동으로 LottoBall(파스칼케이스)-> lotto-ball(케밥케이스)로 바꿔준다.
            */
        },
        data() {
            return {
                winNumbers: getWinNumbers(),
                winBalls: [],
                bonus: null,
                redo: false,
            };
        },
        computed: {

        },
        methods: {
            onClickRedo() {
                // 데이터 초기화
                this.winNumbers = getWinNumbers();
                this.winBalls = [];
                this.bonus = null;
                this.redo = false;
                this.showBalls();
            },

            showBalls() { // 중복으로 사용되는 동작은 함수로 만들기
                for(let i=0; i<this.winNumbers.length - 1; i++){
                    timeouts[i] = setTimeout(() => {
                        this.winBalls.push(this.winNumbers[i]);
                    }, (i+1) * 1000);
                }
                timeouts[6] = setTimeout(() => {
                    this.bonus = this.winNumbers[6];
                    this.redo = true;
                }, 7000);
            }
        },
        
        mounted() {
            this.showBalls();
        },
        
        beforeDestroy() {
            console.log('beforeDestroy');
            timeouts.forEach((t) => {
                clearTimeout(t);
            })
            /*
                setInterval , setTimeout 을 했으면, beforeDestroy에서 
                clearInterval , clearTimeout 을 해줘야 한다.
                (이유? 메모리 누수를 막기 위해서)
            */
        },
        
        watch: {
           
        },
    };
</script>

<style scoped>
    
</style>