<template>
  <div>
    <VueDraggable v-model="list" animation="150" ghostClass="ghost" class="container" group="shared">
      <div v-for="item in list" :key="item.id" class="item">
        {{ item.name }}
      </div>
    </VueDraggable>
    <br /><br />
    <VueDraggable v-model="list2" animation="150" ghostClass="ghost" class="container" group="shared">
      <div v-for="item in list2" :key="item.id" class="item">
        {{ item.name }}
      </div>
    </VueDraggable>
  </div>
</template>

<script>
import { defineComponent, ref } from 'vue';
import { VueDraggable } from 'vue-draggable-plus';
export default defineComponent({
  name: 'App',
  components: {
    VueDraggable,
  },
  setup() {
    const list = ref([
      {
        name: 'Joao',
        id: 1,
      },
      {
        name: 'Jean',
        id: 2,
      },
      {
        name: 'Johanna',
        id: 3,
      },
      {
        name: 'Juan',
        id: 4,
      },
    ]);

    const list2 = ref([]);

    return { list, list2 };
  },
});
</script>

<style>
.container {
  display: grid;
  gap: 10px;
}

.item {
  width: 200px;
  background-color: red;
  height: 30px;
  line-height: 30px;
  text-align: center;
  cursor: move;
}
</style>
