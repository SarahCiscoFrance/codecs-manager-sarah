<template>
  <v-container>
    <v-layout
      row
      wrap
      justify-center
    >
      <v-flex xs12>
        <app-codec-grid></app-codec-grid>
        <v-btn
          fixed
          dark
          fab
          top
          right
          color="success"
          @click="showDialogAdd = true;"
          style="margin-top:65px"
        >
          <v-icon>add</v-icon>
        </v-btn>
        <v-btn
          fixed
          dark
          fab
          top
          right
          color="red"
          @click="showDialogLive = true;"
          style="margin-top:130px"
        >
          <v-icon>cast</v-icon>
        </v-btn>
        <v-dialog
          v-model="showDialogAdd"
          max-width="450"
        >
          <app-codec-add @closeDialog="showDialogAdd = false"></app-codec-add>
        </v-dialog>
        <v-dialog
          v-model="showDialogLive"
          max-width="1000"
        >
          <app-codec-live @closeDialog="showDialogLive = false"></app-codec-live>
        </v-dialog>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
import CodecGrid from "./codec/CodecGrid.vue";
import CodecAdd from "./codec/CodecAdd.vue";
import CodecLive from "./codec/CodecLive.vue";

export default {
  data() {
    return {
      showDialogAdd: false,
      showDialogLive: false
    };
  },
  components: {
    appCodecGrid: CodecGrid,
    appCodecAdd: CodecAdd,
    appCodecLive: CodecLive
  },
  created() {
    this.$store.dispatch("fetchCodecs");
  }
};
</script>

<style>
</style>
