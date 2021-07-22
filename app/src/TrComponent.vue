<template>
  <tr>
    <td-component v-for="(cellData, index) in rowData" :key="index" :cell-index="index" :row-index="rowIndex"></td-component> <!-- State 에 저장된 rowData 를 가져와서 사용 -->
  </tr>
</template>

<script>
  import TdComponent from './TdComponent';
  export default {
    components: {
      TdComponent,
    },
    data() {
      return {
        parent: '내가 니 애비다',
      };
    },
    computed: {
      rowData() {
        return this.$store.state.tableData[this.rowIndex];
      }
    },
    props: {
      rowIndex: Number,
    },
  };
</script>
