<script setup lang="ts">
import { Gender, Popularity, Length, names } from "@/data";
export interface OptionsState {
  gender: Gender;
  popularity: Popularity;
  length: Length;
}
const options = reactive<OptionsState>({
  gender: Gender.GIRL,
  length: Length.ALL,
  popularity: Popularity.UNIQUE,
});
const selectedNames = ref<string[]>([]);
const removeName = (index: number) => {
  const filteredNames = [...selectedNames.value]
  filteredNames.splice(index, 1);
  selectedNames.value = filteredNames;
}
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
const optionsArray = [
  {
    title: "1) Choose a gender",
    category: "gender",
    buttons: [Gender.GIRL, Gender.UNISEX, Gender.BOY]
  },
  {
    title: "2) Choose the name's popularity",
    category: "popularity",
    buttons: [Popularity.TRENDY, Popularity.UNIQUE]
  },
  {
    title: "3) Choose the name's length",
    category: "length",
    buttons: [Length.SHORT, Length.ALL, Length.LONG]
  },
]

</script>

<template>
  <div class="container">
    <h1>Baby Name Generator</h1>
    <p>Choose your options and click the "Find Names" button below</p>
    <div class="options-container">
      <Option v-for="option in optionsArray" :option="option" :options="options" :key="option.title"/>
      <button @click="computeSelectedNames" class="primary">Find Names</button>
    </div>
    <div class="card-container">
      <!-- <div class="card" v-for="name in selectedNames" :key="name">
        <h4>
          {{ name }}
        </h4>
        <p>X</p>
      </div> -->
      <CardName 
      v-for="(name, index) in selectedNames" 
      :key="name" 
      :name="name"
      :index="index"
      @remove="() => removeName(index)"
      />
    </div>
  </div>
</template>

<style scoped>
.container {
  font-family: Arial, Helvetica, sans-serif;
  color: rgb(27, 60, 138);
  max-width: 50rem;
  margin: 0 auto;
  text-align: center;
}
h1 {
  font-size: 3rem;
}
.options-container {
  background-color: rgb(255, 238, 236);
  border-radius: 2rem;
  padding: 1rem;
  width: 95%;
  margin: 0 auto;
  margin-top: 4rem;
  position: relative;
}
.primary {
  background-color: rgb(249, 87, 89);
  color: white;
  border-radius: 6.5rem;
  border: none;
  padding: 0.75rem 4rem;
  font-size: 1rem;
  margin-top: 1rem;
  cursor: pointer;
}
.card-container {
  display: flex;
  margin-top: 3rem;
  flex-wrap: wrap;
}

.card {
  background-color: rgb(27, 60, 138);
  width: 28%;
  color: white;
  border-radius: 1rem;
  padding: 0.1rem;
  margin-right: 0.5rem;
  margin-bottom: 1rem;
  position: relative;
}

.card p {
  position: absolute;
  top: -20%;
  left: 92.5%;
  cursor: pointer;
  color: rgba(255, 255, 255, 0.17);
}
</style>
