<template>
    <div class="ball" :style="styleObject">{{ number }}</div>
</template>

<script>
    export default {
        name: 'LottoBall', // ->name을 여기에 정의하지 않아도, 자동으로 해당파일 이름이 component의 name이 된다.

        // props : 부모컴포넌트에서 넘겨준 데이터를 받아오기 위한 방법
        // props로 가져온 데이터는 수정될 수 없다★ -> 해결방법: 데이터를 바꾸는 부모의 메서드(함수)를 자식에게 porps로 전달★
        props: {
            number: Number, // prop은 가능한 상세하게 정의되어야 하기 때문에, '데이터명: 데이터타입' 형식으로 정의해줘야 한다.
        }, // -> props: ['number'], 와 동일한 표현
        computed: {
            styleObject() {
                let background;

                if(this.number <= 10) {
                    background = 'red';
                }else if(this.number <= 20) {
                    background = 'orange';
                }else if(this.number <= 30) {
                    background = 'yellow';
                }else if(this.number <= 40) {
                    background = 'blue';
                }else {
                    background = 'green';
                }

                return {
                    background,
                };
            }
        },
    }
</script>

<style>
    .ball {
        display: inline-block;
        border: 1px solid black;
        border-radius: 20px;
        width: 40px;
        height: 40px;
        line-height: 40px;
        font-size: 20px;
        text-align: center;
        margin-right: 20px;
    }
</style>