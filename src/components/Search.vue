<template>
<v-container>
    <v-layout row wrap justify-space-around>
     <v-flex xs4 >
        <v-select
          :items="cities"
          v-model="select"
          outline
        ></v-select>
     </v-flex>
    <v-flex xs>
        <v-autocomplete
        append-icon="search"
        :loading="loading"
        :items="cities"
        :search-input.sync="search"
        v-model="select"
        cache-items
        class="mx-3"
        flat
        hide-no-data
        hide-details
        label="What city are you from?"
        @change="getCities"
        solo outline
        ></v-autocomplete>
      </v-flex>
    </v-layout>
    <v-layout>
      <v-flex>
        <h1 v-if="banks.length >0">{{banks[3]}}</h1>
        <h1 v-else-if="!search">enter a city</h1>
        <h1  v-else>Loading banks in {{upperCase}} Please wait.</h1>
      </v-flex>
    </v-layout>
</v-container>

</template>
<script>
  export default {
    data () {
      return {
        loading: false,
        items: [],
        banks:[],
        search: '',
        select: null,
        cities: [
            'Hyderabad',
            'Mumbai',
            'Chennai',
            'Kolkata',
            'Bhubaneshwar',
            'Bangalore',
            'Allahabad'
            
        ]
      }
    },
    watch: {
      search (val) {
        val && val !== this.select && this.querySelections(val)
        if(val){
          this.getCities();
          console.log(this.banks);
        }

      }
    },
    methods: {
      querySelections (v) {
        this.loading = true
        // Simulated ajax query
        setTimeout(() => {
          this.items = this.cities.filter(e => {
            return (e || '').toLowerCase().indexOf((v || '').toLowerCase()) > -1
          })
          this.loading = false
        }, 500)
      },
      getCities(){
        this.banks=[];
        fetch(`https://vast-shore-74260.herokuapp.com/banks?city=${this.upperCase}`)
        .then(res=>res.json())
        .then(data=>{
          for(var bank in data){
            this.banks.push(data[bank])
          }
          
        })
        // console.log(this.banks);
      }
    },
    computed: {
      upperCase(){
        if(this.search){
          return this.search.toUpperCase()
        }
        
      }
    }
  }
</script>