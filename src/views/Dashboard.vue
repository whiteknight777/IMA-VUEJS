<template>
  <v-container
    fill-height
    fluid
    grid-list-xl
  >
  
    <v-layout wrap>
      <v-flex
        sm6
        xs12
        md6
        lg3
      >
        <material-stats-card
          color="green"
          icon="mdi-store"
          title="Total congés acquis"
          small-value="Jr(s)"
          value="12"
          sub-icon="mdi-calendar"
          sub-text="Last 24 Hours"
        />
      </v-flex>
      <v-flex
        sm6
        xs12
        md6
        lg3
      >
        <material-stats-card
          color="orange"
          icon="mdi-content-copy"
          title="Total demandes"
          value="15"
          sub-icon="mdi-alert"
          sub-icon-color="error"
          sub-text="Get More Space..."
          sub-text-color="text-primary"
        />
      </v-flex>
      <v-flex
        sm6
        xs12
        md6
        lg3
      >
        <material-stats-card
          color="red"
          icon="mdi-information-outline"
          title="Total demandes refusées"
          value="4"
          sub-icon="mdi-tag"
          sub-text="Tracked from Github"
        />
      </v-flex>
      <v-flex
        sm6
        xs12
        md6
        lg3
      >
        <material-stats-card
          color="info"
          icon="mdi-twitter"
          title="Total demandes acceptées"
          value="+245"
          sub-icon="mdi-update"
          sub-text="Just Updated"
        />
      </v-flex>
      <v-flex
        md12
        sm12
        lg6
      >
      

        <material-chart-card
          :data="dataCompletedTasksChart.data"
          :options="dataCompletedTasksChart.options"
          :responsive-options="dataCompletedTasksChart.options.responsiveOptions"
          color="green"
          ratio="0"
          type="Line"
        >
          <h3 class="title font-weight-light">Graph des jours d'absences de l'année 2019</h3>
          <p class="category d-inline-flex font-weight-light">Récapitulatif des compteurs</p>
        
             <v-data-table
            :headers="statsHeaders"
            :items="statsItems"
            hide-actions
          > 
            <template
              v-slot:items="statsItems"
            >
              <td>{{ statsItems.item.name }}</td>
              <td class="text-xs-center nbr-color">{{ statsItems.item.solde }}</td>
              <td class="text-xs-center">{{ statsItems.item.jPoses }}</td>
              <td class="text-xs-center">{{ statsItems.item.soldeFinMois }}</td>
            </template>
          </v-data-table>



          <template slot="actions">
            <v-icon
              class="mr-2"
              small
            >
              mdi-clock-outline
            </v-icon>
            <span class="caption grey--text font-weight-light">dernière mise à jours : 01/06/2019</span>
          </template>
        </material-chart-card>
      </v-flex>
      <v-flex
        md12
        lg3
      >
        <material-card
          color="orange"
          title="Historique activités"
          text="3 dernières activités"
        >
          <v-list two-line>

          <v-list-tile avatar>
            <v-list-tile-content>
              <v-list-tile-title>BERGER FLORIAN a validé votre demande du 06/06/2019 </v-list-tile-title>
              <v-list-tile-sub-title>03/06/2019 13:55</v-list-tile-sub-title>
            </v-list-tile-content>
          </v-list-tile>

          <v-divider></v-divider>

          <v-list-tile avatar>
            <v-list-tile-content>
              <v-list-tile-title>Vous avez posé une demande d'absence le 06/06/2019</v-list-tile-title>
              <v-list-tile-sub-title>03/06/2019 12:53</v-list-tile-sub-title>
            </v-list-tile-content>
          </v-list-tile>

          <v-divider></v-divider>

          <v-list-tile avatar>
            <v-list-tile-content>
              <v-list-tile-title>0,34 jour crédité sur votre compte RTT Employeur 2019</v-list-tile-title>
              <v-list-tile-sub-title>31/05/2019 23:26</v-list-tile-sub-title>
            </v-list-tile-content>
          </v-list-tile>
        </v-list>

          <template slot="actions">
            <v-icon
              class="mr-2"
              small
            >
              mdi-clock-outline
            </v-icon>
            <span class="caption grey--text font-weight-light">dernière mise à jours : 01/06/2019</span>
          </template>

        </material-card>
      </v-flex>

      <v-flex
        md12
        lg3
      >
        <material-card
          color="default"
          title="Suivi demandes"
          text="3 dernières demandes"
        >
          <v-card
                class="mx-auto"
                max-width="400"
              >
                
                <v-card-text class="py-0" style="padding-bottom: 16px !important;">
                  <v-timeline
                    align-top
                    dense
                  >
                    <v-timeline-item
                      color="pink"
                      small
                    >
                      <v-layout pt-3>
                        <v-flex xs4>
                          <strong>status</strong>
                          <div class="caption">Refusé</div>
                        </v-flex>
                        <v-flex>
                          <span>Demande <strong>du 06/06/2019</strong></span>
                          <div class="caption"><strong>au 08/06/2019</strong></div>
                        </v-flex>
                      </v-layout>
                    </v-timeline-item>

                    <v-timeline-item
                      color="teal lighten-3"
                      small
                    >
                      <v-layout pt-3>
                         <v-flex xs4>
                          <strong>status</strong>
                          <div class="caption">Accepté</div>
                        </v-flex>
                        <v-flex>
                          <span>Demande <strong>du 06/06/2019</strong></span>
                          <div class="caption"><strong>au 08/06/2019</strong></div>
                        </v-flex>
                      </v-layout>
                    </v-timeline-item>

                    <v-timeline-item
                      color="orange"
                      small
                    >
                      <v-layout pt-3>
                         <v-flex xs4>
                          <strong>status</strong>
                          <div class="caption">En cours</div>
                        </v-flex>
                        <v-flex>
                          <span>Demande <strong>du 06/06/2019</strong></span>
                          <div class="caption"><strong>au 08/06/2019</strong></div>
                        </v-flex>
                      </v-layout>
                    </v-timeline-item>
                  </v-timeline>
                </v-card-text>
                
          </v-card>

        </material-card>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
