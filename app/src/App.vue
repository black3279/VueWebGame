<template>  <!-- 최상위 컴포넌트-->
  <div>
    <div>{{turn}}님의 턴입니다.</div>
    <table-component>  <!-- TableComponent 로 맵핑 ( 케밥 표기법으로 사용)-->
      <tr v-for="(rowData, rowIndex) in tableData" :rowIndex="rowIndex"> <!-- tableData 에서 각 배열을 rowData 로 꺼내고 각 인덱스를 맵핑하여 key 로 보내줌 -> 각 배열은 tr 로 생성 -->
        <td @click="onClickTd(rowIndex, cellIndex)" v-for="(cellData, cellIndex) in rowData" :key="cellIndex">{{cellData}}</td> <!-- 꺼낸 각 배열의 개별 데이터를 cellData 로 꺼냄, 해당 td 클릭 시, row/cell Index 를 전달함 -->
      </tr>
    </table-component>
    <div v-if="winner">{{winner}}님의 승리!</div>
  </div>
</template>

<script>
  // [0, 1, 2, 3, 4 ,12, 7, 8, 9, 10, 13, 156]
  //  0  1  2  3  4  5    6  7  8  9  10, 11,  12
  import { mapState } from 'vuex'; // vuex 에서 mapState import
  import store, { CHANGE_TURN, CLICK_CELL, NO_WINNER, RESET_GAME, SET_WINNER } from './store'; // mutations import
  import TableComponent from './TableComponent';
  import TrComponent from './TrComponent';
  import EventBus from './EventBus';
  export default {
    store,
    components: {
      TableComponent, // 파스칼 표기법으로 import
      TrComponent
    },
    data() {
      return {
        data: 1,
      }
    },
    computed: {
      ...mapState(['winner', 'turn', 'tableData']), // winner turn tableData State computed 로 추가
      // winner() {
      //   return this.$store.state.winner;
      // },
      // turn() {
      //   return this.$store.state.turn;
      // },
    },
    /*created() {
      EventBus.$on('clickTd', this.onClickTd);
    },*/
    methods: {
      onClickTd(rowIndex, cellIndex) { // 개별 td 클릭 시 발생하는 이벤트
        if (this.cellData) return; // 만약 현재 cell 에 세팅된 데이터가 있다면 리턴함
        this.$store.commit(CLICK_CELL, { row: rowIndex, cell: cellIndex }); // 세팅된 데이터가 없다면 CLICK_CELL 뮤테이션을 호출한다
        let win = false;
        if (this.tableData[rowIndex][0] === this.turn && this.tableData[rowIndex][1] === this.turn && this.tableData[rowIndex][2] === this.turn) { // 승리조건 명시
          win = true;
        }
        if (this.tableData[0][cellIndex] === this.turn && this.tableData[1][cellIndex] === this.turn && this.tableData[2][cellIndex] === this.turn) {
          win = true;
        }
        if (this.tableData[0][0] === this.turn && this.tableData[1][1] === this.turn && this.tableData[2][2] === this.turn) {
          win = true;
        }
        if (this.tableData[0][2] === this.turn && this.tableData[1][1] === this.turn && this.tableData[2][0] === this.turn) {
          win = true;
        }
        if (win) { // 이긴 경우: 3줄 달성
          this.$store.commit(SET_WINNER, this.turn);
          this.$store.commit(RESET_GAME);
        } else { // 무승부
          let all = true; // all이 true면 무승부라는 뜻
          this.tableData.forEach((row) => { // 무승부 검사
            row.forEach((cell) => {
              if (!cell) {
                all = false;
              }
            });
          });
          if (all) { // 무승부
            this.$store.commit(NO_WINNER);
            this.$store.commit(RESET_GAME);
          } else {
            this.$store.commit(CHANGE_TURN);
          }
        }
      }
    }
  };
</script>

<style>
  table {
    border-collapse: collapse;
  }
  td {
    border: 1px solid black;
    width: 40px;
    height: 40px;
    text-align: center;
  }
</style>
