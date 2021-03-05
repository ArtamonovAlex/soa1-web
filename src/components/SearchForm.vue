<template>
  <v-form v-model="valid">
    <v-row>
      <v-col cols="2">
        <div class="text-h6 text-center">Filters</div>
      </v-col>
    </v-row>
      <v-row class="ma-0">
        <v-col cols="2">
          <v-text-field hide-details="auto" dense v-model.lazy="filterBeing.id" label="Id" class="ma-0 pa-0"
                        outlined></v-text-field>
        </v-col>
        <v-col cols="2">
          <v-text-field hide-details="auto" dense v-model.lazy="filterBeing.name" label="Name" class="ma-0 pa-0"
                        outlined></v-text-field>
        </v-col>
        <v-col cols="2">
          <v-text-field hide-details="auto" dense v-model.number.lazy="filterBeing.coordinates.x" label="X" class="ma-0 pa-0"
                        outlined></v-text-field>
        </v-col>
        <v-col cols="2">
          <v-text-field hide-details="auto" dense v-model.number.lazy="filterBeing.coordinates.y" label="Y" class="ma-0 pa-0"
                        outlined></v-text-field>
        </v-col>
        <v-col cols="2">
          <v-text-field  dense hide-details v-model="filterBeing.realHero" label="Real hero"
                         outlined></v-text-field>
        </v-col>
        <v-col cols="2">
          <v-text-field dense hide-details v-model="filterBeing.hasToothpick" label="Has toothpick"
                        outlined></v-text-field>
        </v-col>
        <v-col cols="2">
          <v-text-field dense hide-details="auto" v-model.lazy="filterBeing.creationDate" label="Creation date"
                        outlined></v-text-field>
        </v-col>
        <v-col cols="2">
          <v-text-field dense hide-details="auto" v-model.number.lazy="filterBeing.impactSpeed" label="Impact speed"
                        outlined></v-text-field>
        </v-col>
        <v-col cols="2">
          <v-text-field dense hide-details="auto" v-model.number.lazy="filterBeing.minutesOfWaiting" label="Minutes of waiting"
                        outlined></v-text-field>
        </v-col>
        <v-col cols="2">
          <v-text-field dense hide-details="auto" v-model.number.lazy="filterBeing.mood" label="Mood"
                        outlined></v-text-field>
        </v-col>
        <v-col cols="2">
          <v-text-field dense hide-details="auto" v-model.number.lazy="filterBeing.weaponType" label="Weapon type"
                        outlined></v-text-field>
        </v-col>
        <v-col cols="2">
          <v-text-field dense hide-details="auto" v-model.number.lazy="filterBeing.car" label="Car(true/false)"
                        outlined></v-text-field>
        </v-col>
      </v-row>
    <v-row>
      <v-col cols="2">
        <div class="text-h6 text-center">Sorters</div>
      </v-col>
    </v-row>
    <v-row class="ma-0">
      <v-col cols="2">
        <v-select
            dense
            v-model.lazy="sortingBeing.id"
            hide-details="auto"
            :items="items"
            label="Id"
            outlined
            class="ma-0 pa-0"
        ></v-select>
      </v-col>
      <v-col cols="2">
        <v-select
            dense
            v-model.lazy="sortingBeing.name"
            hide-details="auto"
            :items="items"
            label="Name"
            outlined
            class="ma-0 pa-0"
        ></v-select>
      </v-col>
      <v-col cols="2">
        <v-select
            dense
            v-model.lazy="sortingBeing.coordinates.x"
            hide-details="auto"
            :items="items"
            label="X"
            outlined
            class="ma-0 pa-0"
        ></v-select>
      </v-col>
      <v-col cols="2">
        <v-select
            dense
            v-model.lazy="sortingBeing.coordinates.y"
            hide-details="auto"
            :items="items"
            label="Y"
            outlined
            class="ma-0 pa-0"
        ></v-select>
      </v-col>
      <v-col cols="2">
        <v-select
            dense
            v-model.lazy="sortingBeing.realHero"
            hide-details="auto"
            :items="items"
            label="Real hero"
            outlined
            class="ma-0 pa-0"
        ></v-select>
      </v-col>
      <v-col cols="2">
        <v-select
            dense
            v-model.lazy="sortingBeing.hasToothpick"
            hide-details="auto"
            :items="items"
            label="Has toothpick"
            outlined
            class="ma-0 pa-0"
        ></v-select>
      </v-col>
      <v-col cols="2">
        <v-select
            dense
            v-model.lazy="sortingBeing.creationDate"
            hide-details="auto"
            :items="items"
            label="Creation date"
            outlined
            class="ma-0 pa-0"
        ></v-select>
      </v-col>
      <v-col cols="2">
        <v-select
            dense
            v-model.lazy="sortingBeing.impactSpeed"
            hide-details="auto"
            :items="items"
            label="Impact speed"
            outlined
            class="ma-0 pa-0"
        ></v-select>
      </v-col>
      <v-col cols="2">
        <v-select
            dense
            v-model.lazy="sortingBeing.minutesOfWaiting"
            hide-details="auto"
            :items="items"
            label="Minutes of waiting"
            outlined
            class="ma-0 pa-0"
        ></v-select>
      </v-col>
      <v-col cols="2">
        <v-select
            dense
            v-model.lazy="sortingBeing.mood"
            hide-details="auto"
            :items="items"
            label="Mood"
            outlined
            class="ma-0 pa-0"
        ></v-select>
      </v-col>
      <v-col cols="2">
        <v-select
            dense
            v-model.lazy="sortingBeing.weaponType"
            hide-details="auto"
            :items="items"
            label="Weapon type"
            outlined
            class="ma-0 pa-0"
        ></v-select>
      </v-col>
      <v-col cols="2">
        <v-select
            dense
            v-model.lazy="sortingBeing.car"
            hide-details="auto"
            :items="items"
            label="Car"
            outlined
            class="ma-0 pa-0"
        ></v-select>
      </v-col>
    </v-row>
    <v-row justify="end" class="ma-0">
      <v-col cols="2">
        <v-text-field hide-details="auto" dense v-model.number.lazy="meta.page" label="Page" class="ma-0 pa-0"
                      outlined :rules="[textFieldRules.required, textFieldRules.isInteger]"></v-text-field>
      </v-col>
      <v-col cols="2">
        <v-text-field hide-details="auto" dense v-model.number.lazy="meta.count" label="Count" class="ma-0 pa-0"
                      outlined :rules="[textFieldRules.required, textFieldRules.isInteger]"></v-text-field>
      </v-col>
      <v-col cols="4">
        <v-btn color="teal" block @click="submit" :disabled="!valid" width="200px">
          Получить данные
        </v-btn>
      </v-col>
    </v-row>
    <v-divider class="ma-4"/>
    <v-row justify="end" class="ma-0">
      <v-col cols="8">
        <v-btn color="teal" block @click="find" width="300px">
          Получить всех созданий с именем на:
        </v-btn>
      </v-col>
      <v-col cols="4">
        <v-text-field hide-details="auto" dense v-model.lazy="specialName" label="Starts with" class="ma-0 pa-0"
                      outlined></v-text-field>
      </v-col>
    </v-row>
    <v-row justify="end" class="ma-0">
      <v-col cols="5">
        <div class="text-h6 text-center">Получить количество созданий с машиной</div>
      </v-col>
      <v-col cols="1">
        <v-btn color="teal" block @click="less">&lt;</v-btn>
      </v-col>
      <v-col cols="1">
        <v-btn color="teal" block @click="equal">=</v-btn>
      </v-col>
      <v-col cols="2">
        <v-select
            dense
            v-model.lazy="car"
            hide-details="auto"
            :items="cars"
            label="Car"
            outlined
            class="ma-0 pa-0"
        ></v-select>
      </v-col>
      <v-col cols="2">
        <div class="text-h6">: {{ countLabel }}</div>
      </v-col>
    </v-row>
  </v-form>
