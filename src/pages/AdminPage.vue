<template>
  <div class="admin-page">
    <MdPreview class="admin-page__content" :modelValue="text" language="ru"/>
  </div>
</template>
<script setup lang="ts">// Админская страница, пустая
import { MdPreview, config } from 'md-editor-v3'
import 'md-editor-v3/lib/style.css'
import ru from '@vavt/cm-extension/dist/locale/ru'

config({
  editorConfig: {
    languageUserDefined: {
      ru
    }
  }
})

const text = `# Доброго времени суток, администратор!
#### Дружеское напоминание как работать с системой 😉

 - Слева можно увидеть список комнат, в которых пользователи задают свои вопросы 🌟
 - Ты можешь зайти в любую комнату 🏠, и посмотреть как у пользователя дела, и оставить свои комментарии ✨
- Если нейросеть в комнате не справляется 🌧️, то такая комната будет помечена знаком вопроса ❓
- Твоя задача, помочь как можно большему количеству человек справиться с их трудностями, удачи и хорошего настроения❗
Смартфон vivo`
</script>
<style scoped lang="scss">
.admin-page{
  display: flex;
  justify-content: center;
  &__content{
    width: fit-content;
    background-color: transparent;
  }

}
</style>
