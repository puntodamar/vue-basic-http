<template>
  <section>
    <base-card>
      <h2>Submitted Experiences</h2>
      <div>
        <base-button>Load Submitted Experiences</base-button>
      </div>
      <ul>
        <survey-result
          v-for="result in surveyResults"
          :key="result.id"
          :name="result.name"
          :rating="result.rating"
        ></survey-result>
      </ul>
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
      surveyResults: []
    }
  },
  mounted() {
    this.fetchSurveyResults()
  },
  methods: {
    async fetchSurveyResults() {
      let data = null
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

        console.log(this.surveyResults)
      }
    }
  }
};
</script>

<style scoped>
ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
</style>