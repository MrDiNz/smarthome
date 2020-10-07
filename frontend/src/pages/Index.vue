<template>
  <q-page>
    {{ datas }}
    <div class="row">
      <q-card
        class="col-auto q-pa-sm q-ma-sm"
        v-for="(data, i) in datas"
        :key="i"
      >
        <q-icon name="fas fa-lightbulb" size="md" />
        <q-card-actions>
          <q-btn>test</q-btn>
        </q-card-actions>
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
    this.$axios.get('http://smart.pakorns.com/get-state').then(res => {
      console.log(res);
      this.datas = res.data.payload;
    });
  }
}
</script>
