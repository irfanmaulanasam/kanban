<template>
  <div class="">
    <v-card v-for="(task, index) in tasks" style="margin:5px;" :key="index">
      <v-card-title
        ><h4>{{ task.title }}</h4></v-card-title
      >
      <v-divider></v-divider>
      <v-card-text>
        {{ task.description }}
      </v-card-text>
      <v-card-text> Assigned to : {{ task.for }} </v-card-text>
      <v-divider></v-divider>
      <v-card-actions>
        <v-layout row justify-center>
          <v-dialog v-model="dialog" persistent max-width="600px">
            <v-btn slot="activator" color="green darken-1" dark
              >Show Details</v-btn
            >
            <v-card>
              <v-card-title>
                <span class="headline">Detail Task: {{ task.title }}</span>
              </v-card-title>
              <v-card-text>
                <v-divider></v-divider>
                <v-container grid-list-md>
                  <v-layout wrap>
                    <v-card-text>
                      <strong>Description:</strong> {{ task.description }}
                    </v-card-text>
                    <v-card-text>
                      <strong>Assigned to:</strong> {{ task.for }}
                    </v-card-text>
                    <v-card-text>
                      <strong>Status:</strong> {{ task.status }}
                    </v-card-text>
                  </v-layout>
                </v-container>
                <v-divider></v-divider>
                <v-btn color="danger" dark @click="deleteTask(task.id)"
                  >Delete</v-btn
                >
              </v-card-text>
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn
                  v-if="status == 'To-Do'"
                  color="primary"
                  dark
                  @click="changeStatus(task.id, 'Back-Log')"
                  >Back-Log</v-btn
                >
                <v-btn
                  v-if="status == 'Back-Log' || status == 'Going'"
                  color="info"
                  dark
                  @click="changeStatus(task.id, 'To-Do')"
                  >To-Do</v-btn
                >
                <v-btn
                  v-if="status == 'To-Do' || status == 'Done'"
                  color="warning"
                  dark
                  @click="changeStatus(task.id, 'Going')"
                  >Going</v-btn
                >
                <v-btn
                  v-if="status == 'Going'"
                  color="success"
                  dark
                  @click="changeStatus(task.id, 'Done')"
                  >Done</v-btn
                >
                <v-btn color="blue darken-1" flat @click="dialog = false"
                  >Cancel</v-btn
                >
              </v-card-actions>
            </v-card>
          </v-dialog>
        </v-layout>
      </v-card-actions>
    </v-card>
  </div>
</template>

<script>
import database from "../assets/config.js";

export default {
  data: () => ({
    dialog: false
  }),
  methods: {
    changeStatus: function(id, newStatus) {
      database
        .collection("tasks")
        .doc(id)
        .update({
          status: newStatus
        })
        .then(() => {
          console.log("Document successfully written!");
          this.dialog = false;
        })
        .catch(error => {
          console.error("Error writing document: ", error);
        });
    },

    deleteTask: function(id) {
      database
        .collection("tasks")
        .doc(id)
        .delete()
        .then(function() {
          console.log("Document successfully deleted!");
        })
        .catch(function(error) {
          console.error("Error removing document: ", error);
        });
    }
  },
  props: ["cards", "tasks", "status"]
};
</script>

<style></style>
