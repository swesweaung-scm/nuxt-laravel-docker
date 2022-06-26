<template>
  <div>
    <client-only>
      <button class="btn btn-secondary next-btn">
        <NuxtLink to="/" class="next-route">Exit</NuxtLink>
      </button>
      <b-tabs content-class="mt-3" fill>
        <b-tab
          v-for="item in data.languages"
          :key="item.id"
          :title="item.title"
        >
          <div v-for="(d, i) in item.children" :key="d.id">
            <b-card class="border-top-0">
              <span>Question {{ i + 1 }}</span>
              <br />
              <div class="mb-2">
                <span>{{ d.title }} </span>
              </div>
              <div
                v-for="(cd, index) in d.children"
                :key="cd.id"
                class="d-flex align-items-center mt-2 justify-content-between"
              >
                <div
                  class="d-inline p-1"
                  :class="
                    recordArray.includes(cd.id.toString()) && cd.status === 0
                      ? 'bg-danger text-white'
                      : cd.status === 1
                      ? 'bg-success text-white'
                      : ''
                  "
                >
                  <span>({{ index + 1 }}) {{ cd.title }}</span>
                </div>
                <div
                  class="d-inline p-1 bg-success text-white"
                  v-if="cd.status"
                >
                  <span>{{
                    recordArray.includes(cd.id.toString())
                      ? "Your answer"
                      : "correct answer"
                  }}</span>
                </div>
                <div
                  class="d-inline p-1 bg-danger text-white"
                  v-if="
                    recordArray.includes(cd.id.toString()) && cd.status === 0
                  "
                >
                  <span>Your answer</span>
                </div>
              </div>
            </b-card>
          </div>
        </b-tab>
      </b-tabs>
    </client-only>
  </div>
</template>

<script>
export default {
  async asyncData({ store, redirect, params }) {
    let languages, records;
    if (process.client) {
      languages = store.state.languages;
      records = store.state.records.find((record) => record.id == params.id);
    } else {
      redirect("/");
    }
    return {
      data: {
        languages: languages,
        records: records,
      },
    };
  },
  computed: {
    recordArray() {
      return this.data.records.answer.split(",");
    },
  },
};
</script>
