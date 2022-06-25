<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">

      <q-input
        v-model="newTask"
        class="col"
        square
        filled
        bg-color = "white"
        bottom-slots
        placeholder="Adicionar tarefa"
        dense>
  
        <template v-slot:append>
          <q-btn
          @click="addTask"
          @keyup.enter="addTask"
          round
          dense
          flat
          icon="add" />
        </template>

      </q-input>

    </div>
    <q-list
    separator
    bordered
    class="bg-white">
    
      <q-item
        v-for="task, index in tasks"
        :key="task.title"
        @click="task.done= !task.done"
        :class="{ 'done bg-blue-1': task.done }"
        clickable
        v-ripple>
        <q-item-section avatar>
          <q-checkbox
          v-model="task.done"
          class="no-pointer-events"
          color="primary" />
        </q-item-section>
        <q-item-section>
          <q-item-label> {{task.title}} </q-item-label>
        </q-item-section>

        <q-item-section
          v-if="task.done"
          side>
          <q-btn
          @click.stop = "deleteTask(index)"
          flat
          dense
          round
          color="primary"
          icon="delete"/>
        </q-item-section>
      </q-item>
    </q-list>

    <div
      v-if="!tasks.length"
      class="no-tasks absolute-center">
      <q-icon
        name="check"
        size="100px"
        color="primary"
      />
      <div class="text-h5 text-primary text-center">
          Não há tarefas!
      </div>
    </div>
  </q-page>
</template>

<script>
  export default {

    data() {
      return {

        newTask:'',
        tasks: [
          /*{
            title: 'Get bananas',
            done: true
          },
          {
            title: 'Eat bananas',
            done: false
          },
          {
            title: 'Poo bananas',
            done: false
          }*/
        ] 
      }
    },

    methods: {
        deleteTask(index) {

          this.$q.dialog({
          title: 'Mensagem de Confirmação.',
          message: 'Tem Certeza que Deseja Remove?',
          cancel: true,
          persistent: true
        }).onOk(() => {
          this.tasks.splice(index, 1)
          this.$q.notify('Removido com sucesso!')
        })
      },

      addTask() {
        this.tasks.push({
          title: this.newTask,
          done: false
        })

        this.newTask= ''
      }
    }
  }
</script>

<style lang="scss">
  .done {
    .q-item__label {
      text-decoration: line-through;
      color: #bbb;
    }
  }

  .no-tasks {
    opacity: .5;
  }
</style>
