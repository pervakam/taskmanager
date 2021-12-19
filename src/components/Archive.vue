<template>
  <v-container class="cards__wrapper">
    <h2 class="text-uppercase mb-8">Архив</h2>
    <h3 v-if="archivedCards.length === 0">Пока нет завершённых задач</h3>


    <ul class="cards__list cards__list--archive">
      <li
          class="cards__item mr-8 mb-8"
          v-for="(card, i) in archivedCards"
          :key="i">
        <task-card
            :taskCard="card"
        >
        </task-card>
      </li>
    </ul>

  </v-container>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';
import TaskCard from '@/components/TaskCard.vue';

@Component({
  name: 'Archive',
  components: { TaskCard },
})

export default class Archive extends Vue {
  @Prop({ type: Array, required: true }) public archivedCards!: [];
}
</script>

<style lang="scss">
.cards {
  &__list--archive {
    .cards__to-archive-button,
    .cards__edit-button {
      display: none;
    }
  }
}

</style>
