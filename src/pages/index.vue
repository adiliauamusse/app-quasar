<template>
  <QPage class="bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">
      <QInput
        @keyup.enter="createTask"
        v-model="input"
        bg-color="white"
        class="col"
        dense
        filled
        placeholder="Add task"
        square
      >
        <template v-slot:append>
          <QBtn @click="createTask" round dense flat icon="add" />
        </template>
      </QInput>
    </div>

    <QList class="bg-white" separator bordered>
      <QItem
        v-for="(task, index) in tasks"
        :key="task.title"
        @click="task.done = !task.done"
        :class="{ 'done bg-blue-1': task.done }"
        clickable
        v-ripple
      >
        <QItemSection avatar>
          <QCheckbox
            v-model="task.done"
            class="no-pointer-events"
            color="primary"
          />
        </QItemSection>

        <QItemSection>
          <QItemLabel>{{ task.title }}</QItemLabel>
        </QItemSection>

        <QItemSection v-if="task.done" side>
          <QBtn
            @click.stop="removeTask(index)"
            color="primary"
            dense
            flat
            icon="delete"
            round
          />
        </QItemSection>
      </QItem>
    </QList>

    <div v-if="!tasks.length" class="no-tasks absolute-center">
      <QIcon name="check" size="100px" color="primary" />
      <div class="text-h5 text-primary text-center">Nothing to display!</div>
    </div>
  </QPage>
</template>

<script setup>
import {
  QBtn,
  QCheckbox,
  QIcon,
  QInput,
  QItem,
  QItemLabel,
  QItemSection,
  QList,
  QPage,
  useQuasar,
} from "quasar";
import { reactive, ref } from "vue";
const $q = useQuasar();

const input = ref("");
const tasks = reactive([]);

const removeTask = (index) => {
  const target = tasks.at(index);

  $q.dialog({
    title: "Confirm",
    message: "Are you sure?",
    cancel: true,
    persistent: true,
  }).onOk(() => {
    tasks.splice(index, 1);
    $q.notify(target.title + ". Deleted");
  });
};

const createTask = () => {
  tasks.push({ title: input.value, done: false });
  input.value = "";
};
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
