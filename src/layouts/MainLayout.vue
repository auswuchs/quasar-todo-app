<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar>
        <q-btn flat dense round icon="menu" aria-label="Menu" @click="toggleLeftDrawer" />
      </q-toolbar>

      <div class="q-px-lg q-pt-xl q-mb-md">
        <div class="text-h3">Todo</div>
        <div class="text-subtitle1">{{ todayDate }}</div>
      </div>

      <q-img src="src/assets/imgs/flowers.jpg" class="header-image absolute-top"></q-img>
    </q-header>

    <q-drawer v-model="leftDrawerOpen" show-if-above :width="250" :breakpoint="500">
      <q-scroll-area
        style="height: calc(100% - 192px); margin-top: 192px; border-right: 1px solid #ddd"
      >
        <q-list padding>
          <q-item to="/" clickable v-ripple exact active-class="text-orange-14">
            <q-item-section avatar>
              <q-icon name="list" />
            </q-item-section>

            <q-item-section>Todo</q-item-section>
          </q-item>

          <q-item to="/help" clickable v-ripple exact active-class="text-orange-14">
            <q-item-section avatar>
              <q-icon name="help" />
            </q-item-section>

            <q-item-section>Help</q-item-section>
          </q-item>
        </q-list>
      </q-scroll-area>

      <q-img class="absolute-top" src="src/assets/imgs/flowers.jpg" style="height: 192px">
        <div class="absolute-bottom bg-transparent">
          <q-avatar size="56px" class="q-mb-sm">
            <img src="src/assets/imgs/avatar.png" />
          </q-avatar>
          <div class="text-weight-bold">Evgeny Auswuchs</div>
          <div>@auswuchs</div>
        </div>
      </q-img>
    </q-drawer>

    <q-page-container>
      <router-view v-slot="{ Component }">
        <keep-alive>
          <component :is="Component" />
        </keep-alive>
      </router-view>
    </q-page-container>
  </q-layout>
</template>

<script setup>
import { date } from 'quasar'
import { ref, computed } from 'vue'

const leftDrawerOpen = ref(false)

const todayDate = computed(() => {
  const timeStamp = Date.now()
  return date.formatDate(timeStamp, 'dddd D MMMM')
})

const toggleLeftDrawer = () => {
  leftDrawerOpen.value = !leftDrawerOpen.value
}
</script>

<style lang="scss" scoped>
.header-image {
  height: 100%;
  z-index: -1;
  opacity: 0.8;
  filter: grayscale(50%);
}
</style>
