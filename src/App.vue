<template>
  <v-app>
    <v-app-bar color="teal" app>
    </v-app-bar>

    <v-main class="pa-6 pt-16">
        <v-row>
          <v-col cols="12">
            <v-card>
              <search-form @submit-form="getAll"
                           @find="findByName"></search-form>
            </v-card>
          </v-col>
        </v-row>
        <v-row dense>
          <v-col v-for="(being, index) in humanBeings" :key="being.id">
            <v-card min-width="500px">
              <div v-if="!being.isEditFormOpen">
                <v-card-title class="teal--text text--accent-3">
                  <span>ID: {{being.id}}</span>
                  <v-spacer></v-spacer>
                  <v-btn icon @click="openEditForm(index, being)" class="edit-button">
                    <v-icon color="orange" class="text--lighten-2">mdi-pencil</v-icon>
                  </v-btn>
                  <v-btn icon @click="deleteBeing(index)" class="delete-button">
                    <v-icon color="red" class="text--lighten-1">mdi-close</v-icon>
                  </v-btn>
                </v-card-title>
                <v-card-text>
                  <div>Name: {{being.name}}</div>
                  <div>Creation Date: {{being.creationDate}}</div>
                  <div>Coordinates: ({{being.coordinates.x}}, {{being.coordinates.y}})</div>
                  <div>Real hero: {{being.realHero}}</div>
                  <div>Has toothpick: {{being.hasToothpick}}</div>
                  <div>Impact speed: {{being.impactSpeed}}</div>
                  <div>Minutes of waiting: {{being.minutesOfWaiting}}</div>
                  <div>Weapon type: {{being.weaponType == null ? "No" : being.weaponType}}</div>
                  <div>Mood: {{being.mood == null ? "No" : being.mood}}</div>
                  <div>Car: {{being.car == null ? "No" : being.car.cool ? "cool" : "not cool"}}</div>
                </v-card-text>
              </div>
              <create-being-form :being="being" v-if="being.isEditFormOpen"
                        @close-form="closeEditForm(index)" @submit-form="editBeing(index, $event)">
              </create-being-form>
            </v-card>
          </v-col>
          <v-col>
            <v-card id="plus-card" min-width="200px">
              <v-btn block v-if="!isCreateFormOpen" @click="isCreateFormOpen = true">
                <v-icon>mdi-plus</v-icon>
              </v-btn>
              <v-container v-if="isCreateFormOpen">
                <v-alert v-model="createAlert" transition="scale-transition"
                         color="teal"
                         border="top"
                         dismissible>
                  Something went wrong
                </v-alert>
                <create-being-form :being="newBeing"
                                   @close-form="closeCreateForm" @submit-form="saveNewBeing">
                </create-being-form>
              </v-container>

            </v-card>
          </v-col>
        </v-row>
    </v-main>

      <v-footer app>
        <span>Aleksandr Artamonov &copy; 2020</span>
      </v-footer>

  </v-app>
</template>

<script>
import axios from "axios";

import CreateBeingForm from "@/components/CreateBeingForm";
import SearchForm from "@/components/SearchForm";

const xml2js = require('xml2js');
const parser = new xml2js.Parser({explicitArray : false});

