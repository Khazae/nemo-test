<template>
  <div class="content">
    <span class="header"> {{ item.name }}</span>

    <div class="detail" v-for="(attr, i) in innerItem.attributes" :key="i">
      <div class="field">
        <span class="title">code:</span>
        <input type="text" :value="attr.code" />
      </div>

      <div class="field">
        <span class="title">name:</span>
        <input type="text" :value="attr.name" />
      </div>

      <div v-if="attr.color" class="field">
        <span class="title">color:</span>
        <input type="text" :value="attr.color" />
      </div>

      <div v-if="attr.size" class="field">
        <span class="title">size:</span>
        <span>
          <input :value="attr.size.width" type="number" /> x
          <input :value="attr.size.height" type="number" />
        </span>
      </div>

      <div v-if="attr.weight" class="field">
        <span class="title">weight:</span>
        <input :value="attr.weight" type="number" />
      </div>
    </div>

    <div class="add">
      <label>
        type
        <select ref="selectRef">
          <option value="color">color</option>
          <option value="size">size</option>
          <option value="weight">weight</option>
        </select>
      </label>

      <button @click="emit('click', selectRef.value)">Add</button>
    </div>
  </div>
</template>

<script setup lang="ts">
import { defineProps, defineEmits, ref, watch } from "vue";

const props = defineProps<{
  item: {
    id: number;
    name: string;
    attributes: {
      code: string;
      name: string;
    };
  };
}>();

const emit = defineEmits(["click"]);

const selectRef = ref<HTMLSelectElement>();

const innerItem = ref(props.item);

// Вместо обьявления innerItem можно было бы просто передать сам props.item в template
watch(
  () => props.item,
  () => {
    innerItem.value = props.item;
  },
  { deep: true }
);
</script>

<style scoped lang="css">
.header {
  font-weight: bold;
}
.content {
  display: flex;
  flex-direction: column;
  padding: 20px;
  gap: 20px;
}
.detail {
  padding: 10px;
  display: flex;
  flex-direction: column;
  gap: 10px;
}
.field {
  display: flex;
  gap: 5px;
}
.title {
  font-weight: bold;
}
.add {
  display: flex;
  gap: 10px;
}
</style>
