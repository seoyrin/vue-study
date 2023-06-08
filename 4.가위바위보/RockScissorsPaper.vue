<template>
    <div>
        <div id="computer" :style="computedStyleObject"></div>
        <div>
            <button @click="onClickButton('바위')">바위</button>
            <button @click="onClickButton('가위')">가위</button>
            <button @click="onClickButton('보')">보</button>
        </div>
        <div>{{result}}</div>
        <div>현재 {{score}}점</div>
    </div>
</template>

<script>
    const rspCoords = { // 의미가 불분명한 변수들은 이름을 붙여주는게 좋다.
        바위: '0',
        가위: '-142px',
        보: '-284px'
    };

    const scores = {
        가위: 1,
        바위: 0,
        보: -1,
    };
    const computerChoice = (imgCoord) => { // 컴퓨터의 선택
        return Object.entries(rspCoords).find(function (v) {
            return v[1] === imgCoord;
        })[0];
    };

    let interval = null;
    export default { 
        data() {
            return {
                imgCoord: rspCoords.바위,
                result: '',
                score: 0,
            };
        },
        computed: { // 단순히 데이터만 사용하는게 아니면, computed를 사용
            computedStyleObject() {
                return {
                    background: `url(https://en.pimg.jp/023/182/267/1/23182267.jpg) ${this.imgCoord} 0` ,
                };
            }
        },
        methods: {
            changeHand() {
                interval = setInterval(() => {
                    if(this.imgCoord === rspCoords.바위) {
                        this.imgCoord = rspCoords.가위;
                    }else if(this.imgCoord === rspCoords.가위) {
                        this.imgCoord = rspCoords.보;
                    }else if(this.imgCoord === rspCoords.보) {
                        this.imgCoord = rspCoords.바위;
                    }
                }, 100); // 0.1초마다 '바위->가위->보'로 이미지 변경
            },

            onClickButton(choice) {
                clearInterval(interval); // 이미지를 멈춤으로써 시각적으로 확인
                const myScore = scores[choice];
                const cpuScore = scores[computerChoice(this.imgCoord)];
                const diff = myScore - cpuScore;
                if(diff === 0) {
                    this.result = '비겼습니다.';
                }else if([-1, 2].includes(diff)) {
                    this.result = '이겼습니다.';
                    this.score +=1;
                }else {
                    this.result = '졌습니다.';
                    this.score -=1;
                }
                setTimeout(() => {
                    this.changeHand();
                }, 1000);
            },
        },
        
        // vue의 라이프사이클★
        beforeCreate() {
            console.log('beforeCreate');
        },
        created() { // 컴포넌트가 보여지긴 하지만 화면엔 나타나기 전에 실행 / 데이터들이 모두 준비되면 실행(js안에서만 존재)
            console.log('created');
        },
        beforeMount() {
            console.log('beforeMouned');
        },
        mounted() { // 회면에 나타난 후에 실행 / 데이터들이 모두 준비되고, 화면에 보여지면 실행
            //화면에 관련된 프로세스를 작성하는 곳
            //(화면에 표시된 후에 접근하는게 좋기 때문,created에서 해도 되지만 안되는 경우도 있어서 안전하게 mounted에서 진행하는게 좋다.)
            console.log('mounted');
            this.changeHand();
        },
        beforeUpdate() {
            console.log('beforeUpdate');
        },  
        updated() { // 화면의 데이터가 바뀌어서, 화면이 다시 그려질 때 실행
            console.log('updated');
        },
        beforeDestroy() {
            console.log('beforeDestroy');
            clearInterval(interval);
            /*
                setInterval , setTimeout 을 했으면, beforeDestroy에서 
                clearInterval , clearTimeout 을 해줘야 한다.
                (이유? 메모리 누수를 막기 위해서)
            */
        },
        destroyed() { // 컴포넌트가 화면에 있었다가 사라질 때 실행(ex:<lifecycle-example v-if-:"fase">가 되어서 사라질 때..)
            console.log('destroyed');
        },
    };

    /*
        모든함수들 안에는 async를 붙일 수 있다.
        예)async onClickButton(choice) {
            await new Promise() ~ 
        }
    */
</script>

<style scoped>
    #computer {
        width: 142px;
        height: 200px;
        background-position: 0 0;
    }
</style>