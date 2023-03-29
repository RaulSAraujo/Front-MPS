<template>
  <v-dialog
    v-model="dialog"
    scrollable
    height="70vh"
    width="80vw"
    scrim="black"
  >
    <v-card rounded="xl" border="sm" color="surface">
      <v-card-title class="bg-surface text-h5 py-4">
        <v-row no-gutters>
          <span class="text-h5">Temporadas/Episodios</span>
          <v-spacer></v-spacer>
          <v-btn
            icon="mdi-close"
            variant="plain"
            @click="dialog = false"
          ></v-btn>
        </v-row>
      </v-card-title>
      <v-card-text>
        <v-item-group v-model="selection">
          <v-row>
            <v-col v-for="(season, i) in seasons" :key="i" cols="12">
              <v-item v-slot="{ isSelected, toggle }">
                <!-- <v-banner
                  class="my-4"
                  rounded="xl"
                  elevation="8"
                  border="sm"
                  style="background: rgb(0, 0, 0, 0.3)"
                >
                  <template v-slot:prepend>
                    <v-avatar size="130">
                      <v-img
                        v-if="season.poster_path"
                        :src="`https://image.tmdb.org/t/p/w200${season.poster_path}`"
                        :alt="season.name"
                        cover
                      ></v-img>
                      <v-icon v-else icon="mdi-cancel" size="80"></v-icon>
                    </v-avatar>
                  </template>

                  <v-banner-text>
                    <p class="text-h6">{{ season.name }}</p>
                    <p class="text-caption-2">
                      {{
                        season.air_date != "" ? formatDate(season.air_date) : ""
                      }}
                    </p>
                    <span class="text-body-1">{{ season.overview }}</span>
                  </v-banner-text>

                  <template v-slot:actions>
                   
                  </template>
                </v-banner> -->

                <v-card
                  elevation="5"
                  border="sm"
                  rounded="xl"
                  height="250"
                  class="ma-3"
                >
                  <v-img
                    v-if="season.poster_path"
                    :src="`https://image.tmdb.org/t/p/original${season.poster_path}`"
                    :alt="season.name"
                    gradient="to bottom, rgba(0,0,0,.9), rgba(0,0,0,.5)"
                    cover
                  >
                    <v-card-title>{{ season.name }}</v-card-title>
                    <v-card-subtitle class="mt-n1">
                      {{
                        season.air_date != "" ? formatDate(season.air_date) : ""
                      }}
                    </v-card-subtitle>
                    <v-card-text>
                      <p
                        style="
                          display: -webkit-box;
                          max-width: 100vw;
                          -webkit-line-clamp: 5;
                          -webkit-box-orient: vertical;
                          overflow: hidden;
                        "
                        class="text-body-2"
                      >
                        {{ season.overview }}
                      </p>
                    </v-card-text>
                  </v-img>
                  <v-row class="mt-n12 mr-2" no-gutters justify="end">
                    <v-btn
                      variant="plain"
                      :icon="isSelected ? 'mdi-chevron-up' : 'mdi-chevron-down'"
                      @click="toggle"
                    ></v-btn>
                  </v-row>
                </v-card>

                <v-expand-transition>
                  <div v-show="isSelected">
                    <v-slide-group
                      v-model="model"
                      class="pa-4"
                      selected-class="bg-success"
                      show-arrows
                    >
                      <v-slide-group-item
                        v-for="episodio in episodios"
                        :key="episodio"
                      >
                        <v-card
                          elevation="5"
                          width="410"
                          border="sm"
                          rounded="xl"
                          class="ma-3"
                        >
                          <v-img
                            :src="`https://image.tmdb.org/t/p/original${episodio.still_path}`"
                            gradient="to bottom, rgba(0,0,0,.3), rgba(0,0,0,.7)"
                            cover
                          >
                            <v-card-title>
                              <v-row
                                no-gutters
                                justify="space-between"
                                align="center"
                              >
                                <v-col cols="10" sm="10" md="11" lg="11">
                                  <div class="text-h5 text-truncate">
                                    <v-avatar
                                      color="white"
                                      variant="outlined"
                                      class="text-body-2 mr-2 mt-n1"
                                      size="x-small"
                                    >
                                      {{ episodio.episode_number }}
                                    </v-avatar>
                                    <span>{{ episodio.name }}</span>
                                  </div>
                                </v-col>
                                <v-col cols="2" sm="1" md="1" lg="1">
                                  <v-avatar
                                    color="white"
                                    variant="outlined"
                                    class="text-body-2"
                                    size="small"
                                  >
                                    {{ formatAverage(episodio.vote_average) }}
                                  </v-avatar>
                                </v-col>
                              </v-row>
                            </v-card-title>
                            <v-card-subtitle class="mt-n1">
                              <v-icon icon="mdi-calendar-month" flat></v-icon>
                              <span>
                                {{ formatDate(episodio.air_date) }}
                              </span>
                              <v-icon
                                icon="mdi-timer-sand"
                                class="ml-1"
                                flat
                              ></v-icon>

                              <span
                                >{{
                                  episodio.runtime
                                    ? formatRuntime(episodio.runtime)
                                    : ""
                                }}m</span
                              >
                            </v-card-subtitle>
                            <v-card-text>{{ episodio.overview }}</v-card-text>
                          </v-img>
                          <v-row class="mt-n9" no-gutters justify="end">
                            <v-btn
                              variant="plain"
                              @click="
                                getEpisodiosCredits(episodio.episode_number)
                              "
                              >Participantes</v-btn
                            >
                          </v-row>
                        </v-card>
                      </v-slide-group-item>
                    </v-slide-group>
                  </div>
                </v-expand-transition>
              </v-item>
            </v-col>
          </v-row>
        </v-item-group>
      </v-card-text>
    </v-card>

    <v-dialog
      v-model="dialogCredit"
      scrollable
      height="90vh"
      width="40vw"
      scrim="black"
    >
      <v-card rounded="xl">
        <v-card-title>
          <v-row no-gutters>
            <span class="text-h5">Participantes</span>
            <v-spacer></v-spacer>
            <v-icon
              icon="mdi-close"
              size="small"
              color="grey"
              @click="dialogCredit = false"
            ></v-icon>
          </v-row>
        </v-card-title>
        <v-card-text>
          <v-row>
            <v-col
              v-for="guestStar in guestStars.cast"
              :key="guestStar"
              cols="6"
            >
              <v-card border="sm" rounded="xl">
                <v-img
                  :src="`https://image.tmdb.org/t/p/original${guestStar.profile_path}`"
                  height="250"
                  cover
                >
                </v-img>
                <v-card-title>{{ guestStar.name }}</v-card-title>
                <v-card-subtitle class="mt-n2 mb-4">{{
                  guestStar.character
                }}</v-card-subtitle>
              </v-card>
            </v-col>
          </v-row>
        </v-card-text>
      </v-card>
    </v-dialog>
  </v-dialog>
