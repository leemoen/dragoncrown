<template>
  <div class="main">
    <header class="header">
      <div class="game-logo ta_c">
        <img src="/static/images/gamelogo.png" alt="">
      </div>
    </header>
    <div class="character-select">
      <select v-model="characterSelected" @change="selectChanged">
        <option disabled selected="selected" value="">请选择职业</option>
        <option v-for="(character, index) in characterList" v-bind:value="character" :key="index">
          {{characterName[index]}}
        </option>
      </select>
    </div>
    <div class="skill-count-line" v-if="characterSelected !== ''">
      <div class="skill-count">技能点：{{skillTotal}}/155</div>
      <div class="skill-count" v-for="(className,index) in classNames" :key="index">
        {{className}}系：{{classCounts[index]}}
      </div>
    </div>
    <div class="emu">
      <div class="top-span"></div>
      <div class="emu-content" v-if="characterSelected !== ''">
        <div class="skill-box" v-for="(skill,index) in characterSkill[characterSelected]" :key="skill.name">
          <div class="skill-info">
            <div class="skill-img"><img :src="'/static/images/'+ characterSelected + index +'.jpg'" alt=""></div>
            <div class="skill-info-line">{{skill.name}} | {{skill.class}}系</div>
            <div class="skill-info-line"  v-if="skill.max === characterSkillCurrent[index]">{{skill.detail[characterSkillCurrent[index] - 1]}}</div>
            <div class="skill-info-line" v-else>升级后：{{skill.detail[characterSkillCurrent[index]]}}</div>
            <div class="skill-info-line" v-if="skill.max === characterSkillCurrent[index]"></div>
            <div class="skill-info-line" v-else>升级所需SP：{{skill.sp[characterSkillCurrent[index]]}}</div>
            <div class="skill-info-line" v-if="skill.max === characterSkillCurrent[index]"></div>
            <div class="skill-info-line" v-else>升级所需等级：{{skill.level[characterSkillCurrent[index]]}}</div>
            <div class="skill-info-line" v-if="skill.max === characterSkillCurrent[index]">已达满级</div>
            <div class="skill-info-line" v-else>升级需要{{skill.class}}系技能点数：{{skill.require[characterSkillCurrent[index]]}}</div>
            <div class="skill-level-line">
              <div class="skill-level-bottom" :style="'width: ' + skill.max * 10 + '%'"></div>
              <div class="skill-level-bg" :style="'width: ' + characterSkillCurrent[index] * 10 + '%'"></div>
              <img src="../assets/images/star.png" class="skill-level" alt="">
              <img src="../assets/images/star.png" class="skill-level" alt="">
              <img src="../assets/images/star.png" class="skill-level" alt="">
              <img src="../assets/images/star.png" class="skill-level" alt="">
              <img src="../assets/images/star.png" class="skill-level" alt="">
              <img src="../assets/images/star.png" class="skill-level" alt="">
              <img src="../assets/images/star.png" class="skill-level" alt="">
              <img src="../assets/images/star.png" class="skill-level" alt="">
              <img src="../assets/images/star.png" class="skill-level" alt="">
              <img src="../assets/images/star.png" class="skill-level" alt="">
            </div>
            <div class="skill-buttons">
              <img src="../assets/images/reduce.png" alt="" class="skill-button" @click="characterSkillSet(false,index)">
              <img src="../assets/images/plus.png" alt="" class="skill-button" @click="characterSkillSet(true,index)">
            </div>
          </div>
        </div>
        <div class="skill-box" v-for="(skill,index) in commonSkill" :key="skill.name">
          <div class="skill-info">
            <div class="skill-img"><img :src="'/static/images/common' + (index + 1) +'.jpg'" alt=""></div>
            <div class="skill-info-line">{{skill.name}} | {{skill.class}}系</div>
            <div class="skill-info-line" v-if="skill.max === commonSkillCurrent[index]">{{skill.detail[commonSkillCurrent[index] - 1]}}</div>
            <div class="skill-info-line" v-else>升级后：{{skill.detail[commonSkillCurrent[index]]}}</div>
            <div class="skill-info-line" v-if="skill.max === commonSkillCurrent[index]"></div>
            <div class="skill-info-line" v-else>升级所需SP：{{skill.sp[commonSkillCurrent[index]]}}</div>
            <div class="skill-info-line" v-if="skill.max === commonSkillCurrent[index]"></div>
            <div class="skill-info-line" v-else>升级所需等级：{{skill.level[commonSkillCurrent[index]]}}</div>
            <div class="skill-info-line" v-if="skill.max === commonSkillCurrent[index]">已达满级</div>
            <div class="skill-info-line" v-else>升级需要{{skill.class}}系技能点数：{{skill.require[commonSkillCurrent[index]]}}</div>
            <div class="skill-level-line">
              <div class="skill-level-bottom" :style="'width: ' + skill.max * 10 + '%'"></div>
              <div class="skill-level-bg" :style="'width: ' + commonSkillCurrent[index] * 10 + '%'"></div>
              <img src="../assets/images/star.png" class="skill-level" alt="">
              <img src="../assets/images/star.png" class="skill-level" alt="">
              <img src="../assets/images/star.png" class="skill-level" alt="">
              <img src="../assets/images/star.png" class="skill-level" alt="">
              <img src="../assets/images/star.png" class="skill-level" alt="">
              <img src="../assets/images/star.png" class="skill-level" alt="">
              <img src="../assets/images/star.png" class="skill-level" alt="">
              <img src="../assets/images/star.png" class="skill-level" alt="">
              <img src="../assets/images/star.png" class="skill-level" alt="">
              <img src="../assets/images/star.png" class="skill-level" alt="">
            </div>
            <div class="skill-buttons">
              <img src="../assets/images/reduce.png" alt="" class="skill-button" @click="commonSkillSet(false,index)">
              <img src="../assets/images/plus.png" alt="" class="skill-button" @click="commonSkillSet(true,index)">
            </div>
          </div>
        </div>
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
      characterSelected: '',
      characterName: ['亚马逊', '战士', '矮人', '精灵', '巫师', '女巫'],
      commonSkill: {},
      characterSkill: {},
      characterList: [],
      characterSkillCurrent: [],
      commonSkillCurrent: [0, 0, 0, 0, 0, 0, 0, 0, 0],
      classCounts: [0, 0, 0, 0],
      skillTotal: 0
    }
  },
  mounted () {
    this.commonSkill = skill.common
    this.characterSkill = skill.character
    this.characterList = Object.keys(this.characterSkill)
  },
  methods: {
    selectChanged: function () {
      this.skillTotal = 0
      this.characterSkillCurrent = []
      for (let i = 0; i < this.characterSkill[this.characterSelected].length; i++) {
        this.characterSkillCurrent.push(0)
      }
      this.commonSkillCurrent = [0, 0, 0, 0, 0, 0, 0, 0, 0]
      this.classCounts = [0, 0, 0, 0]
    },
    characterSkillSet: function (type, index) {
      if (type) {
        let thisClassCount = this.classCounts[this.classNames.indexOf(this.characterSkill[this.characterSelected][index].class)]
        if (this.characterSkillCurrent[index] >= this.characterSkill[this.characterSelected][index].max) {
          return
        }
        if (thisClassCount < this.characterSkill[this.characterSelected][index].require[this.characterSkillCurrent[index]]) {
          return
        }
        if (this.skillTotal >= 155 || this.skillTotal + this.characterSkill[this.characterSelected][index].sp[this.characterSkillCurrent[index]] > 155) {
          return
        }
        this.skillTotal += this.characterSkill[this.characterSelected][index].sp[this.characterSkillCurrent[index]]
        this.$set(this.characterSkillCurrent, index, this.characterSkillCurrent[index] + 1)
        this.$set(this.classCounts, this.classNames.indexOf(this.characterSkill[this.characterSelected][index].class), this.classCounts[this.classNames.indexOf(this.characterSkill[this.characterSelected][index].class)] + 1)
        // this.characterSkillCurrent[index]++
        // this.classCounts[this.classNames.indexOf(this.characterSkill[this.characterSelected][index].class)]++
      } else {
        if (this.characterSkillCurrent[index] === 0) {
          return
        }
        this.skillTotal -= this.characterSkill[this.characterSelected][index].sp[this.characterSkillCurrent[index] - 1]
        this.$set(this.characterSkillCurrent, index, this.characterSkillCurrent[index] - 1)
        this.$set(this.classCounts, this.classNames.indexOf(this.characterSkill[this.characterSelected][index].class), this.classCounts[this.classNames.indexOf(this.characterSkill[this.characterSelected][index].class)] - 1)
      }
    },
    commonSkillSet: function (type, index) {
      if (type) {
        let thisClassCount = this.classCounts[3]
        if (this.commonSkillCurrent[index] >= this.commonSkill[index].max) {
          return
        }
        if (thisClassCount < this.commonSkill[index].require[this.commonSkillCurrent[index]]) {
          return
        }
        if (this.skillTotal >= 155 || this.skillTotal + this.commonSkill[index].sp[this.commonSkillCurrent[index]] > 155) {
          return
        }
        this.skillTotal += this.commonSkill[index].sp[this.commonSkillCurrent[index]]
        this.$set(this.commonSkillCurrent, index, this.commonSkillCurrent[index] + 1)
        this.$set(this.classCounts, 3, this.classCounts[3] + 1)
      } else {
        if (this.commonSkillCurrent[index] === 0) {
          return
        }
        this.skillTotal -= this.commonSkill[index].sp[this.commonSkillCurrent[index] - 1]
        this.$set(this.commonSkillCurrent, index, this.commonSkillCurrent[index] - 1)
        this.$set(this.classCounts, 3, this.classCounts[3] - 1)
      }
    }
  },
  computed: {
    classNames: function () {
      let _classNames = []
      for (let i = 0; i < this.characterSkill[this.characterSelected].length; i++) {
        if (_classNames.indexOf(this.characterSkill[this.characterSelected][i].class) === -1) {
          _classNames.push(this.characterSkill[this.characterSelected][i].class)
        }
      }
      _classNames.push('一般')
      return _classNames
    }
  }
}
</script>

