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
          <v-data-table
            :headers="nbJrs"
            :items="months"
            hide-actions
          >
           <template v-slot:headers="nbJrs">
            <tr>
              <th></th>
            </tr>
            <tr>
              <th
                v-for="(jr, i) in nbJrs"
                :key="'nbjour-'+i"
              >
                {{ jr }}
              </th>
            </tr>
          </template>
            
          <template v-slot:items="months">
            <tr
            v-for="month in months"
            :key="+'-nbmonth'"
            >
              <td>
                {{month.name}}
              </td>
              <td
                v-for="(k, n) in month.nbJrs"
                :key="n+'-jm'"
              >
                {{ getCurrentDay(n, (months.indexOf(month)+1)) }}
              </td>
              
            </tr> 
          </template>

          </v-data-table>
        </material-card>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
export default {
  data: () => ({
    nbJrs: [],
    months: [
      {
        name: 'Janvier',
        year: '2019',
        nbjrs: 31,
        feries: ['10/01/2019', '20/01/2019']
      },
      {
        name: 'Fevrier',
        year: '2019',
        nbjrs: 28,
        feries: ['10/01/2019', '20/01/2019']
      },
      {
        name: 'Mars',
        year: '2019',
        nbjrs: 31,
        feries: ['10/01/2019', '20/01/2019']
      },
      {
        name: 'Avril',
        year: '2019',
        nbjrs: 30,
        feries: ['10/01/2019', '20/01/2019']
      },
    ],
    days: ["D", "L", "M", "Me", "J", "V", "S"]
  }),
  methods: {
    getNbJrs: function(){
      let tab = []
      for(let i=1; i == 31; i++){
        tab.push(i)
    console.log(i)
      }
      return tab
    },
    getCurrentDay: function(day, month){
      let year = (new Date()).getFullYear()
      let d = new Date(month+'/'+day+'/'+year)
      return days[d.getDay()]
    }
  },
  // computed: {
  //   nbJrs: function() {
  //     return this.getNbJrs()
  //   }
  // },
  mounted: function (){
    console.log('test')
    this.getNbJrs()
  }
}
</script>
