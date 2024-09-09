<template>
  <main>
    <div v-for="(item, index) in CardListOptions" :key="index">
      <ListPanel
        :id="item.id"
        @sort-list="onSort"
        @reset-filter="resetFilter"
      />
      <CardList :options="item" />
    </div>
  </main>
</template>

<script setup>
import axios from "axios";
import { computed, provide, ref } from "vue";
import CardList from "./components/CardList.vue";
import ListPanel from "./components/ListPanel.vue";

const firstList = ref([]);
const secondList = ref([]);
const lastList = ref([]);


const resetFilter = ({item, id}) => {
  switch (id) {
    case 1:
      firstList.value = item;
      break;
    case 2:
      secondList.value = item;
      break;
    case 3:
      lastList.value = item;
      break;
  }
};
const onSort = ({ item, id }) => {
  switch (id) {
    case 1:
      firstList.value = item;
      break;
    case 2:
      secondList.value = item;
      break;
    case 3:
      lastList.value = item;
      break;
  }
};

const CardListOptions = ref([
  {
    color: "#0aa6e362",
    title: "Необработанные",
    id: 1,
  },
  {
    color: "#e86405bd",
    title: "В работе",
    id: 2,
  },
  {
    color: "#9400d364",
    title: "Завершенные",
    id: 3,
  },
]);



const getAllCards = async () => {
  try {
    await axios
      .get("https://fakestoreapi.com/products")
      .then((res) => (firstList.value = res.data));
    console.log("getAllCards - успешно");
  } catch (error) {
    console.log(error);
    alert("Упс! Ошибка получения данных ;(");
  }
};

getAllCards();

provide("firstList", firstList);
provide("secondList", secondList);
provide("lastList", lastList);
</script>

<style scoped>
main {
  margin: 5% 0;
  display: flex;
  justify-content: center;
  gap: 5%;
  width: 100%;
}
.panel {
  display: flex;
}
</style>
