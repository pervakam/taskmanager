<template>
  <v-card
      elevation="0"
      class="cards__task-card d-flex flex-column justify-space-between pa-5"
      :class="checkDate(taskCard.date) ? 'cards__task-card--expired' : ''"
  >
    <div class="cards__task-card-accent"
         :class="taskCard.is_main ? 'cards__task-card-accent--main' : 'cards__task-card-accent--minor'"></div>
    <div>
      <v-card-title class="pa-0 ma-0 mb-6">{{ taskCard.title }}</v-card-title>
      <v-card-subtitle class="pa-0 ma-0">{{ taskCard.value }}</v-card-subtitle>
      <v-card-subtitle class="pa-0 ma-0">Срок до {{ taskCard.date }}</v-card-subtitle>
    </div>
    <div
        class="d-flex justify-end align-start"
    >
      <v-btn
          class="cards__to-archive-button"
          icon
          @click="setCardToArchive"
      >
        <v-icon
            color="green darken-2"
        >
          mdi-bookmark-check-outline
        </v-icon>
      </v-btn>
      <edit-card
          v-model="isEditCard"
          :taskCardToEdit="taskCard"
      >
        <template v-slot:activator="{ on }">
          <v-btn
              class="cards__edit-button"
              icon
              @click="editCard"
              v-on="on"
          >
            <v-icon
                color="blue darken-2"
            >
              mdi-playlist-edit
            </v-icon>
          </v-btn>
        </template>
      </edit-card>
      <v-btn
          icon
          @click="deleteCard"
      >
        <v-icon
            color="red darken-2"
        >
          mdi-trash-can-outline
        </v-icon>
      </v-btn>
    </div>
  </v-card>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';
import EditCard from '@/components/EditCard.vue';

@Component({
  name: 'TaskCard',
  components: { EditCard },
})

export default class TaskCard extends Vue {
  @Prop({ type: Object, required: true }) public taskCard!: {};
  public isEditCard: boolean = false;

  public deleteCard(): void {
    this.$emit('deleteCard');
  }

  public checkDate(date: string): void | boolean {
    const cardDate = new Date(date);
    const currentDate = new Date((new Date(Date.now() - (new Date()).getTimezoneOffset() * 60000)).toISOString().substr(0, 10));

    return cardDate < currentDate;
  }

  public editCard(): void {
    this.isEditCard = true;
  }

  public setCardToArchive(): void {
    this.$emit('setCardToArchive');
  }
}

</script>

<style lang="scss" scoped>
.cards {
  &__task-card {
    width: 320px;
    height: 100%;
    box-shadow: 0 8px 16px 8px rgba(0, 0, 0, 0.25) !important;

    &--expired {
      box-shadow: 0px 8px 16px 8px rgba(231, 31, 31, 0.25) !important;
    }
  }

  &__task-card-accent {
    position: absolute;
    top: 8px;
    right: 8px;
    width: 8px;
    height: 8px;
    border-radius: 50%;

    &--main {
      background-color: red;
    }

    &--minor {
      background-color: green;
    }
  }
}
</style>