export default {
  data () {
    return {
      dataCompletedTasksChart: {
        data: {
          labels: ['Jan', 'Fev', 'Mar', 'Avr', 'Mai', 'Juin', 'Jui', 'Aou', 'Sep', 'Oct', 'Nov', 'Dec'],
          series: [
            [230, 750, 450, 300, 280, 240, 200, 190, 225, 445, 241, 223]
          ]
        },
        options: {
          lineSmooth: this.$chartist.Interpolation.cardinal({
            tension: 0
          }),
          low: 0,
          high: 750, // creative tim: we recommend you to set the high sa the biggest value + something for a better look
          chartPadding: {
            top: 30,
            right: 10,
            bottom: 10,
            left: 10
          },
          responsiveOptions: [
          ['screen and (max-width: 640px)', {
            seriesBarDistance: 5,
            axisX: {
              labelInterpolationFnc: function (value) {
                return value[0]
              }
            }
          }]
        ]
        }
      },
      headers: [
        {
          sortable: false,
          text: 'ID',
          value: 'id'
        },
        {
          sortable: false,
          text: 'Name',
          value: 'name'
        },
        {
          sortable: false,
          text: 'Salary',
          value: 'salary',
          align: 'right'
        },
        {
          sortable: false,
          text: 'Country',
          value: 'country',
          align: 'right'
        },
        {
          sortable: false,
          text: 'City',
          value: 'city',
          align: 'right'
        }
      ],
      items: [
        {
          name: 'Dakota Rice',
          country: 'Niger',
          city: 'Oud-Tunrhout',
          salary: '$35,738'
        },
        {
          name: 'Minerva Hooper',
          country: 'Curaçao',
          city: 'Sinaai-Waas',
          salary: '$23,738'
        }, {
          name: 'Sage Rodriguez',
          country: 'Netherlands',
          city: 'Overland Park',
          salary: '$56,142'
        }, {
          name: 'Philip Chanley',
          country: 'Korea, South',
          city: 'Gloucester',
          salary: '$38,735'
        }, {
          name: 'Doris Greene',
          country: 'Malawi',
          city: 'Feldkirchen in Kārnten',
          salary: '$63,542'
        }
      ],
      statsHeaders: [
        {
          sortable: false,
          text: '',
          value: 'name'
        },
        {
          sortable: false,
          text: 'Solde actuel',
          value: 'solde'
        },
        {
          sortable: false,
          text: 'Jours posés',
          value: 'jPoses',
          align: 'center'
        },
        {
          sortable: false,
          text: 'Solde estimé à fin du mois (Juin)',
          value: 'soldeFinMois',
          align: 'center'
        }
      ],
      statsItems: [
        {
          name: 'Congés payés 2018/2019',
          solde: '5',
          jPoses: '10',
          soldeFinMois: '7.2'
        },
        {
          name: 'Permission(s) exceptionnelle(s)',
          solde: '6',
          jPoses: '4',
          soldeFinMois: '6'
        }
      ],
    }
  },
  methods: {
    complete (index) {
      this.list[index] = !this.list[index]
    }
  }
}
</script>
<style>
  .v-card--material-chart.v-card .v-card__text {
    padding-bottom: 2px !important;
}
.v-list__tile__title {
    font-size: 0.9rem;
}
.v-timeline-item__dot--small .v-timeline-item__inner-dot {
    height: 25px;
    margin: 0px;
    width: 25px;
}
.v-timeline-item {
    padding-bottom: 22px;
}
span{
    font-size: 12px;
    font-weight: 300;
}
</style>


