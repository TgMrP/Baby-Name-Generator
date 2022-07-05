<template>
  <div class="container mx-auto py-8">
    <div class="header">
      <h1>Baby Name Generator</h1>
      <h4>Choose your options and click the "Find Names" button below</h4>
    </div>
    <form @submit.prevent="computeSelectedNames">
      <div class="row" v-for="option in optionsArray" :key="option.title">
        <label>{{ option.title }}</label>
        <div>
          <button
            type="button"
            v-for="g in option.buttons"
            :key="g"
            @click="options[option.category] = g"
            :class="{ selected: options[option.category] === g }"
          >
            {{ g }}
          </button>
        </div>
      </div>

      <div class="row">
        <div>
          <button class="primary" type="submit">Find Names</button>
        </div>
      </div>
    </form>

    <div class="grid grid-cols-6 gap-x-8 gap-y-4 mt-4">
      <CardName
        v-for="(name, index) in selectedNames"
        :key="name"
        :name="name"
        @remove="() => removeName(index)"
        :index="index"
      />
    </div>
  </div>
</template>

<script setup lang="ts">
import { names, Gender, Popularity, Length } from "@/data/names";

interface OptionsState {
  gender: Gender;
  popularity: string;
  length: Length;
}

const options = reactive<OptionsState>({
  gender: Gender.BOY,
  popularity: Popularity.TRENDY,
  length: Length.ALL,
});

const optionsArray = [
  {
    title: "Choose a gender",
    category: "gender",
    buttons: Gender,
  },
  {
    title: "Choose the name's popularity",
    category: "popularity",
    buttons: Popularity,
  },
  {
    title: "Choose name's length",
    category: "length",
    buttons: Length,
  },
];
const selectedNames = ref<string[]>([]);

const computeSelectedNames = () => {
  const filteredNames = names
    .filter((name) => name.gender === options.gender)
    .filter((name) => name.popularity === options.popularity)
    .filter((name) => {
      if (options.length === Length.ALL) return true;
      else return name.length === options.length;
    });
  selectedNames.value = filteredNames.map((name) => name.name);
};

const removeName = (index: number) => {
  const filteredNames = [...selectedNames.value];
  filteredNames.splice(index, 1);
  selectedNames.value = filteredNames;
};
</script>

<style lang="scss" scoped>
.header {
  @apply text-center text-neutral-900;
  h1 {
    @apply text-6xl font-bold;
  }
  h4 {
    @apply text-2xl font-bold pt-4;
  }
}

form {
  @apply my-12;
  @apply bg-red-200 rounded-3xl px-10 text-center;

  .row {
    @apply flex flex-col py-8;

    label {
      @apply font-bold pb-2;
    }

    button:not(.primary) {
      @apply w-36 py-4 bg-white border border-red-600;
      @apply border-r-0;

      &:first-child {
        @apply rounded-l-xl;
      }

      &:last-child {
        @apply rounded-r-xl border-r;
      }

      &.selected {
        @apply bg-red-400;
      }
    }

    button.primary {
      @apply bg-red-600 text-white rounded-3xl border-0 px-16 py-3 text-3xl mt-8;
    }
  }
}
</style>
