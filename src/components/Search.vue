<template>
<v-container>
    <v-layout row wrap justify-space-around>
     <v-flex xs4 >
        <v-select
          :items="cities"
          v-model="selectcity"
          outline
        ></v-select>
     </v-flex>
    <v-flex xs>
        <v-autocomplete :disabled="toggle()"
        append-icon="search"
        :loading="loading"
        :items="branches"
        :search-input.sync="search"
        v-model="selectbranch"
        
        class="mx-3"
        flat
        hide-no-data
        hide-details
        label="What branch do you want?"
        
        solo outline
        ></v-autocomplete>
      </v-flex>
    </v-layout>
    <v-layout>
      <v-flex>
        <h1 v-if="!selectcity">Please select a your city.</h1>
        <h1 v-else-if="!search">Please wait! select/search for a branch </h1>
        <div v-else>
          <h1 v-if="!match">Loading details! Please wait.</h1>
          <bank :banks="match"></bank>
        </div>
      </v-flex>
    </v-layout>
    
</v-container>

</template>
<script>
import Bank from './banks/Bank.vue';
  export default {
    components:{
      'bank': Bank
    },
    data () {
      return {
        loading: false,
        banks:[],
        branches:[],
        search: '',
        selectcity: null,
        selectbranch: null,
        match: '',
        cities: [
            'Hyderabad',
            'Visakhapatnam',
            'Mumbai',
            'Chennai',
            'Patna',
            'Kolkata',
            'Bhubaneshwar',
            'Bangalore',
            'Allahabad'
            
        ]
      }
    },
    watch: {
      search (val) {
        if(val){
          this.filterBranches(val);
        }
        
      },
      selectcity(city) {
        if(city){
        this.getCities();
        console.log(this.banks);
        console.log(this.branches);
        }
      }
    },
    methods: {
      querySelections (v) {
        this.loading = true
        // Simulated ajax query
        setTimeout(() => {
          this.items = this.branches.filter(e => {
            return (e || '').toLowerCase().indexOf((v || '').toLowerCase()) > -1
          })
          this.loading = false
        }, 500)
      },
      getCities(){
        this.banks=[];
        this.branches=[];
        this.items=[];
        fetch(`https://vast-shore-74260.herokuapp.com/banks?city=${this.upperCase}`)
        .then(res=>res.json())
        .then(data=>{
          for(var bank in data){
            this.banks.push(data[bank])
            this.branches.push(data[bank].branch);
          }
          
        })
        // console.log(this.banks);
      },
      toggle(){
        if(this.banks.length<=0){
          return true;
        }
          else{
            return false;
          }
        
      },
      filterBranches(branchname){
          this.match=[];
          const result= this.banks.filter(bank=>{
          return bank.branch==branchname
        })
        if(result){
          console.log(result);
          this.match= result;
        }
      }
    },
    computed: {
      upperCase(){
        if(this.selectcity){
          return this.selectcity.toUpperCase()
        }
      }
    
    }
  }
</script>