<template>
  <v-form v-model="valid">
    <v-container>
      <v-row justify="space-between">
        <v-col cols="12">
          <v-text-field hide-details="auto" dense v-model.lazy="baseBeing.name" label="Name" class="ma-0 pa-0"
                        required outlined :rules="[textFieldRules.required]"></v-text-field>
        </v-col>
        <v-col cols="6">
          <v-text-field hide-details="auto" dense v-model.number.lazy="baseBeing.coordinates.x" label="X" class="ma-0 pa-0"
                        outlined :rules="[textFieldRules.required, textFieldRules.isInteger]"></v-text-field>
        </v-col>
        <v-col cols="6">
          <v-text-field hide-details="auto" dense v-model.number.lazy="baseBeing.coordinates.y" label="Y" class="ma-0 pa-0"
                        outlined :rules="[textFieldRules.required, textFieldRules.isFloat]"></v-text-field>
        </v-col>
        <v-col cols="6">
          <v-switch hide-details v-model="baseBeing.realHero" label="Real hero"></v-switch>
        </v-col>
        <v-col cols="6">
          <v-switch hide-details v-model="baseBeing.hasToothpick" label="Has toothpick"></v-switch>
        </v-col>
        <v-col cols="12">
          <v-text-field dense hide-details="auto" v-model.number.lazy="baseBeing.impactSpeed" label="Impact speed"
                        outlined :rules="[textFieldRules.required, textFieldRules.isFloat]"></v-text-field>
        </v-col>
        <v-col cols="12">
          <v-text-field dense hide-details="auto" v-model.number.lazy="baseBeing.minutesOfWaiting" label="Minutes of waiting"
                        outlined :rules="[textFieldRules.required, textFieldRules.isInteger]"></v-text-field>
        </v-col>
        <v-col cols="6">
          <v-select dense hide-details v-model="baseBeing.mood"
                    item-text="text"
                    item-value="value"
                    outlined :items="moods" label="Mood"></v-select>
        </v-col>
        <v-col cols="6">
          <v-select dense hide-details v-model="baseBeing.weaponType"
                    item-text="text"
                    item-value="value"
                    outlined :items="weaponTypes" label="Weapon type"></v-select>
        </v-col>
        <v-col cols="12">
          <v-select dense outlined hide-details
                    v-model="baseBeing.car"
                    :items="cars"
                    item-text="text"
                    item-value="value" label="Car"></v-select>
        </v-col>
        <v-col cols="6">
          <v-btn block @click="submit" :disabled="!valid" color="success">
            Save
          </v-btn>
        </v-col>
        <v-col cols="6">
          <v-btn block @click="$emit('close-form')" color="red">
            Cancel
          </v-btn>
        </v-col>
      </v-row>
    </v-container>
  </v-form>
</template>

<script>
export default {
  name: "CreateBeingForm",
  props: {
    being: {
      type: Object,
      required: true
    }
  },
  data: function () {
    return {
      valid: false,
      textFieldRules: {
        required: v => !!v || 'Required',
        isInteger: v => (!isNaN(Number(v)) && Number(v) % 1 === 0) || 'Required integer',
        isFloat: v => (!isNaN(Number(v))) || 'Required float'
      },
      baseBeing: {
        name: this.being.name,
        realHero: this.being.realHero,
        hasToothpick: this.being.hasToothpick,
        coordinates: this.being.coordinates,
        impactSpeed: this.being.impactSpeed,
        minutesOfWaiting: this.being.minutesOfWaiting,
        weaponType: this.being.weaponType,
        mood: this.being.mood,
        car: this.being.car,
        isEditFormOpen: true
      },
      moods: [
        {text: "No", value: null},
        {text: "SADNESS", value: "SADNESS"},
        {text: "GLOOM", value: "GLOOM"},
        {text: "CALM", value: "CALM"},
        {text: "RAGE", value: "RAGE"},
        {text: "FRENZY", value: "FRENZY"}
      ],
      weaponTypes: [
        {text: "No", value: null},
        {text: "AXE", value: "AXE"},
        {text: "HAMMER", value: "HAMMER"},
        {text: "PISTOL", value: "PISTOL"},
        {text: "MACHINE_GUN", value: "MACHINE_GUN"}
      ],
      cars: [
        {text: "No", value: null},
        {text: "COOL", value: {cool: true}},
        {text: "NOT COOL", value: {cool: false}},
      ]
    }
  },
  methods: {
    submit() {
      this.$emit('submit-form', this.baseBeing)
    }
  }
}
</script>

<style scoped>

</style>