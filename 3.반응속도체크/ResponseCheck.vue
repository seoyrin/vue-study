<template>
    <div>
        <div id="screen" :class="state" @click="onClickScreen">{{message}}</div>
        <!--v-bind:class 와  :class 는 같은 표현-->
        <div v-if="result.length">
        <!--v-show와 v-if의 차이? : -->
            <div>평균 시간: {{average}}ms</div>
            <!--result.reduce((a, c) => a + c, 0) :: 배열의 값을 다 더하기--><!--|| 0 :: 기본값을 0으로 지정-->
            <!--계산식은 여기부분보다는 computed 속성 안에서 해줘야 함(*데이터의 가공이 필요한 경우엔 computed 적용*)-->
            <button @click="onReset">리셋</button>
            <!--v-on:click 와 @click 는 같은 표현-->
        </div>
    </div>
</template>

<script>
    let startTime = 0;
    let endTime = 0;
    let timeout = null; 
    // startTime, endTime, timeout 을 data() 안에 안넣는 이유? -> 화면이랑 관련이 없기 때문(즉, 화면이랑 관련이 있는 것만 data()안에 넣는다.)
    export default { 
        /*
            data와 computed의 차이점
            ->data : 간단한 데이터
            ->computed : 일반데이터를 가공해서 쓸 때 사용
        */
        data() {
            return {
                result: [],
                state: 'waiting',
                message: '클릭해서 시작하세요.',
            };
        },
        computed: {
            average() {
                return this.result.reduce((a, c) => a + c, 0) / this.result.length || 0;
            }
        },
        methods: {
            onReset() {
                this.result =  [];
            },
            onClickScreen() {
                if(this.state === 'waiting') {
                    this.state = 'ready';
                    this.message = '초록색이 되면 클릭하세요.';
                    timeout = setTimeout(() => {
                        this.state = 'now';
                        this.message = '지금 클릭!';
                        startTime = new Date();
                    }, Math.floor(Math.random() * 1000) + 2000); // 2~3초
                }else if(this.state === 'ready') {
                    clearTimeout(timeout); // 기존Timeout을 없애줌
                    this.state = 'waiting';
                    this.message = '너무 성급하신군요! 초록색이 된 후에 클릭하세요.';
                }else if(this.state === 'now') {
                    endTime = new Date();
                    this.state = 'waiting';
                    this.message = '클릭해서 시작하세요.';
                    this.result.push(endTime - startTime);
                }
            }
        },
    };
</script>

<style scoped>
/*<style scoped> :: 해당 컴포넌트 안에서만 사용할 style속성 */
    #screen {
        width: 300px;
        height: 200px;
        text-align: center;
        user-select: none;
    }
    #screen.waiting {
        background-color: aqua;
    }
    #screen.ready {
        background-color: red;
        color: white;
    }
    #screen.now {
        background-color: greenyellow;
    }
</style>