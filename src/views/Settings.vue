<template>
  <v-container
    fluid
    grid-list-xl
    fill-height>
    <v-layout
      justify-center
      align-center
    >
      <v-flex xs12>
        <material-card
          color="green">
          <div
            slot="header"
          >
            <div class="title font-weight-light mb-2">Updates your settings</div>
            <div class="category">
              Changes yours configs
              <v-icon
                size="17"
              >
                mdi-settings
              </v-icon>
            </div>
          </div>

            <v-container grid-list-xl>
              <v-layout row>
                <v-flex lg12>
                <v-list two-line subheader >
                  <v-subheader>General</v-subheader>

                  <v-list-tile avatar>
                    <v-list-tile-content>
                      <v-list-tile-title>Profile photo</v-list-tile-title>
                      <v-list-tile-sub-title>Change your Google+ profile photo</v-list-tile-sub-title>
                    </v-list-tile-content>
                  </v-list-tile>

                  <v-list-tile avatar>
                    <v-list-tile-content>
                      <v-list-tile-title>Show your status</v-list-tile-title>
                      <v-list-tile-sub-title>Your status is visible to everyone</v-list-tile-sub-title>
                    </v-list-tile-content>
                  </v-list-tile>
                </v-list>

                <v-divider></v-divider>

                <v-list
                  subheader
                  two-line
                >
                  <v-subheader>Hangout notifications</v-subheader>

                  <v-list-tile @click="">
                    <v-list-tile-action>
                      <v-checkbox v-model="emailNotifications"></v-checkbox>
                    </v-list-tile-action>

                    <v-list-tile-content @click="emailNotifications = !emailNotifications">
                      <v-list-tile-title>Email Notifications</v-list-tile-title>
                      <v-list-tile-sub-title>Allow notifications</v-list-tile-sub-title>
                    </v-list-tile-content>
                  </v-list-tile>

                  <v-list-tile @click="">
                    <v-list-tile-action>
                      <v-checkbox v-model="sound"></v-checkbox>
                    </v-list-tile-action>

                    <v-list-tile-content @click="sound = !sound">
                      <v-list-tile-title>Sound</v-list-tile-title>
                      <v-list-tile-sub-title>Hangouts message</v-list-tile-sub-title>
                    </v-list-tile-content>
                  </v-list-tile>

                  <v-list-tile @click="">
                    <v-list-tile-action>
                      <v-checkbox v-model="video"></v-checkbox>
                    </v-list-tile-action>

                    <v-list-tile-content @click="video = !video">
                      <v-list-tile-title>Video sounds</v-list-tile-title>
                      <v-list-tile-sub-title>Hangouts video call</v-list-tile-sub-title>
                    </v-list-tile-content>
                  </v-list-tile>

                  <v-list-tile @click="">
                    <v-list-tile-action>
                      <v-checkbox v-model="invites"></v-checkbox>
                    </v-list-tile-action>

                    <v-list-tile-content @click="invites = !invites">
                      <v-list-tile-title>Invites</v-list-tile-title>
                      <v-list-tile-sub-title>Notify when receiving invites</v-list-tile-sub-title>
                    </v-list-tile-content>
                  </v-list-tile>
                </v-list>

                <v-divider></v-divider>
                
                <v-list
                  subheader
                  two-line
                >
                  <v-subheader>Sidebar Filters</v-subheader>
                  
                  <v-flex xs12>
                    <!-- <div class="body-2 text-uppercase sidebar-filter">Sidebar Filters</div> -->

                    <v-layout justify-start style="padding-left: 20px;">
                      <v-avatar
                        v-for="c in colors"
                        :key="c"
                        :class="[c === color ? 'color-active color-' + c: 'color-' + c]"
                        size="23"

                        @click="setColor(c)"
                      />
                    </v-layout>
                  </v-flex>
                </v-list>

                  <v-divider class="mt-3"/>

                  <v-list
                    subheader
                    two-line
                  >
                  <v-subheader>Sidebar Background</v-subheader>
                  <v-flex
                    xs12
                  >
                  <v-layout align-space-around justify-start row fill-height style="padding: 20px;">
                      <v-img
                        v-for="img in images"
                        :key="img"
                        xs3
                        :class="[image === img ? 'image-active' : '']"
                        :src="img"
                        height="120"
                        @click.native="setImage(img)"
                      />
                    </v-layout>
                  </v-flex>
                </v-list>
                  
                </v-flex>
                
              </v-layout>
            </v-container>

        </material-card>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
import {
  mapMutations,
  mapState
} from 'vuex'

export default {
  data: () => ({
    emailNotifications:false,
    sound:false,
    video:false,
    invites:false,
    colors: [
      'primary',
      'info',
      'success',
      'warning',
      'danger'
    ],
    images: [
      'https://demos.creative-tim.com/vue-material-dashboard/img/sidebar-1.23832d31.jpg',
      'https://demos.creative-tim.com/vue-material-dashboard/img/sidebar-2.32103624.jpg',
      'https://demos.creative-tim.com/vue-material-dashboard/img/sidebar-3.3a54f533.jpg',
      'https://demos.creative-tim.com/vue-material-dashboard/img/sidebar-4.3b7e38ed.jpg'
    ]
  }),

  computed: {
    ...mapState('app', ['image', 'color']),
    color () {
      return this.$store.state.app.color
    }
  },

  methods: {
    ...mapMutations('app', ['setImage']),
    setColor (color) {
      this.$store.state.app.color = color
    }
  }
}
</script>
<style lang="scss">
  .v-avatar,
  .v-responsive {
   cursor: pointer;
  }
  
  .v-card__text .v-avatar.color-primary {
    background-color: #9c27b0;
  }
  
  .v-card__text .v-avatar.color-info {
    background-color: #00bcd4;
  }
  .v-card__text .v-avatar.color-success {
    background-color: #4caf50;
  }
  .v-card__text .v-avatar.color-warning {
    background-color: #ff9800;
  }
  .v-card__text .v-avatar.color-danger {
    background-color: #f44336;
  }
  .v-card__text .v-avatar.image-active, .v-card__text .v-avatar.color-active, .v-card__text .v-responsive.image-active, .v-card__text .v-responsive.color-active {
    border: 2px solid #00bcd4;
  }
  .v-card__text .v-avatar:not(:last-child), .v-card__text .v-responsive:not(:last-child) {
    margin-right: 5px;
  }
</style>