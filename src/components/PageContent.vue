<template>
  <v-container class="cards__wrapper">
    <h2 class="text-uppercase mb-8">Текущие задачи</h2>
    <v-row class="cards__controls" no-gutters>
      <new-card
          v-model="isCreateCard"
          :taskCards="cards"
      >
        <template v-slot:activator="{ on }">
          <v-btn
              text
              @click="createNewCard"
              class="cards__add-button d-block mr-8 mb-8"
              v-on="on"
          >
            добавить задачу
          </v-btn>
        </template>
      </new-card>

      <v-text-field
          v-model="searchText"
          @input="searchCard"
          placeholder="Искать по названию">
        <v-icon
            slot="append"
        >
          mdi-file-find-outline
        </v-icon>
      </v-text-field>
    </v-row>
    <ul class="cards__list">
      <li
          class="cards__item mr-8 mb-8"
          v-for="(card, i) in taskCards"
          :key="i">
        <task-card
            :taskCard="card"
            @deleteCard="deleteCard(i)"
            @setCardToArchive="setCardToArchive(i)"
        >
        </task-card>
      </li>
    </ul>
  </v-container>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';
import TaskCard from '@/components/TaskCard.vue';
import NewCard from '@/components/NewCard.vue';
import { CardData } from '@/types/cardData';

@Component({
  name: 'PageContent',
  components: { TaskCard, NewCard },
})

export default class PageContent extends Vue {
  @Prop({ type: Array, required: true }) public cards!: CardData[];

  public isCreateCard: boolean = false;
  public searchText: string = '';

  public taskCards: CardData[] = this.cards;

  public createNewCard(): void {
    this.isCreateCard = true;
  }

  public setCardToArchive(index: number): void {
    const archivedCard = this.cards.splice(index, 1);
    this.$emit('setCardToArchive', archivedCard);
  }

  public deleteCard(index: number): void {
    this.cards.splice(index, 1);
  }

  public searchCard(): void {
    if (this.searchText.toLowerCase()) {
      this.taskCards = this.cards.filter((el) => el.title.toLowerCase().indexOf(this.searchText.toLowerCase()) !== -1);
    } else if (this.searchText.toLowerCase() === '') {
      this.taskCards = this.cards;
    }

  }
}
</script>

<style lang="scss">
.cards {
  &__wrapper {
    width: 100%;
    max-width: 1280px;
  }

  &__list {
    display: flex;
    justify-content: flex-start;
    flex-wrap: wrap;
    position: relative;
    list-style: none;
    margin: 0;
    padding: 0 !important;
  }

  &__item {
    min-height: 160px;
  }

  &__add-button {
    width: 320px;
    border: 1px dashed black;
    height: unset !important;
    min-height: 160px;
  }


}

</style>
