<template>
  <div class="home">
    <CharacterList />
  </div>
</template>

<script>
// @ is an alias to /src
import CharacterList from '@/components/characterList.vue'

export default {
  name: 'Home',
  components: {
    CharacterList
  }
}
</script>
