<template>
  <q-layout view="hHh Lpr lFf">
    <q-header elevated class="bg-dark relative-position">
      <q-toolbar color="white" class="q-my-xs q-mx-sm">
        <div class="row">
          <q-btn @click="drawerOpened = !drawerOpened" v-if="store.user.username === 'admin'" class="q-mr-md"
            color="white" padding="xs" flat>
            <q-icon :name="symRoundedMenu" size="28px" />
          </q-btn>
          <div v-else style="width:48px"></div>
            <div v-if="Screen.width > 700" class="logo">
              <svg-logo />
            </div>
          </div>
        <q-space />
        <div class="absolute-center">
          <q-input label-color="white" bg-color="white" color="transparent" dense outlined hide-hint @focus="searchFocused = true" @blur="searchFocused = false"
              @mouseover="searchHovered = true" @mouseleave="searchHovered = false" v-model="search"
              placeholder="Поиск по сообщениям">
              <template v-slot:append>
                <q-icon :color="searchIconColor" name="search" />
              </template>
          </q-input>
        </div>
        <q-space />
        <div class="row items-center">
          <q-btn @click="chatOpened = !chatOpened" v-if="store.user.username === 'admin'" class="q-mr-md"
            color="white" padding="xs" flat>
            <q-icon :name="symRoundedNeurology" size="28px" />
          </q-btn>
          <q-btn @click="handleClick()" flat round color="primary" size="md" icon="logout" />
        </div>
      </q-toolbar>
    </q-header>
    <q-drawer v-model="drawerOpened" class="bg-dark" overlay elevated :breakpoint="700">
      <q-scroll-area class="fit" >
        <UserRooms v-if="store.user.username === 'admin'" />
      </q-scroll-area>
    </q-drawer>
    <q-drawer v-model="chatOpened" :width="Screen.width > 800? 800: Screen.width" class="bg-dark" side="right" size="right" :overlay="Screen.width < 1200" elevated :breakpoint="700">
      <div class="absolute-right" style="z-index: 1;">
        <q-btn class="q-mr-md q-mt-sm" @click="chatOpened = false;" unelevated size="md" :icon="symRoundedClose" round />
      </div>
      <div class="q-pa-md">
        <model-chat />
      </div>
    </q-drawer>
    <q-page-container>
      <q-page class="background">
        <router-view />
      </q-page>
    </q-page-container>
  </q-layout>
</template>
<script setup lang="ts">
import { useMessageStore } from 'src/stores/MessageStore'
import { symRoundedNeurology, symRoundedMenu, symRoundedClose } from '@quasar/extras/material-symbols-rounded'
import svgLogo from 'assets/svg/rutube.svg'
import { useUserStore } from 'src/stores/UserStore'
import UserRooms from 'src/components/UserRooms.vue'
import { useRouter } from 'vue-router'
import { computed, ref, watch } from 'vue'
import { useSearchStore } from 'src/stores/searchStore'
import { debounce, Screen } from 'quasar'
import ModelChat from 'src/components/ModelChat.vue'

const router = useRouter()
const store = useUserStore()
const messageStore = useMessageStore()
const drawerOpened = ref(true)

const chatOpened = ref(false)

const searchStore = useSearchStore()
const search = ref('')
const searchHovered = ref(false)
const searchFocused = ref(false)

const setSearch = debounce(function (v) {
  searchStore.setSearch(v)
}, 500)

const searchIconColor = computed(() => {
  if (searchFocused.value) {
    return 'accent'
  }
  if (searchHovered.value) {
    return 'black'
  }
  return 'grey-8'
})

function handleClick () {
  messageStore.disconnect()
  store.logout()
  router.push({ name: 'Login' })
}

watch(() => store.user.username, (name) => {
  if (name !== 'admin') {
    drawerOpened.value = false
  }
}, { immediate: true })
watch(search, (v) => {
  setSearch(v)
})
</script>
<style scoped lang="scss">
.logo {
  width: 200px;
  height: 36px;
  margin-top: -3px
}
</style>