export default {
  name: 'App',
  components: {
    CreateBeingForm,
    SearchForm
  },
  data: () => ({
    isCreateFormOpen: false,
    createAlert: false,
    humanBeings: [],
    newBeing: {
      name: '',
      creationDate: '',
      realHero: true,
      hasToothpick: false,
      impactSpeed: null,
      minutesOfWaiting: null,
      coordinates: {
        x: null,
        y: null,
      },
      weaponType: null,
      mood: null,
      car: null,
      isEditFormOpen: false
    },
  }),
  methods: {
    getBeingsFromUrl(url) {
      this.humanBeings = []
      let App = this
      console.log('url: ' + url)
      axios.get(url).then(response => {
        if (response.data !== ''){
          parser.parseString(response.data, function (err, result) {
            console.log(JSON.stringify(result))
            let loadedBeings = result.root.data.humanBeing;
            if (loadedBeings !== undefined) {
              if (!Array.isArray(loadedBeings)) {
                loadedBeings = [loadedBeings]
              }
              App.humanBeings.push(...loadedBeings.map(a => {
                a.id = a.$.id
                a.hasToothpick = a.hasToothpick === "true"
                a.realHero = a.realHero === "true"
                if (a.car != null) {
                  a.car.cool = a.car.cool === "true"
                } else {
                  a.car = null
                }
                a.mood = a.mood == null ? null : a.mood
                a.weaponType = a.weaponType == null ? null : a.weaponType
                return a
              }))
            }
          });
        }
      }).catch(() => {
        // alert
      })
    },
    getAll(filterBeing = {}, sortingBeing = {}, meta = {page: 1, count: 20}) {
      let filterQ = this.prepareFilter(filterBeing)
      let sortQ = this.prepareSort(sortingBeing)
      var url = 'https://localhost:47263/soa1/human-beings'
      url += '?page=' + meta.page + '&count=' + meta.count
      if (sortQ !== '') {
        url += '&' + sortQ
      }
      if (filterQ !== '') {
        url += '&' + filterQ
      }
      this.getBeingsFromUrl(url)
    },
    saveNewBeing(being) {
      let app = this
      let xml = this.prepareBeing(being)
      console.log("sending: " + xml)
      axios.post('https://localhost:47263/soa1/human-beings', xml, {headers: {
          'Content-Type': 'application/xml',
        }}).then(response => {
        if (response.status === 201) {
          app.closeCreateForm()
          app.getAll()
        } else {
          app.createAlert = true
        }
      }).catch(() => {
        app.createAlert = true
      })
    },
    deleteBeing(index) {
      let app = this
      let idToDelete = this.humanBeings[index].id
      axios.delete('https://localhost:47263/soa1/human-beings/' + idToDelete).then(() => {
        app.getAll()
      }).catch(() => {
        // alert
      })
    },
    findByName(name) {
      this.getBeingsFromUrl('https://localhost:47263/soa1/human-beings/name/starts_with/' + name)
    },
    openEditForm(index, being) {
      being.isEditFormOpen = true
      this.$set(this.humanBeings, index, being)
    },
    closeEditForm(index) {
      let being = this.humanBeings[index]
      being.isEditFormOpen = false
      this.$set(this.humanBeings, index, being)
    },
    editBeing(index, being) {
      let app = this
      let idToEdit = this.humanBeings[index].id
      let xml = this.prepareBeing(being)
      console.log("sending: " + xml)
      axios.put('https://localhost:47263/soa1/human-beings/' + idToEdit, xml, {headers: {
          'Content-Type': 'application/xml',
        }}).then(() => {
        this.closeEditForm(index)
        app.getAll()
      }).catch(() => {
        // alert
      })
    },
    closeCreateForm() {
      this.newBeing = {
        name: '',
        creationDate: '',
        realHero: true,
        hasToothpick: false,
        impactSpeed: null,
        coordinates: {
          x: null,
          y: null,
        },
        minutesOfWaiting: null,
        weaponType: null,
        mood: null,
        car: null,
        isEditFormOpen: false
      }
      this.isCreateFormOpen = false
    },
    prepareBeing(being) {
      let builder = new xml2js.Builder();
      for (let propName in being) {
        if (being[propName] === null || being[propName] === undefined || propName === "isEditFormOpen") {
          delete being[propName];
        }
      }
      being.coordinates.x = Number.parseInt(being.coordinates.x)
      being.minutesOfWaiting = Number.parseInt(being.minutesOfWaiting.toString())
      return builder.buildObject({humanBeing: being});
    },
    prepareFilter(being) {
      let filters = []
      for (let field in being) {
        if (being[field] !== null && being[field] !== '') {
          switch (field) {
            case 'coordinates':
              for (let coord in being[field]) {
                if (being[field][coord] !== null && being[field][coord] !== '') {
                  filters.push('coordinates.' + coord + ':' + being[field][coord])
                }
              }
              break
            case 'car':
              filters.push('car.cool' +  ':' + being[field])
              break
            case 'creationDate':
              filters.push('creationDate' +  ':' + being[field].replaceAll("+", "%2B"))
              break
            default:
              filters.push('' + field + ':' + being[field])
              break
          }
        }
      }
      if (filters.length > 0) {
        return 'filter=' + filters.join(',')
      } else {
        return ''
      }
    },
    prepareSort(being) {
      let sorts = []
      for (let field in being) {
        if (being[field] !== null) {
          switch (field) {
            case 'coordinates':
              for (let coord in being[field]) {
                if (being[field][coord] !== null) {
                  sorts.push('coordinates.' + coord + ':' + being[field][coord])
                }
              }
              break
            case 'car':
              sorts.push('car.cool' +  ':' + being[field])
              break
            default:
              sorts.push('' + field + ':' + being[field])
              break
          }
        }
      }
      if (sorts.length > 0) {
        return 'sort=' + sorts.join(',')
      } else {
        return ''
      }
    }
  },
  created() {
    this.$vuetify.theme.dark = true
  }
}
</script>
