<template>
  <section>
    <base-card>
      <h2>Submitted Experiences</h2>
      <div>
        <base-button>Load Submitted Experiences</base-button>
      </div>

      <p v-if="isLoading">Loading...</p>
      <ul v-else-if="!isLoading && surveyResults.length > 0">
        <survey-result
          v-for="result in surveyResults"
          :key="result.id"
          :name="result.name"
          :rating="result.rating"
        ></survey-result>
      </ul>

      <p v-else-if="!isLoading && surveyResults.length === 0">No experience found</p>
      <p v-if="error">Failed to fetch data</p>

    </base-card>
  </section>
</template>

<script>
import SurveyResult from './SurveyResult.vue';

export default {
  components: {
    SurveyResult,
  },
  data() {
    return {
      surveyResults: [],
      isLoading: false,
      error: false,
    }
  },
  mounted() {
    this.fetchSurveyResults()
  },
  methods: {
    async fetchSurveyResults() {
      this.isLoading = true
      this.error = false
      let data = null

      try {
        const response = await fetch("https://vue-http-demo-85943-default-rtdb.asia-southeast1.firebasedatabase.app/surveys.json", {
          headers: {
            'Content-Type': 'application/json',
          },
        })

        if (response.ok) {
          data = await response.json()

          for (const id in data) {
            console.log(data[id])
            this.surveyResults.push({
              id: id,
              name: data[id].name,
              rating: data[id].rating,
            })
          }
          setTimeout(() => {
            this.isLoading = false
          },3000)

          console.log(this.surveyResults)
        } else {
          setTimeout(() => {
            this.isLoading = false
          },3000)
        }
      } catch(err) {
        this.error = true
        console.log(err)
    }}}
}
</script>

<style scoped>
ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
</style>