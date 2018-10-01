<template>
  <div class="main">
    <header class="header">
      <div class="game-logo ta_c">
        <img src="/static/images/gamelogo.png" alt="">
      </div>
    </header>
    <div class="character-select">
      <select v-model="characterSelected" @change="selectCharacter">
        <option disabled selected="selected" value="">请选择职业</option>
        <option v-for="(character, index) in characterList" v-bind:value="index" :key="index">{{character}}</option>
      </select>
    </div>
    <div class="emu">
      <div class="emu-content" v-if="characterSelected = -1">
        1233123
      </div>
    </div>
  </div>
</template>

<script>
import skill from '@/components/skillconfig'
export default {
  name: 'index',
  data () {
    return {
      characterSelected: -1,
      characterList: ['亚马逊', '骑士', '矮人', '精灵', '巫师', '女巫'],
      commonSkill: {},
      characterSkill: []
    }
  },
  mounted () {
    this.commonSkill = skill.common
    this.characterSkill = skill.character
  },
  methods: {
    selectCharacter: function () {
      alert('2222')
    }
  },
  computed: {}
}
</script>

<style lang="scss" scoped>
  .header {
    width: 100%;
    height: 1rem;
    .game-logo {
      height: 100%;
      font-size: 0;
      padding-left: 0.2rem;
      padding-right: 0.2rem;
      img {
        height: 100%;
      }
    }
  }

  .character-select {
    width: 100%;
    height: 0.3rem;
    margin-top: 0.1rem;
    font-size: 0;
    text-align: center;
    select {
      width: 3rem;
      height: 0.3rem;
    }
  }

  .emu {
    width: 100%;
    min-height: 3rem;
  }
</style>
