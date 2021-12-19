<template>
  <v-row justify="center" class="d-block edit-form" no-gutters>
    <v-dialog
        persistent
        max-width="600px"
        v-model="dialog"
    >
      <template #activator="bindings">
        <slot name="activator" v-bind="bindings"/>
      </template>
      <v-card>
        <v-card-title>
          <span class="text-h5">Редактирование задачи</span>
        </v-card-title>
        <v-card-text>
          <v-container>
            <v-row>
              <v-col
                  cols="12"
              >
                <v-text-field
                    label="Название"
                    v-model="titleField"
                    required
                ></v-text-field>
              </v-col>
              <v-col
                  cols="12"
              >
                <v-textarea
                    label="Описание"
                    v-model="descriptionField"
                    required
                ></v-textarea>
              </v-col>
              <v-col
                  cols="12"
                  sm="6"
                  md="4"
              >
                <v-menu
                    v-model="dateField"
                    :close-on-content-click="false"
                    :nudge-right="40"
                    transition="scale-transition"
                    offset-y
                    min-width="auto"
                >
                  <template v-slot:activator="{ on, attrs }">
                    <v-text-field
                        v-model="taskDateField"
                        label="Срок выполнения"
                        prepend-icon="mdi-calendar"
                        readonly
                        v-bind="attrs"
                        v-on="on"
                    ></v-text-field>
                  </template>
                  <v-date-picker
                      v-model="taskDateField"
                      @input="dateField = false"
                  ></v-date-picker>
                </v-menu>
              </v-col>
              <v-col
                  cols="12"
              >
                <span class="subheading">Приоритет</span>
                <v-chip-group
                    v-model="taskCardToEdit.is_main ? selection = 0 : selection = 1"
                    active-class="deep-purple--text text--accent-4"
                    mandatory
                >
                  <v-chip
                      @click="taskPriority = true">Высокий
                  </v-chip>
                  <v-chip
                      @click="taskPriority = false">Низкий
                  </v-chip>
                </v-chip-group>
              </v-col>
            </v-row>
          </v-container>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
              color="blue darken-1"
              text
              @click="closeModal"
          >
            Close
          </v-btn>
          <v-btn
              color="blue darken-1"
              text
              @click="saveEditData"
          >
            Save
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-row>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';
import { CardData } from '@/types/cardData';

@Component({
  name: 'EditCard',
  components: {},
})

export default class EditCard extends Vue {
  @Prop({ type: Object, required: true }) public taskCardToEdit!: CardData;

  public dialog: boolean = false;
  public titleField: string = this.taskCardToEdit.title;
  public descriptionField: string = this.taskCardToEdit.value;
  public taskDateField: string = this.taskCardToEdit.date;
  public dateField: boolean = false;
  public taskPriority: boolean = this.taskCardToEdit.is_main;
  public selection: number = 0;

  public closeModal(): any {
    this.dialog = false;
  }

  public saveEditData(): any {
    this.dialog = false;
    this.taskCardToEdit.title = this.titleField;
    this.taskCardToEdit.value = this.descriptionField;
    this.taskCardToEdit.date = this.taskDateField;
    this.taskCardToEdit.is_main = this.taskPriority;
  }

}

</script>

<style lang="scss" scoped>
.edit-form {
  flex: unset;
}
</style>
