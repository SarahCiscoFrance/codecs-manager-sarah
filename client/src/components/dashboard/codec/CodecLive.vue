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
        <v-icon
          :color="iconColor"
          style="padding-right:3%; padding-left:3%;"
        >wifi_off</v-icon>
        <v-switch
          v-model="switch1"
          label="Régler à nulle la valeur de Proximity sur tous les endpoints du Showroom"
          :color="iconColor"
          @change="switchChange(codecs)"
        >
        </v-switch>
        <v-btn
          color="success"
          flat="flat"
          @click="submitAll(codecs); snackbar = true;"
        >
          <v-icon left>leak_add</v-icon>
          Modifier pour tous
        </v-btn>
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
      iconColor: "grey",
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
        this.iconColor = "red";
        codecs.forEach(element => {
          element.proximity = 0;
        });
      } else {
        this.iconColor = "grey";
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
    },
    async submitAll(codecs) {
      //obligé de passer par un for sinon await pas reconnu
      for (var index = 0; index < codecs.length; index++) {
        var codec = codecs[index];
        console.log("request being sent");
        this.submit(codec);
        var result = await resolveAfterSeconds();
        console.log(result + " for codec" + codec.ip);
      }
    }
  },
  computed: {
    codecs() {
      return this.$store.getters.codecs;
    }
  }
};

//cette fonction permet de brider la boucle for de submitAll()
function resolveAfterSeconds() {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve("request sending");
    }, 100);
  });
}
</script>