<style lang="scss" scoped>
  .header {
    width: 10rem;
    height: 1rem;
    position: fixed;
    top: 0;
    left: 50%;
    background: #eee;
    transform: translate(-50%, 0);
    z-index: 99;
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
    width: 10rem;
    height: 0.4rem;
    padding-top: 0.1rem;
    font-size: 0;
    text-align: center;
    position: fixed;
    top: 1.0rem;
    left: 50%;
    transform: translate(-50%, 0);
    background: #eee;
    z-index: 99;
    select {
      width: 3rem;
      height: 0.3rem;
      font-size: 0.14rem;
      option {
        font-size: 0.14rem;
      }
    }
  }

  .skill-count-line {
    position: fixed;
    top: 1.4rem;
    left: 50%;
    transform: translate(-50%, 0);
    width: 9.6rem;
    height: 0.4rem;
    background: #eee;
    z-index: 99;
    .skill-count {
      width: 20%;
      height: 0.4rem;
      font-size: 0.16rem;
      line-height: 0.4rem;
      text-align: center;
      float: left;
    }
  }

  .emu {
    width: 100%;
    min-height: 3rem;
    .top-span {
      width: 100%;
      height: 2.1rem;
    }
    .emu-content {
      width: 9.6rem;
      margin-left: auto;
      margin-right: auto;
      overflow: hidden;
      .skill-box {
        height: 1.6rem;
        margin-bottom: 0.1rem;
        position: relative;
        .skill-info {
          width: 100%;
          height: 1.6rem;
          padding-left: 1rem;
          position: relative;
          .skill-buttons{
            position: absolute;
            right: 0.2rem;
            top: 50%;
            transform: translate(0, -50%);
            width: 1rem;
            height: 0.4rem;
            font-size: 0;
            .skill-button{
              width: 0.4rem;
              height: 0.4rem;
              &:last-child{
                margin-left: 0.1rem;
              }
            }
          }
          .skill-img {
            position: absolute;
            width: 0.9rem;
            height: 1.6rem;
            left: 0;
            top: 0;
            font-size: 0;
            img {
              width: 100%;
            }
          }
          .skill-info-line{
            width: 100%;
            height: 0.24rem;
            line-height: 0.24rem;
          }
          .skill-level-line{
            position: relative;
            width: 3rem;
            height: 0.3rem;
            font-size: 0;
            margin-top: 0.05rem;
            .skill-level-bg{
              position: absolute;
              top:0;
              left: 0;
              z-index:-1;
              height: 0.3rem;
              background: linear-gradient(to right, #f9cd48 ,#f96e48);
            }
            .skill-level-bottom{
              position: absolute;
              top:0;
              left: 0;
              z-index:-10;
              height: 0.3rem;
              background: linear-gradient(to right, #ccc ,#ddd);
            }
            .skill-level{
              position: relative;
              z-index: 10;
              width: 0.3rem;
              height: 0.3rem;
              float: left;
            }
          }
        }
      }
    }
  }

  @media screen and (min-width: 750px) {
    .skill-box {
      width: 50%;
      float: left;
    }
  }

  @media screen and (max-width: 750px) {
    .skill-box {
      width: 100%;
    }
  }
</style>
