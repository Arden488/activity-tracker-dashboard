<template>
  <div class="entry-list">
    <div class="entry-item" v-for="entry in entries" :key="entry.id">
      {{ new Date(entry.datetime).toLocaleString() }}<br />
      <div class="entry-item-props">
        <span>activity: {{ entry.activity }}</span>
        <span>coffee: {{ entry.coffee }}</span>
        <span>eat: {{ entry.eat }}</span>
        <span>energy: {{ entry.energy }}</span>
        <span>mood: {{ entry.mood }}</span>
        <span>location: {{ entry.location }}</span>
        <span>water: {{ entry.water }}</span>
      </div>
    </div>
  </div>
</template>

<style>
.entry-item {
  margin-bottom: 20px;
}
.entry-item-props {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
}
</style>

<script>
export default {
  data() {
    return {
      entries: null,
    }
  },
  methods: {
    async getEntries() {
      const ref = this.$fire.firestore
        .collection('hourly')
        .orderBy('datetime')
        .startAt(new Date().getTime() - 86400 * 1000)
      try {
        const snapshot = await ref.get()
        this.entries = snapshot.docs.map((doc) => {
          return {
            id: doc.id,
            ...doc.data(),
          }
        })
      } catch (e) {
        console.error(e)
        return
      }
    },
  },
  mounted() {
    this.getEntries()
    console.log('mounted')
  },
}
</script>