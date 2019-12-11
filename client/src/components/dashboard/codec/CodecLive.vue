<template>
  <v-card
    popout
    v-if="codecs"
  >
    <v-card-title
      class="headline"
      style="justify-content: center;"
    >Live Proximity</v-card-title>
    <v-container>
      <div style="display:flex;">
        <v-icon style="padding-right:3%; padding-left:3%;">wifi_off</v-icon>
        <v-switch
          v-model="switch1"
          label="Désactiver Proximity de tous les endpoints du Showroom"
          @change="switchChange(codecs)"
        >
        </v-switch>
      </div>
      <v-flex
        v-for="codec in codecs"
        :key="codec._id"
        class="pa-4 elevation-1"
      >
        <div
          slot="header"
          class="title"
          :class="{'red--text': codec.error}"
        >
          {{ codec.name }}
        </div>

        <v-slider
          label="Proximity Volume"
          v-model="codec.proximity"
          thumb-label
          :max="maxValue"
          :min="minValue"
        >
        </v-slider>

        <v-btn
          color="success"
          flat="flat"
          @click="submit(codec); snackbar = true;"
        >
          <v-icon left>wifi</v-icon>
          Modifier
        </v-btn>

        <v-btn
          small
          fab
          color="info"
          flat="flat"
          @click="max(codec)"
        >
          ON
        </v-btn>

        <v-btn
          small
          fab
          color="info"
          flat="flat"
          @click="min(codec)"
        >
          OFF
        </v-btn>
      </v-flex>
      <v-snackbar
        v-model="snackbar"
        color="green"
        :timeout="timeout"
      >
        <v-icon left>send</v-icon>
        Requête envoyée avec succès
      </v-snackbar>
    </v-container>
  </v-card>
</template>

<script>
export default {
  data() {
    return {
      codecCopy: null,
      codecId: null,
      snackbar: false,
      timeout: 2000,
      switch1: false,
      minValue: 0,
      maxValue: 70
    };
  },
  methods: {
    max(codec) {
      codec.proximity = 70;
    },
    min(codec) {
      codec.proximity = 0;
    },
    switchChange(codecs) {
      if (this.switch1) {
        codecs.forEach(element => {
          element.proximity = 0;
        });
      } else {
        codecs.forEach(element => {
          element.proximity = 70;
        });
      }
    },
    submit(codec) {
      this.codecCopy = {
        name: codec.name,
        type: codec.type,
        ip: codec.ip,
        mac: codec.mac,
        login: codec.login,
        password: codec.password,
        cloud: codec.cloud,
        team: codec.team,
        floor: codec.floor,
        proximity: codec.proximity
      };
      this.codecId = codec._id;
      var props = [];

      for (var prop in this.codecCopy) {
        props.push({
          propName: prop,
          value: this.codecCopy[prop]
        });
      }

      this.$store.dispatch("updateProps", {
        id: this.codecId,
        props: props
      });
    }
  },
  computed: {
    codecs() {
      return this.$store.getters.codecs;
    }
  }
};
</script>

