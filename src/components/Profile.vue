<template>
  <v-container
    @mouseover="
      $gtag.event('hover', {
        event_label: channel.broadcaster_name,
        event_category: 'inspect_channel',
      })
    "
  >
    <v-sheet
      color="white"
      elevation="14"
      height="720"
      outlined
      rounded
      width="600"
    >
      <v-container>
        <v-lazy
          v-model="channel.isActive"
          :options="{
            threshold: 1,
          }"
          class="fill-height"
        >
          <v-card class="mx-auto" outlined>
            <v-toolbar
              :color="channel.configuration.panel_settings.background_colour"
              elevation="4"
              flat
              outlined
              rounded
            >
              <v-toolbar-title
                ><b
                  :style="
                    `color:` +
                    channel.configuration.panel_settings.font_colour +
                    ';'
                  "
                >
                  {{ channel.broadcaster_name }}
                </b>
              </v-toolbar-title>

              <v-avatar>
                <img :src="channel.profile" :alt="channel.broadcaster_id" />
              </v-avatar>
            </v-toolbar>

            <v-card-title
              >{{ channel.game_name }}
              <v-card-subtitle>
                <v-spacer></v-spacer>
                <v-spacer></v-spacer>
                Last Updated:
                <b>{{ channel.updated_date }}</b>
              </v-card-subtitle>
            </v-card-title>

            <v-card-text>
              <v-divider></v-divider>
              <v-container>
                <v-container
                  align="center"
                  justify="center"
                  v-if="!iframe.loaded"
                >
                  <v-progress-circular
                    :size="30"
                    :width="7"
                    color="purple"
                    indeterminate
                  ></v-progress-circular>
                </v-container>

                <v-container></v-container>

                <iframe
                  @load="load"
                  v-show="iframe.loaded"
                  v-bind:src="
                    `https://player.twitch.tv/?channel=` +
                    channel.broadcaster_name +
                    `&parent=jackmcguire1.github.io&autoplay=false`
                  "
                  @mouseover="
                    $gtag.event('hover', {
                      event_label: channel.broadcaster_name + '_video',
                      event_category: 'inspect_channel',
                    })
                  "
                  @click="
                    $gtag.event('click', {
                      event_label: channel.broadcaster_name + '_video',
                      event_category: 'inspect_channel',
                    })
                  "
                  allowfullscreen
                  height="300px"
                  width="500px"
                >
                </iframe>
              </v-container>
              <v-container align="center">
                <span
                  v-if="
                    !channel.configuration.motd ||
                    channel.configuration.motd.msg == ''
                  "
                >
                  {{ channel.title }}
                </span>
                <span v-else>{{ channel.configuration.motd.msg }}</span>
              </v-container>
              <v-divider></v-divider>
              <v-container> Profile Views: {{ channel.viewers }} </v-container>
              <v-divider></v-divider>
              <v-container>
                Installed Stat-Milestones:
                <b>{{ channel.created_date }}</b>
              </v-container>
            </v-card-text>
          </v-card>
        </v-lazy>
      </v-container>
    </v-sheet>
  </v-container>
</template>

<script>
export default {
  name: "Profile",
  data: () => ({
    iframe: {
      loaded: false,
    },
  }),
  props: {
    channel: {},
  },
  methods: {
    load: function () {
      this.iframe.loaded = true;
    },
  },
};
</script>
<style scoped></style>
