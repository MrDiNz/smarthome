<template>
  <q-page>
    <div class="row justify-end">
      <q-card class="col-auto row q-pa-sm items-center">
        <q-icon
          class="col-auto"
          name="fas fa-thermometer-three-quarters"
          size="lg"
        />
        <div class="col-auto text-h6 q-pa-sm">
          {{ tempAndHum.temperature }}°C
        </div>
        <q-icon class="col-auto" name="fas fa-tint" size="lg" />
        <div class="col-auto text-h6 q-pa-sm">
          {{ tempAndHum.humidity }}
        </div>
      </q-card>
    </div>
    <div class="row">
      <q-card
        v-for="(data, i) in datas"
        :key="i"
        :dark="!data.status"
        class="col-auto q-pa-md q-ma-sm"
      >
        <div class="text-center">{{ data.id }}</div>
        <div class="row justify-center">
          <q-icon
            :color="data.status ? 'yellow' : 'grey'"
            class="col-auto q-pb-md"
            name="fas fa-lightbulb"
            size="md"
          />
        </div>

        <q-btn @click="lightControllerBtnClicked(data)">
          {{ data.status ? 'Off' : 'On' }}
        </q-btn>
      </q-card>
    </div>
  </q-page>
</template>

<script lang="ts">
import { Vue, Component } from 'vue-property-decorator';

@Component({
  components: {}
})
export default class PageIndex extends Vue {
  datas = {};
  tempAndHum = {};

  mounted() {
    this.$q.loading.show();
    this.getState();
    setInterval(() => {
      this.getState();
    }, 5000);
  }

  async getState() {
    await this.$axios
      .get('http://smart.pakorns.com/get-temp')
      .then(res => {
        console.log(res);
        this.tempAndHum = res.data.payload;
      })
      .finally(() => {});

    await this.$axios
      .get('http://smart.pakorns.com/get-state')
      .then(res => {
        console.log(res);
        this.datas = res.data.payload;
      })
      .finally(() => {});

    await this.$q.loading.hide();
  }

  lightControllerBtnClicked(data: any) {
    this.$q.loading.show();
    this.$axios
      .post('http://smart.pakorns.com/light-control', {
        board_num: data.id,
        condition: !data.status
      })
      .then(res => {
        console.log(res);
      })
      .catch(err => {
        console.log(err);
      })
      .finally(() => {
        this.getState();
      });
  }
}
</script>
