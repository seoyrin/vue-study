<template>
    <td @click="onClickTd">{{ cellData }}</td>
</template>

<script>
    export default {
        props: {
            cellData: String,
            rowIndex: Number,
            cellIndex: Number,
        },
        methods: {
            onClickTd() {
                const rootData = this.$root.$data;
                console.log('### root데이터 >>> ',  rootData);
                /*
                    this.$root.$data.tableData[this.rowIndex][this.cellIndex] = this.$root.$data.turn;
                    ★배열의 index를 사용해서 data를 바꾸게 되면, data는 바뀌지만 화면에 적용이 되진 않는다.(ex: this.tableData[1][0]='a';
                    ★this.turn = 'X'; -> 변수는 가능
                    ★객체나 배열의 내부 data를 index를 사용해서 바꾸게 되면 화면엔 반영이 되지 않는다.
                    ★배열의 메소드를 사용하여 수정하게 되면 화면에 반영이 된다.(ex: this.tableData.push('a');) 
                    ★해결방법) Vue.set() 또는 this.$set() 사용
                        ex1)import Vue from 'vue';
                            Vue.set(this.tableData[1], 0, 'X'); -> [1][0]칸에 'X'데이터 넣기
                        ex2)this.$set(this.tableData[1], 0, 'X'); -> [1][0]칸에 'X'데이터 넣기
                */
                // this.$set 사용
                this.$set(rootData.tableData[this.rowIndex], this.cellIndex, rootData.turn);

                //console.log('### root데이터 >>> ', this.$root.$data); // 최상위 컴포넌트의 data
                //console.log('### parent데이터 >>> ', this.$parent.$data); // 현재 컴포넌트 기준, 부모 컴포넌트의 data
                rootData.turn = rootData.turn === 'O' ? 'X' : 'O'; // 최상위 컴포넌트 data 바꿔주기 가능

                
                
                
            }
        }
    };
</script>