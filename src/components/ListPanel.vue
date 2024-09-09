<template>
  <div class="panel">
    <div class="panel__wrapper">
      <div class="panel__actions">
        <span class="panel__title">Фильтрация по рейтингу</span>
        <div>
          <v-menu>
            <template v-slot:activator="{ props }">
              <v-btn color="primary" v-bind="props">
                {{ selectedFilter }}
              </v-btn>
            </template>
            <v-list>
              <v-list-item>
                <v-btn class="panel__btn" @click="onClickSortDescend">
                  <v-list-item-title>возрастание</v-list-item-title>
                </v-btn>
              </v-list-item>
              <v-list-item>
                <v-btn class="panel__btn" @click="onClickSortIncrease">
                  <v-list-item-title>убывание</v-list-item-title>
                </v-btn>
              </v-list-item>
            </v-list>
          </v-menu>
        </div>
      </div>
      <div class="">
        <v-btn variant="tonal" @click="resetFilter">Сбросить</v-btn>
      </div>
    </div>
  </div>
</template>

<script setup>
import { inject, ref } from "vue";
const sortedList = ref([]);
const selectedFilter = ref("По умолчанию");

const firstList = inject("firstList");
const secondList = inject("secondList");
const lastList = inject("lastList");

const props = defineProps({
  id: Number,
});
const emit = defineEmits(["sortList", "resetFilter"]);

const onClickSortIncrease = () => {
  switch (props.id) {
    case 1:
      sortedList.value = firstList.value.toSorted(
        (a, b) => a.rating.rate - b.rating.rate
      );
      break;
    case 2:
      sortedList.value = secondList.value.toSorted(
        (a, b) => a.rating.rate - b.rating.rate
      );
      break;
    case 3:
      sortedList.value = lastList.value.toSorted(
        (a, b) => a.rating.rate - b.rating.rate
      );
      break;
  }
  selectedFilter.value = "Возрастание";
  emit("sortList", { item: sortedList.value, id: props.id });
};

const onClickSortDescend = () => {
  switch (props.id) {
    case 1:
      sortedList.value = firstList.value.toSorted(
        (a, b) => b.rating.rate - a.rating.rate
      );
      break;
    case 2:
      sortedList.value = secondList.value.toSorted(
        (a, b) => b.rating.rate - a.rating.rate
      );
      break;
    case 3:
      sortedList.value = lastList.value.toSorted(
        (a, b) => b.rating.rate - a.rating.rate
      );
      break;
  }
  selectedFilter.value = "Убывание";
  emit("sortList", { item: sortedList.value, id: props.id });
};

const resetFilter = () => {
  switch (props.id) {
    case 1:
      sortedList.value = firstList.value.toSorted((a, b) => a.id - b.id);
      break;
    case 2:
      sortedList.value = secondList.value.toSorted((a, b) => a.id - b.id);
      break;
    case 3:
      sortedList.value = lastList.value.toSorted((a, b) => a.id - b.id);
      break;
  }
  selectedFilter.value = "По умолчанию";
  emit("resetFilter", { item: sortedList.value, id: props.id });
};
</script>

<style lang="scss" scoped>
.panel {
  margin-bottom: 8px;
  padding: 6px 8px;
  background-color: white;
  border-radius: 8px;
  &__wrapper {
    display: flex;
    justify-content: space-between;
    align-items: end;
    width: 100%;
    gap: 8px;
  }
  &__title {
    font-size: 14px;
    color: gray;
  }
  &__actions {
    flex: 1;
  }
  &__btn {
    width: 100%;
    text-align: start;
  }
}
</style>
