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
                if(this.cellData) return; // 이미 눌러진 칸을 다시 눌렀을 때 처리

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

                // 빙고가 됐는지 확인
                let win = false;
                if (rootData.tableData[this.rowIndex][0] === rootData.turn && rootData.tableData[this.rowIndex][1] === rootData.turn && rootData.tableData[this.rowIndex][2] === rootData.turn) {
                win = true;
                }
                if (rootData.tableData[0][this.cellIndex] === rootData.turn && rootData.tableData[1][this.cellIndex] === rootData.turn && rootData.tableData[2][this.cellIndex] === rootData.turn) {
                win = true;
                }
                if (rootData.tableData[0][0] === rootData.turn && rootData.tableData[1][1] === rootData.turn && rootData.tableData[2][2] === rootData.turn) {
                win = true;
                }
                if (rootData.tableData[0][2] === rootData.turn && rootData.tableData[1][1] === rootData.turn && rootData.tableData[2][0] === rootData.turn) {
                win = true;
                }

                // 승자 확인 
                if(win) { // 이긴 경우: 3줄 달성
                    rootData.winner = rootData.turn; // 승자 기록
                    // 데이터 초기화
                    rootData.turn = 'O';
                    rootData.tableData = [['','',''], ['','',''], ['','','']];
                }else { // 무승부
                    let all = true; // all이 true면 무승부라는 뜻
                    rootData.tableData.forEach((row) => { // 무승부 검사
                        console.log('row >>> ', row);
                        //console.log('index >>> ', index);
                        //console.log('array >>> ', array);
                        row.forEach((cell) => {
                        if (!cell) {
                            all = false;
                        }
                        });
                    });
                    if(all) { // 무승부
                        rootData.turn = 'O';
                        rootData.winner = '';
                        rootData.tableData = [['','',''], ['','',''], ['','','']];
                    }else { // 게임이 안끝난 경우
                        rootData.turn = rootData.turn === 'O' ? 'X' : 'O'; // 최상위 컴포넌트 data 바꿔주기 가능
                    }
                }

                //console.log('### root데이터 >>> ', this.$root.$data); // 최상위 컴포넌트의 data
                //console.log('### parent데이터 >>> ', this.$parent.$data); // 현재 컴포넌트 기준, 부모 컴포넌트의 data

            }
        }
    };
</script>