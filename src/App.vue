<script setup lang="ts">
import { ref } from 'vue'
import { ElMessage, ElDialog, ElButton  } from 'element-plus'


const map = ref([
  ['1','1','1'],
  ['1','1','1','1','1'],
  ['1','1','1','1','1','1','1']
])
const play = ref('A')
const dialogVisible = ref(false)

const game = {
  nowY : -1,
  isNext: false,
  start (){
    map.value = [
      ['1','1','1'],
      ['1','1','1','1','1'],
      ['1','1','1','1','1','1','1']
    ]
    game.isNext = false
    play.value = 'A'
    game.nowY = -1
    dialogVisible.value = false, 
    ElMessage({
      message: 'Restart is Ok',
      type: 'success',
    })
  },
  nextRound (x:number, y: number, i: String) {
    if(game.isNext) {
      game.isNext = false
      play.value = play.value === 'A' ? 'B' : 'A'
      game.nowY = -1
    } else {
      ElMessage({
        message: 'Please play games!!!',
        type: 'warning',
      })
    }

  },
  clickItem (x:number, y: number, i: String) {
    if(i !== '1') return false
    if(game.nowY === y || game.nowY === -1) {
      game.nowY = y
      map.value[y][x] = play.value
      if(!game.isEnd()) {
        return false
      }
      game.isNext = true
    } else {
      ElMessage({
        message: 'Not clickable',
        type: 'warning',
      })
    }
  },
  isEnd () {
    const arr = map.value.flat(2)
    if(!arr.some(e=> e === '1')) {
      dialogVisible.value = true
      return false
    } 
    return true
  }
}

</script>

<template>
  <header class="header">
    Im {{play}}
  </header>
  <div class="box-list" v-for="(list, y) in map">
    <div class="box-item" :class="[ `box-item-${item}`]" v-for="(item, x) in list" @click="game.clickItem(x, y, item)"></div>
  </div>
  <footer class="footer">
    <ElButton @click="game.nextRound" type="success" size="large">Next Round</ElButton>
    <ElButton @click="game.start" type="primary" size="large">Restart</ElButton>
  </footer>
  <ElDialog v-model="dialogVisible"
    title="Game Over"
    width="30%"
    :modal="true"
    >
    <span>Play {{play}} you failed!!!!</span>
    <template #footer>
      <span class="dialog-footer">
        <ElButton @click="dialogVisible = false">Cancel</ElButton>
        <ElButton type="primary" @click="game.start"
          >Restart</ElButton>
      </span>
    </template>
  </ElDialog>
</template>

<style lang="scss">
*{
  margin:  0;
  padding: 0;
}
.box-list{
  display: flex;
  position: relative;
  .box-item{
    width: 60px;
    height: 60px;
    padding: 10px;
    border: 1px solid #666;
    margin: 5px;
    cursor: pointer;
    border-radius: 4px;
    text-align: center;
    // box-sizing: border-box;
    line-height: 60px;
    background-color: gray;
  } 
  .box-item-A {
    background-color: red;
  }
  .box-item-B {
    background-color: green;
  }
}
.header{
  font-size: 30px;
  text-align: left;
  margin-bottom: 20px;
}
.footer{
  margin-top: 20px;
}
</style>
