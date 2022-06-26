<template>
  <b-container fluid class="p-5">
    <b-row>
      <b-col class="d-flex justify-content-end">
        <b-button variant="primary" tag="nuxt-link" :to="`/quiz/1`"
          >Start Quiz</b-button
        >
      </b-col>
    </b-row>
    <b-row class="mt-3">
      <b-col>
        <b-table striped hover :items="data" :fields="fields">
          <template #cell(details)="row">
            <b-button
              size="sm"
              class="mr-2"
              tag="nuxt-link"
              :to="`/record/${row.item.id}`"
            >
              Details
            </b-button>
          </template>
        </b-table>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
import moment from "moment";
export default {
  name: "IndexPage",
  async asyncData({ $axios, store }) {
    try {
      const data = await $axios.$get(`/api/v1/quiz`);
      store.commit("SET_LANGUAGES", data.quizzes);
      store.commit("SET_RECORDS", data.records);
      const tempData = [];
      data.quizzes.map((element) => {
        tempData.push({
          language_id: element.id,
          answers: element.children
            ? Array.from({ length: element.children.length }, () => "")
            : null,
        });
      });
      store.commit("SET_INIT_ANSWERS", tempData);
      let items = data.records.map((record, index) => {
        return {
          id: record.id,
          "no.": index + 1,
          date: moment(record.created_at).format("DD-MM-YYYY"),
          correct_answers: record.details_answer.filter(
            (ans) => ans.status === 1
          ).length,
        };
      });
      return {
        data: items,
        fields: ["no.", "date", "correct_answers", "details"],
      };
    } catch (error) {
      console.log(error);
    }
  },
};
</script>
