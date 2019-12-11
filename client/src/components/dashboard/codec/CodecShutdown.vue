<template>
  <v-card>
    <v-card-title class="headline">Etes-vous sûr de vouloir éteindre ce codec ?</v-card-title>

    <v-card-text>
      Vous devrez rallumer le codec manuellement.
    </v-card-text>

    <v-card-actions>
      <v-spacer></v-spacer>

      <v-btn
        color="info"
        flat="flat"
        @click="closeDialog"
      >
        Annuler
      </v-btn>

      <v-btn
        color="success"
        flat="flat"
        @click="shutdownCodec"
      >
        Eteindre
      </v-btn>
    </v-card-actions>
  </v-card>
</template>

<script>
export default {
  props: ["codec"],
  methods: {
    closeDialog() {
      this.$emit("closeDialog");
    },
    shutdownCodec() {
      this.$store.dispatch("shutdownCodec", this.codec);
      this.closeDialog();
    }
  }
};
</script>