</template>

<script>
import axios from "axios";
export default {
  props: {
    serieId: Number,
    seasons: Object,
  },
  data() {
    return {
      dialog: false,
      episodios: [],
      selection: undefined,
      model: undefined,
      dialogCredit: false,
      guestStars: [],
    };
  },
  watch: {
    dialog(val) {
      if (val == false) {
        this.selection = undefined;
      }
    },
    selection(val) {
      if (val != undefined) {
        this.getEpisodios();
      }
    },
  },
  methods: {
    getEpisodios() {
      axios
        .get(
          `https://api.themoviedb.org/3/tv/${this.serieId}/season/${
            this.seasons[this.selection].season_number
          }?api_key=9f9a623c8918bc56839f26a94b5507aa&language=pt-BR`
        )
        .then((response) => {
          console.log("Episodios", response);
          this.episodios = response.data.episodes;
        })
        .catch((error) => {
          console.log(error);
        });
    },
    getEpisodiosCredits(epsodeNumber) {
      axios
        .get(
          `https://api.themoviedb.org/3/tv/${this.serieId}/season/${
            this.seasons[this.selection].season_number
          }/episode/${epsodeNumber}/credits?api_key=9f9a623c8918bc56839f26a94b5507aa&language=pt-BR`
        )
        .then((response) => {
          console.log("Credit", response);
          this.guestStars = response.data;
          this.dialogCredit = true;
        })
        .catch((error) => {
          console.log(error);
        });
    },

    formatDate(dateString) {
      const date = dateString != undefined ? new Date(dateString) : "";
      return new Intl.DateTimeFormat("default", { dateStyle: "long" }).format(
        date
      );
    },
    formatAverage(voteAverage) {
      if (voteAverage != undefined) {
        return voteAverage.toFixed(1);
      }
    },
    formatRuntime(runtime) {
      if (runtime != undefined) {
        const horas = Math.floor(runtime / 60);
        const min = runtime % 60;
        const textoHoras = `00${horas}`.slice(-2);
        const textoMinutos = `00${min}`.slice(-2);

        return `${textoHoras}:${textoMinutos}`;
      }
    },
  },
};
</script>

<style>
</style>