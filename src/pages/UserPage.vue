<template>
  <div  class="index-page">
    <UserChat ref="chat" class="index-page__chat" />
    <div class="index-page__input-wrapper">
      <UserInput @send="chat.scrollBottom()" v-model:extended="expanded" v-model="input" />
    </div>
  </div>
</template>
<script setup lang="ts">
import UserChat from 'src/components/UserChat.vue'
import UserInput from 'src/components/UserInput.vue'
import { computed, ref } from 'vue'

const chat = ref()

const expanded = ref(false)
const input = ref('')
const chatPadding = computed(() => {
  if (expanded.value) {
    return '448px'
  }
  return '156px'
})
</script>
<style scoped lang="scss">
@import "../css/app.scss";

.index-page {
  height: calc(100vh - 58px);
  display: flex;
  align-items: center;
  flex-direction: column;
  &__chat{
    flex: 1 0 auto;
    width: 100%;
    transition: padding 0.55s ease;
    padding-bottom: v-bind(chatPadding)
  }
  &__input-wrapper {
    @extend .background;
    position: absolute;
    border-radius: 12px 12px 0 0;
    bottom: 0;
    width: 100%;
    padding: 24px 16px 24px 16px;
    max-width: 1088px;
  }
}
</style>
