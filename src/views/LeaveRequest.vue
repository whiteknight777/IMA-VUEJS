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
          :title="'Choisissez vos jours de congés [ du '+firstDate.date+' au '+lastDate.date+' ] - '+nbDaySelected+' jour(s)'"
          text="Calendrier annuel des absences"
        >
          <table class="v-datatable v-table theme--light">
            <thead>
              <th>
                <v-btn depressed small color="info" @click="moveToUp()" :disabled="minMonth == 0"><v-icon>mdi-arrow-up-drop-circle-outline</v-icon></v-btn>
              </th>
              <th 
              v-for="jr in nbJrs"
              :key="jr.text+'days'"
              class="column text-xs-center"
              :class="{
                hover: jr.hover
              }"
              >
              <span 
              class="subheading font-weight-light text--darken-3"
              >{{ jr.text }}</span>
              </th>
            </thead>
            <tbody>
              <tr 
              v-for="item in daysOfTheYear"
              :key="item.month+'-mth'"
              class="column text-xs-left"
              >
                <td 
                :class="{
                hover : item.hover
                }"
                >{{ item.month }}  </td>
                <td
                v-for="subItem in item.days"
                :key="subItem.id+'-day-month'"
                class="text-xs-center day-leave"
                :class="{
                  week_end : subItem.isWeekEnd,
                  day_passed : subItem.passed,
                  feries : subItem.isFerie,
                  today : subItem.today,
                  selected : subItem.selected,
                  leave : subItem.leave
                }"
                @click="selecteDate(item.id, subItem.id)"
                @mouseover="dayInfos(item.month, subItem.id)"
                :title="hoverDate"
                >
                  {{ subItem.day }}
                </td>
              </tr>
            </tbody>
            <tfoot>
              <th>
                <v-btn depressed small color="info" @click="moveToDown()" :disabled="maxMonth == 12"><v-icon>mdi-arrow-down-drop-circle-outline</v-icon></v-btn>
              </th>
            </tfoot>
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
    daysOfTheYear:[],
    daysOfTheMonth:[],
    minMonth: 0,
    maxMonth: 7,
    nbClick: 0,
    firstDate:{date:''},
    lastDate:{date:''},
    daySelected: [],
    hoverDate: "",
    leaves:[
      "11/4/2019", "12/4/2019", "15/4/2019", "16/4/2019"
    ],
    months: [
      {
        name: 'Janvier',
        id: 1,
        year: '2019',
        jrs: 31,
        feries: ['07/01/2019', '11/01/2019', '24/01/2019']
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
        feries: ['02/05/2019', '20/05/2019']
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
      let tab = []
      for(let i=1;i <32; i++){
        tab.push({
          text: i,
          hover: false
          })
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
      if(month < 10 ){
        month = '0'+month
      }
      if(day < 10 ){
        day = '0'+day
      }
      if(!_r){
        feries.find(f => {
            if(f == day+'/'+month+'/'+this.year){
              result = true
            }
        })
      }
      return result
    },
    getDaysOfTheYear() {
      this.daysOfTheYear = []
      let months = this.months;
      let id = 0
      let today = new Date()
      let tdy_mnth = (today.getMonth()<10) ? (today.getMonth()+1) : today.getMonth()
      let tdy_day = (today.getDate()<10) ? today.getDate() : today.getDate()
      let current_date = tdy_day+'/'+(tdy_mnth)+'/'+today.getFullYear()
      let tms_current_date = (new Date(tdy_mnth+'/'+tdy_day+'/'+this.year)).getTime()

      // console.log(tdy_day+'/'+tdy_mnth+'/'+today.getFullYear())
      for(let q=this.minMonth; q < this.maxMonth; q++){
        let current_month = months[q].id
        let days = []
        for(let d=1; d < (months[q].jrs + 1); d++){
          // Récupération des infos du jours 
          days.push({
            id: d,
            day: this.getCurrentDay(d, current_month),
            date: d+'/'+current_month+'/'+this.year,
            isWeekEnd: false,
            selected: false,
            nbClick: 0,
            timestamp: this.getTimestampDate(d+'/'+current_month+'/'+this.year),
            passed: false,
            today: false,
            currentMonth: current_month,
            leave: false,
            isFerie: false
          })
        }

        // marquons les congés déjà existant
        for(let l=0; l < this.leaves.length; l++){
          // Vérification et mise à jours des congés
          days.map(e =>{
            let date = this.leaves[l]
            if(e.date == date) e.leave = true
          })
        }

        // Vérification et mise à jours des fériés
        days.map(e =>{
          let date = e.date.split('/')
          let resultFerie = this.checkFeries(date[0], date[1], months[q].feries)
          let resultWeekEnd = this.checkWeekEnd(date[0], date[1])
          if(e.date == current_date) e.today = true
          if(e.timestamp < tms_current_date) e.passed = true
          if(resultWeekEnd) e.isWeekEnd = true;
          if(resultFerie) e.isFerie = true;
        })

        // Vérifions si des jours ont été selectionnez
        days.map(e =>{
          if(e.date == this.firstDate.date || e.date == this.lastDate.date && e.nbClick < 2) e.selected = true //
          // if(e.date >= this.firstDate.date || e.date <= this.lastDate.date && e.nbClick < 2) e.selected = true
          if(this.firstDate.date != '' && this.lastDate.date != ''){

            // ### 1/ Vérifions si la période choisie est dans le même mois
            if(this.firstDate.currentMonth == this.lastDate.currentMonth){
              // Activons les jours appartenant au mois en question
              if(e.currentMonth == this.firstDate.currentMonth){
                // Récupérons les jours compris entre les dates selectionnez
                  if(e.timestamp > this.firstDate.timestamp && e.timestamp < this.lastDate.timestamp && e.nbClick < 2) {
                      if(e.isFerie == false && e.isWeekEnd == false){
                          // sélectionnons uniquement les jours ouvrés
                        e.selected = true
                      }
                  }
              }
            }

            // période contenu dans différents mois
            if(this.firstDate.currentMonth != this.lastDate.currentMonth){
                // Vérifions que les jours sont bien compris dans les différents mois
                if(e.currentMonth >= this.firstDate.currentMonth && e.currentMonth <= this.lastDate.currentMonth){
                  // Vérifions que les jours sont bien compris entre les différents date sélectionnées
                    if((e.timestamp > this.firstDate.timestamp) && (e.timestamp < this.lastDate.timestamp)){
                        // sélectionnons uniquement les jours ouvrés
                        if(e.isFerie == false && e.isWeekEnd == false){
                          e.selected = true
                        }
                    }
                }
            }
            
          }
        })

        // Récupérations des jours du mois
        this.daysOfTheYear.push({
            id: id,
            number: months[q].id,
            days: days,
            hover: false,
            month: months[q].name,
        })

        id++
      }
      
    },
     moveToUp: function (){
      if(this.minMonth > 0) {
        this.minMonth = this.minMonth - 1
        this.maxMonth = this.maxMonth - 1
        this.getDaysOfTheYear()
        this.nbJrs = []
        this.nbJrs = this.getNbJrs()
      }
    },
    moveToDown: function (){
      if(this.maxMonth < 12) {
        this.minMonth = this.minMonth + 1
        this.maxMonth = this.maxMonth + 1
        this.getDaysOfTheYear()
        this.nbJrs = []
        this.nbJrs = this.getNbJrs()
      }
    },
    selecteDate: function (idElm, idDay){
      if(this.nbClick == 2) {
        this.getDaysOfTheYear()
      }
      this.daysOfTheYear.find(d => {
        if(d.id == idElm){
          d.days.find(e =>{
            switch(this.nbClick){
              case (0): 
                if(e.id == idDay && e.isWeekEnd == false && e.isFerie == false && e.leave == false && e.passed == false){
                  e.nbClick++
                  e.selected = true
                  this.firstDate = e
                  this.nbClick++
                  break;
                }
              case (1): 
                if(e.id == idDay && e.isWeekEnd == false && e.isFerie == false && e.leave == false && e.passed == false){
                    // e.selected = !e.selected
                    e.nbClick++
                    this.lastDate = e
                    if(e.nbClick == 2){
                      console.log('prendre ta journée ??')
                      console.log(this.lastDate)
                      this.nbClick++
                    }
                    if(this.firstDate.timestamp > this.lastDate.timestamp){
                      console.log('Coquin')
                      this.firstDate = {date:''}
                      this.lastDate = {date:''}
                      this.getDaysOfTheYear()
                      this.nbClick = 0
                      this.daySelected = []
                    }
                    if((this.firstDate.date != this.lastDate.date) && e.nbClick != 2){
                      e.selected = true
                      // this.daySelected.push(e)
                      this.nbClick++
                      this.selectedLeavesDays()

                    }
                    break;
                }
              case (2): 
                if(e.id == idDay && e.isWeekEnd == false && e.isFerie == false && e.leave == false && e.passed == false){
                  //  3ième jours selectionné
                  this.nbClick++
                  // réinitialisation des jours sélectionnez ( 3 jours différents )
                  if(this.firstDate.date !== this.lastDate.date){
                    this.nbClick = 0
                    this.firstDate = {date:''}
                    this.lastDate = {date:''}
                    this.getDaysOfTheYear()
                    this.daySelected = []
                  }
                  // réinitialisation des jours sélectionnez
                  if((this.firstDate.date == this.lastDate.date) || (this.firstDate.date != this.lastDate.date) && this.nbClick < 3){
                    this.nbClick = 0
                    this.firstDate = {date:''}
                    this.lastDate = {date:''}
                    this.getDaysOfTheYear()
                    this.daySelected = []
                  }
                    
                  break;
                }
            }
          })
        }
      })
    },
    dayInfos: function (month, idDay) {
      this.hoverDate = idDay+' '+month+' '+this.year
      let days = this.nbJrs
      this.daysOfTheYear.find(e => {
        if(e.month == month) {
          e.hover = true
        }else{
          e.hover = false
        }
      })
      days.find(e => {
        if(e.text == idDay){
          e.hover = true
        }else{
          e.hover = false
        }
      })
      // console.log(month)
      // console.log(idDay)
    },
    getTimestampDate: function (date){
      let _d = date.split('/')
      var date = new Date(_d[1]+'/'+_d[0]+'/'+_d[2]); 
      return date.getTime();
    },
    selectedLeavesDays: function (){
      // Recuperer le mois du premier et du dernier jour
      let frs_month = this.firstDate.currentMonth
      let lst_month = this.lastDate.currentMonth
      // période contenu dans le même mois
      if (frs_month === lst_month){
          // Les jours selectionnez sont dans le même mois
          // récupérons les jours du mois
          let month_info = this.daysOfTheYear.find(e => {
              if((e.number) == frs_month) return e
          })
          let days = month_info.days
          days.find(e => {
            if(e.currentMonth == frs_month){
                if((e.timestamp >= this.firstDate.timestamp) && (e.timestamp <= this.lastDate.timestamp)){
                    // selectionnons et récupérons les jours
                    if(e.isFerie == false && e.isWeekEnd == false){
                      // sélectionnons uniquement les jours ouvrés
                      e.selected = true
                      if(e.nbClick == 0) e.nbClick++
                      this.daySelected.push(e)
                    }
                }
            }
          })
      }
      
      // période contenu dans différents mois
      if(frs_month != lst_month){
          // Les jours selectionnez sont dans des mois différents
          // récupérons les mois concernés
          let months_info = [] 
          this.daysOfTheYear.find(e => {
            if((e.number) >= frs_month && (e.number) <= lst_month) {
                months_info.push(e)
            }
          })
          console.log(months_info)
          // Parcourons les différents mois
          for(let q = 0 ; q < months_info.length; q++){
              // On recupère les jours du mois
              let days = months_info[q].days
              days.find(e => {
                // Vérifions que les jours sont bien compris dans les différents mois
              if(e.currentMonth >= frs_month && e.currentMonth <= lst_month){
                // Vérifions que les jours sont bien compris entre les différents date sélectionnées
                  if((e.timestamp >= this.firstDate.timestamp) && (e.timestamp <= this.lastDate.timestamp)){
                      // sélectionnons uniquement les jours ouvrés
                      if(e.isFerie == false && e.isWeekEnd == false){
                        e.selected = true
                        if(e.nbClick == 0) e.nbClick++
                        this.daySelected.push(e)
                      }
                  }
              }
            })
          }
      }
      
    }
  },
  computed: {
    nbDaySelected: function (){
      return this.daySelected.length
    }
  },
  mounted: function (){
    this.nbJrs = this.getNbJrs()
    this.getDaysOfTheYear()
  }
}
</script>
<style>
.week_end{
  background: #23bace;
  color: white;
}
.feries{
  background: #fd6155;
  color: white;
}
.hover{
  background: #7036ce;
  color: white !important;
}
.day_passed{
  background: rgba(134, 167, 199, 0.14);
  color: #adadad;
}
.today{
  background: gold;
  color: white;
}
.leave{
  background: #a2c72e;
  color: white;
}
table.v-table thead th:first-child, table.v-table thead th:not(:first-child){
    padding: 0px 12px;
}
table.v-table tfoot th:first-child{
    padding: 12px 8px;
} 
.day-leave{
  cursor: pointer;
}
.day-leave.selected{
  background: #ff9800;
  color: white;
}
td.day-leave.week_end.day_passed{
  background: #23bace70;
  color: #f7f7f7;
}
td.day-leave.day_passed.feries{
  background: #ff9991;
  color: #e2e2e2;
}
td.day-leave.day_passed.leave{
  background: #a2c72e;
  color: #e2e2e2;
}
</style>