</template>

<script>

import axios from "axios";

export default {
  name: "SearchForm",
  data: function () {
    return {
      specialName: '',
      countLabel: 0,
      valid: true,
      textFieldRules: {
        required: v => !!v || 'Required',
        isInteger: v => (!isNaN(Number(v)) && Number(v) % 1 === 0) || 'Required integer'
      },
      meta: {
        page: 1,
        count: 20
      },
      filterBeing: {
        id: '',
        name: '',
        creationDate: '',
        realHero: '',
        hasToothpick: '',
        coordinates: {
          x: '',
          y: ''
        },
        impactSpeed: '',
        minutesOfWaiting: '',
        weaponType: '',
        mood: '',
        car: null
      },
      sortingBeing: {
        id: null,
        name: null,
        creationDate: null,
        realHero: null,
        hasToothpick: null,
        coordinates: {
          x: null,
          y: null
        },
        impactSpeed: null,
        minutesOfWaiting: null,
        weaponType: null,
        mood: null,
        car: null
      },
      items:[
        {text: "No", value: null},
        {text: "Asc", value: "asc"},
        {text: "Desc", value: "desc"}
      ],
      car: null,
      cars: [
        {text: "No", value: null},
        {text: "COOL", value: true},
        {text: "NOT COOL", value: false},
      ]
    }
  },
  methods: {
    submit() {
      this.$emit('submit-form', this.filterBeing, this.sortingBeing, this.meta)
    },
    find() {
      this.$emit('find', this.specialName)
    },
    less() {
      let app = this
      axios.get('https://localhost:47263/soa1/human-beings/count/car/cool/less/' + this.car, {headers: {
          'Content-Type': 'application/xml',
        }}).then((response) => {
        if (response.data !== '') {
          app.countLabel = response.data
        }
      }).catch(() => {
        // alert
      })
    },
    equal() {
      let app = this
      axios.get('https://localhost:47263/soa1/human-beings/count/car/cool/equals/' + this.car, {headers: {
          'Content-Type': 'application/xml',
        }}).then((response) => {
        if (response.data !== '') {
          app.countLabel = response.data
        }
      }).catch(() => {
        // alert
      })
    }
  }
}
</script>

<style scoped>

</style>