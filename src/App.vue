<template>
  <v-app class="wrapper mx-auto">
    <div class="wrapper__nav d-flex justify-space-between align-center">
      <h1 class="text-uppercase">Планировщик задач</h1>
      <div class="wrapper__nav-links">
        <button @click="setVisibility"
                :class="isBlockVisible ? 'active' : ''">Текущие задачи ({{ cards.length }})
        </button>
        <button @click="setVisibility"
                :class="isBlockVisible ? '' : 'active'">Архив ({{ archivedCards.length }})
        </button>
      </div>
    </div>

    <page-content
        :cards="cards"
        :class="isBlockVisible ? '' : 'show'"
        @setCardToArchive="setCardToArchive"
        @сardСounter="cardCounter">
    </page-content>
    <archive
        :class="isBlockVisible ? 'show' : ''"
        :archivedCards="archivedCards">
    </archive>

    <v-footer
        app
        padless
        color="transparent"
        class="wrapper"
    >
      <v-row class="page-footer">
        <v-col>
          <p>@pervakam</p>
        </v-col>
        <v-col class="d-flex justify-end">
          <a href="https://github.com/pervakam/taskmanager">GitHub</a>
        </v-col>
      </v-row>
    </v-footer>
  </v-app>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import PageContent from '@/components/PageContent.vue';
import Archive from '@/components/Archive.vue';
import { CardData } from '@/types/cardData';

@Component({
  name: 'App',
  components: { PageContent, Archive },
})

export default class App extends Vue {
  public archivedCards: CardData[] = []
  public cards = [
    {
      title: 'Выучить Vue',
      value: 'Всё знать и разбираться',
      date: '2022-01-01',
      is_main: true,
    },
    {
      title: 'Выучить React',
      value: 'Всё знать и разбираться',
      date: '2022-01-01',
      is_main: true,
    },
    {
      title: 'Выучить Angular',
      value: 'Всё знать и разбираться',
      date: '2022-01-01',
      is_main: true,
    },
    {
      title: 'Выучить стихотворение',
      value: 'Всё знать и разбираться, Всё знать и разбираться, Всё знать и разбираться, Всё знать и разбираться',
      date: '2021-01-01',
      is_main: false,
    }
  ]
  public isBlockVisible: boolean = true

  public setCardToArchive(archivedCard: CardData[]): void {
    this.archivedCards.push(archivedCard[0]);
  }

  public setVisibility(): void {
    this.isBlockVisible = !this.isBlockVisible;
  }

  public cardCounter(count: string): string {
    return count
  }
}

</script>

<style lang="scss" scoped>
.wrapper {
  width: 100%;
  max-width: 1280px;
  margin: 0 auto;
  display: flex;
  justify-content: flex-start;
  flex-wrap: wrap;

  &__nav-links {
    button {
      text-decoration: none;
      color: black;
      text-transform: uppercase;
      margin: 0 0 0 24px;
    }
  }

  .active {
    border-bottom: 1px solid black;
  }

  .show {
    display: none;
  }
}

.page-footer {
  a {
    text-decoration: none;
    color: black;

    &:hover {
      text-decoration: underline;
    }
  }
}

</style>
