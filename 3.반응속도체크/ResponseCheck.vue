<template>
    <div>
        <div id="screen" :class="state" @click="onClickScreen">{{message}}</div>
        <!--v-bind:class 와  :class 는 같은 표현-->
        <div>
            <div>평균 시간: {{result.reduce((a, c) => a + c, 0) / result.length || 0}}ms</div>
            <!--result.reduce((a, c) => a + c, 0) :: 배열의 값을 다 더하기--><!--|| 0 :: 기본값을 0으로 지정-->
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
        data() {
            return {
                result: [],
                state: 'waiting',
                message: '클릭해서 시작하세요.',
            };
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