<script setup lang="ts">
interface NameProps {
  name: string;
  index: number;
}
const props = defineProps<NameProps>();
const emit = defineEmits(["remove"]);
const removeName = () => {
  emit("remove", props.index);
};
</script>

<template>
  <div class="card">
    <h4>{{ name }}</h4>
    <p @click="removeName">x</p>
  </div>
</template>

<style lang="scss" scoped>
.card {
  @apply relative;
  @apply bg-slate-400 text-white w-full py-4 text-center font-bold;
}
.card p {
  @apply absolute top-1 right-5 text-red-700 font-bold cursor-pointer;
}
</style>
