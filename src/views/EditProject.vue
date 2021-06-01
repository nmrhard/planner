<template>
  <form @submit.prevent="handleSubmit">
    <label for="title">Title</label>
    <input type="text" v-model="title" name="title" required>
    <label for="details">Details</label>
    <textarea name="details" v-model="details" cols="30" rows="10" required></textarea>
    <button type="submit">Update Project</button>
  </form>
</template>

<script>
export default {
  props: ['id'],
  data() {
    return {
      title: '',
      details: '',
      uri: `http://localhost:3000/projects/${this.id}`,
    }
  },
  mounted() {
    fetch(this.uri)
      .then(res => res.json())
      .then(data => {
        this.title  = data.title;
        this.details = data.details;
      })
  },
  methods: {
    handleSubmit() {
      fetch(this.uri, {
        method: 'PATCH',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({
          title: this.title,
          details: this.details,
        })
      }).then (() => this.$router.push('/'))
        .catch( err => console.log(err.message))
    }
  }
}
</script>

<style>

</style>