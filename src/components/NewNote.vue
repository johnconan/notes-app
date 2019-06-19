<template>
  <div class="new-note">
    <label>Title</label>
    <input type="text" v-model.trim="note.title">
    <label>Priority</label>

    <select v-model="note.notify">
      <option
        v-for="(prior, index) in priority"
        :key="index"
        :value="prior.value">{{ prior.text }}</option>
    </select>

    <label style="margin-top:20px;">Description</label>
    <textarea v-model.trim="note.descr"></textarea>
    <button class="btn btnPrimary" @click="addNote()">New Note</button>
  </div>
</template>

<script>
export default {
  props: {
    note: {
      type: Object,
      required: true
    },
    priority: {
      type: Array,
      required: true
    }
  },
  methods: {
    addNote() {
      this.$emit('addNote', this.note);
    }
  }
  
}
</script>

<style lang="scss">
.new-note {
  text-align: center;
}
select {
  width: 40%;
  height: 50px;
  margin-bottom: 50px;
}
</style>



