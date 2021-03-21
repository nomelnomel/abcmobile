<template>
  <div class="selects" v-click-outside="closeSelect">
    <div
        class="btn btn-select"
         @click="openSelect = true"
         :class="{'error': error}"
    >
      {{ isItemChoosen ? choosenItem : word }}
    </div>
    <div class="select-items" v-if="openSelect">
      <div
          class="select-item"
          v-for="(item,i) in arr"
          :key="i"
          @click="selectItem(item)"
      >
        {{ item }}
      </div>
    </div>
  </div>
</template>

<script>
import ClickOutside from 'vue-click-outside'

export default {
props: ['word', 'arr', 'error', 'getAge'],
  data() {
    return {
      openSelect: false,
      isItemChoosen: false,
      choosenItem: '',
    }
  },
  methods: {
    selectItem(item) {
      this.choosenItem = item
      this.isItemChoosen = true
      this.openSelect = false
      this.$emit('noError')
      this.$emit('selectIsChoosen')
      if(this.getAge){
        let age = new Date().getFullYear() - item
        this.$emit('gotAge', age)
      }
    },
    closeSelect(){
      this.openSelect = false
    }
  },

  directives: {
    ClickOutside
  },
}
</script>

<style>

</style>