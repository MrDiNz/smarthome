<template>
  <q-layout>
    <q-header elevated>
      <q-toolbar>
        <q-btn
          aria-label="Menu"
          dense
          flat
          icon="menu"
          round
          @click="leftDrawerOpen = !leftDrawerOpen"
        />
        <q-space />
        <q-toolbar-title>
          MrDiNz SmartHome
        </q-toolbar-title>
        <q-space />
      </q-toolbar>
    </q-header>

    <q-drawer
      v-model="leftDrawerOpen"
      bordered
      content-class="bg-grey-1"
      show-if-above
    >
      <q-list>
        <q-item-label class="text-grey-8" header>
          Menu
        </q-item-label>
        <div
          v-for="link in links"
          :key="link.path"
          @click="menuClicked(link.path)"
        >
          <q-item>
            <q-item-section v-if="link.icon" avatar>
              <q-icon :name="link.icon" />
            </q-item-section>

            <q-item-section>
              <q-item-label>{{ link.name }}</q-item-label>
            </q-item-section>
          </q-item>
        </div>
      </q-list>
    </q-drawer>

    <q-page-container>
      <router-view class="q-pt-sm" />
    </q-page-container>
  </q-layout>
</template>

<script lang="ts">
import { Vue, Component } from 'vue-property-decorator';

@Component({
  components: {}
})
export default class MainLayout extends Vue {
  leftDrawerOpen = false;
  links = [
    { path: '/', icon: 'fas fa-home', name: 'Home' },
    { path: '/monitoring', icon: 'fas fa-chart-line', name: 'Monitoring' }
  ];

  menuClicked(path: string) {
    this.$router.push(path);
  }
}
</script>
