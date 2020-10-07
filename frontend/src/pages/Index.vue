<template>
  <q-page>
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

  mounted() {
    this.$q.loading.show();
    setInterval(() => {
      this.getState();
    }, 2500);
  }

  getState() {
    this.$axios
      .get('http://smart.pakorns.com/get-state')
      .then(res => {
        console.log(res);
        this.datas = res.data.payload;
      })
      .finally(() => {
        this.$q.loading.hide();
      });
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
        this.$q.loading.hide();
      });
  }
}
</script>
