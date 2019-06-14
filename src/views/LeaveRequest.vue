<template>
  <v-container
    fill-height
    fluid
    grid-list-xl
  >
    <v-layout
      justify-center
      wrap
    >
      <v-flex
        md12
      >
        <material-card
          color="green"
          title="Simple Table"
          text="Here is a subtitle for this table"
        >
          <!-- <v-data-table
            :headers="nbJrs"
            :items="months"
          >
           <template v-slot:headers="props">
            <tr>
              <th>
                {{ props.text }}
              </th>
            </tr>
          </template>
            
          <template v-slot:items="props">
            <tr>
              <td>
                {{props.item.name}}
              </td>
              <td
                v-for="(k, n) in props.item.nbJrs"
                :key="n+'-jm'"
              >
                {{ getCurrentDay(n, (months.indexOf(month)+1)) }}
              </td>
              
            </tr> 
          </template>

          </v-data-table> -->

          <table class="v-datatable v-table theme--light">
            <thead>
              <th></th>
              <th 
              class="column text-xs-center"
              v-for="n in 31"
              :key="n+'days'"
              >
              {{ n }}
              </th>
            </thead>
            <tbody>
              <tr 
              v-for="month in months"
              :key="month.id+'-month'"
              class="column text-xs-left"
              >
                <td>{{ month.name }}</td>
                <td
                v-for="day in month.jrs"
                :key="day+'-day-month'"
                class="text-xs-center"
                :class="{
                  week_end : checkWeekEnd(day, (months.indexOf(month)+1)),
                  feries : checkFeries(day, (months.indexOf(month)+1), month.feries)
                }"
                >
                  {{ getCurrentDay(day, (months.indexOf(month)+1)) }}
                </td>
              </tr>
            </tbody>
          </table>
        </material-card>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
export default {
  data: () => ({
    nbJrs: [],
    year: (new Date()).getFullYear(),
    isWeekEnd: false,
    months: [
      {
        name: 'Janvier',
        id: 1,
        year: '2019',
        jrs: 31,
        feries: ['07/01/2019']
      },
      {
        name: 'Fevrier',
        id: 2,
        year: '2019',
        jrs: 28,
        feries: []
      },
      {
        name: 'Mars',
        id: 3,
        year: '2019',
        jrs: 31,
        feries: []
      },
      {
        name: 'Avril',
        id: 4,
        year: '2019',
        jrs: 30,
        feries: []
      },
      {
        name: 'Mai',
        id: 5,
        year: '2019',
        jrs: 31,
        feries: ['10/05/2019', '20/05/2019']
      },
      {
        name: 'Juin',
        id: 6,
        year: '2019',
        jrs: 30,
        feries: []
      },
      {
        name: 'Juillet',
        id: 7,
        year: '2019',
        jrs: 31,
        feries: ['18/07/2019']
      },
      {
        name: 'Août',
        id: 8,
        year: '2019',
        jrs: 31,
        feries: []
      },
      {
        name: 'Septembre',
        id: 9,
        year: '2019',
        jrs: 30,
        feries: []
      },
      {
        name: 'Octobre',
        id: 10,
        year: '2019',
        jrs: 31,
        feries: []
      },
      {
        name: 'Novembre',
        id: 11,
        year: '2019',
        jrs: 30,
        feries: []
      },
      {
        name: 'Decembre',
        id: 12,
        year: '2019',
        jrs: 31,
        feries: []
      }
    ],
    days: ["D", "L", "M", "Me", "J", "V", "S"]
  }),
  methods: {
    getNbJrs: function(){
      let tab = [{text:''}]
      for(let i=1;i <32; i++){
        tab.push({text: i})
      }
      return tab
    },
    getCurrentDay: function(day, month){
      let d = new Date(month+'/'+day+'/'+this.year)

      return this.days[d.getDay()]
    },
    checkWeekEnd: function(day, month){
      let d = new Date(month+'/'+day+'/'+this.year)
      let _day = this.days[d.getDay()]
      if(_day === "S" || _day === "D" ){
        return true
      }else {
        return false
      }
    },
    checkFeries: function(day, month, feries){
      let _r = this.checkWeekEnd(day, month)
      let result = false
      if(!_r){
        feries.forEach(f =>{
          if(month < 10){
            console.log(month)
            if(f == day+'/0'+month+'/'+this.year){
              result = true
              // console.log(f)
            }else{
              result = false
            }
          }else{
            if(f == day+'/'+month+'/'+this.year){
              result = true
              // console.log(f)
            }else{
              result = false
            }
          }
        })
      }
      return result
    }
  },
  mounted: function (){
    this.nbJrs = this.getNbJrs()
  }
}
</script>
<style>
.week_end{
  background: #23bace;
  color: white;
}
.feries{
  background: #7036ce;
  color: white;
}
</style>

