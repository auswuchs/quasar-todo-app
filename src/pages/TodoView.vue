<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">
      <q-input
        class="col"
        square
        filled
        placeholder="Добавить заметку..."
        dense
        bg-color="white"
        autofocus
        v-model.trim="newTask"
        @keyup.enter="addTask"
      >
        <template v-slot:append>
          <q-btn round dense flat icon="add" @click="addTask" />
        </template>
      </q-input>
    </div>
    <q-list class="bg-white" separator bordered>
      <q-item
        tag="label"
        v-ripple
        v-for="(task, index) in tasks"
        :key="task"
        :class="{ 'done bg-grey-2': task.done }"
      >
        <q-item-section avatar>
          <q-checkbox v-model="task.done" color="primary" />
        </q-item-section>

        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>

        <q-item-section v-if="task.done" side>
          <q-btn flat round dense color="primary" icon="delete" @click.stop="deleteTask(index)" />
        </q-item-section>
      </q-item>
    </q-list>
    <div class="absolute-center no-tasks" v-if="!tasks.length">
      <q-icon name="check" size="100px"></q-icon>
      <div class="text-h6 text-primary text-center">Заметок нет</div>
    </div>
  </q-page>
</template>

<script setup>
import { ref } from 'vue'
import { useQuasar } from 'quasar'

const $q = useQuasar()

const newTask = ref('')

const tasks = ref([])

const deleteTask = (index) => {
  $q.dialog({
    title: 'Подтвердите',
    message: 'Вы уверены, что хотите удалить заметку?',
    cancel: 'Отмена',
    ok: 'Удалить',
    persistent: 'true'
  }).onOk(() => {
    tasks.value.splice(index, 1)

    $q.notify({
      message: 'Заметка была успешно удалена',
      color: 'primary',
      progress: true,
      timeout: 3000
    })
  })
}

const addTask = () => {
  if (newTask.value) {
    tasks.value.push({
      title: newTask.value,
      done: false
    })
    newTask.value = ''
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
  opacity: 0.5;
}
</style>
