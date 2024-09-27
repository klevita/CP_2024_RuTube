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
          <div class="logo">
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
          <div class="text-dark text-subtitle1 q-mr-sm">
            {{ store.user.username }}
          </div>
          <q-btn @click="handleClick()" flat round color="primary" size="md" icon="logout" />
        </div>
      </q-toolbar>
    </q-header>
    <q-drawer v-model="drawerOpened" class="bg-dark" overlay elevated :breakpoint="700">
      <q-scroll-area class="fit" >
        <UserRooms v-if="store.user.username === 'admin'" />
      </q-scroll-area>
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
import svgLogo from 'assets/svg/rutube.svg'
import { useUserStore } from 'src/stores/UserStore'
import UserRooms from 'src/components/UserRooms.vue'
import { symRoundedMenu } from '@quasar/extras/material-symbols-rounded'
import { useRouter } from 'vue-router'
import { computed, ref, watch } from 'vue'
import { useSearchStore } from 'src/stores/searchStore'
import { debounce } from 'quasar'

const router = useRouter()
const store = useUserStore()
const messageStore = useMessageStore()
const drawerOpened = ref(true)

const searchStore = useSearchStore()
const search = ref('')
const searchHovered = ref(false)
const searchFocused = ref(false)

const setSearch = debounce(function (v) {
  searchStore.search = v
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
