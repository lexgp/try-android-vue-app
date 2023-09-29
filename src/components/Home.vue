<template>
  <div>
  <el-page-header :icon="null" :title="null">
    <template #content>
    </template>
  </el-page-header>

    <ion-content :fullscreen="true">

      <div id="container">

        <div style="margin-top: 20px; margin-bottom: 10px; padding-left: 5px; padding-right: 5px;">
          <el-input
            v-model="expression"
            class="large-calc-input"
            size="large"
            readonly
            :class="{'input-danger' : expression == '#ERROR'}"
          />
        </div>

        <div style="display: flex; flex-wrap: wrap;">
          <el-button type="primary" plain v-for="index in 9"
            :key="index"
            style="min-width: 30%; height: 60px; font-size: 200%; flex: 1; margin-right: 5px; margin-left: 5px; margin-top: 5px;"
            @click="buttonsHandler(index)"
          >
            {{ index }}
          </el-button>
          <el-button type="primary" plain
            style="min-width: 30%; height: 60px; font-size: 200%; flex: 1; margin-right: 5px; margin-left: 5px; margin-top: 5px;"
            @click="buttonsHandler(0)"
          >
            0
          </el-button>
          <el-button type="success" plain
            v-for="operation in ['+', '-', '*', '/', '=', ]"
            :key="operation"
            style="min-width: 30%; height: 60px; font-size: 200%; flex: 1; margin-right: 5px; margin-left: 5px; margin-top: 5px;"
            @click="buttonsHandler(operation)"
          >
            {{ operation }}
          </el-button>
        </div>
      </div>
    </ion-content>
  </div>
</template>

<script lang="ts">
  import { defineComponent } from "vue"

  export default defineComponent({
    data() {
      return {
        expression: ''
      }
    },
    methods: {
      buttonsHandler(value: string | number) {

        value = value.toString()
        
        // Сообщение об ошибке заменяем на пустое выражение. Малозначимая фигня, хотя да, стоило иначе сделать.
        let expression = ['#ERROR'].includes(this.expression) ? '' : this.expression

        // Перечень возможных элементов выражение.
        // Когда нажмут '=' - то выражение полетит вычисляться.
        const digits = ['0', '1' ,'2' ,'3' ,'4' ,'5' ,'6' ,'7' ,'8' ,'9' ,]
        const operations = ['-', '+', '/', '*']
        
        // Добавляем элементы к выражению
        if (digits.includes(value) || operations.includes(value)) {
          expression += value
        }

        // Если это цифра, то озвучиваем
        if (digits.includes(value)) {
          new Audio('audio/digits/' + value + '.mp3').play()
        }

        // Считаем
        if (value == '=') {
          let result = ''
          try {
            // Вычисление происходит тут
            result = eval(expression)
          } catch {}
          
          if (result) {
            const trackIndex = Math.floor(Math.random() * 5)
            new Audio('audio/result/' + trackIndex + '.mp3').play()
            expression = result
          } else {
            expression = '#ERROR'
            new Audio('audio/result/error.mp3').play()
          }
        }

        this.expression = expression
      }
    }
  })

</script>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>

<style>
  .el-page-header__back, .el-divider.el-divider--vertical {
    display: none !important;
  }
  .el-input.large-calc-input {
    font-size: 20px;
    height: 50px;
  }
  .el-input.large-calc-input input.el-input__inner {
    text-align: right;
    font-weight: bold;
    /* font-size: 150%; */
  }
  .el-input.large-calc-input.input-danger {
    border-color: var(--el-color-error-dark-2) !important;
  }
</style>
