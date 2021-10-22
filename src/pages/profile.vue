<template>
  <q-page padding>
    <div class="column justify-center items-center">
      <h3>{{ character.name }}</h3>
      <q-card class="my-card">
        <img 
          :src="character.img"
          :alt="character.name"
          @click="alert = true"
        >

        <q-card-section>
          <div class="text-h6">{{ character.name }}</div>
          <div class="text-subtitle2">{{ character.nickname }}</div>
        </q-card-section>

        <q-card-section class="q-pt-none">
          <q-chip
            v-for="occupation in character.occupation"
            color="teal"
            :key="occupation"
            text-color="white"
            icon="bookmark"
          >
            {{ occupation }}
          </q-chip>
        </q-card-section>
      </q-card>
    </div>
    <q-dialog v-model="alert">
      <q-card>
        <q-card-section>
          <div class="text-h6">Birthday Info</div>
        </q-card-section>
        <q-card-section>
          {{character.birthday}}
        </q-card-section>
        <q-card-section align="right">
          <q-btn flat label="OK" color="primary" v-close-popup></q-btn>
        </q-card-section>
      </q-card>
    </q-dialog>
  </q-page>
</template>

<script>
import {QSpinnerPie} from 'quasar';

export default {

  async created() {
    this.$q.loading.show({
      message: 'loading...',
      spinnerColor: 'green',
      spinner: QSpinnerPie
    })
    let url = `https://www.breakingbadapi.com/api/characters/${this.$route.params.id}`;
    if (this.$route.params.id === 'random'){
      url = `https://www.breakingbadapi.com/api/character/${this.$route.params.id}`
    }
    const character = await this.$axios.get(url)
    this.$q.loading.hide();
    this.character = character.data[0];
  },
// TODO, add try catch
  data() {
    return {
      character: "",
      alert: false
    }
  }
}
</script>

<style>
  .my-card {
    width: 30%;
  }
</style>