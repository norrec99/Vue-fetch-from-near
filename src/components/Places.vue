<template>
  <div v-if="!loading && data && data.length" class="places">
    <div class="card" v-for="(place, index) of data" :key="index">
      <div class="card-info">
        <h4>{{ place.name }}</h4>
        <p>{{ place.description }}</p>
      </div>
      <img :src="place.image" />
    </div>
  </div>

  <p v-if="loading">Still loading..</p>
  <p v-if="error"></p>
</template>

<script>
function ABToStr(ab) {
  return new Uint8Array(ab).reduce((p, c) => p + String.fromCharCode(c), "");
}

import { ref, onMounted } from "vue";
export default {
  name: "Places",
  props: {},
  setup() {
    const data = ref(null);
    const loading = ref(true);
    const error = ref(null);

    function fetchData() {
      // Will be implemented next
      loading.value = true;
      // I prefer to use fetch
      // you can use use axios as an alternative
      return fetch("https://rpc.testnet.near.org", {
        method: "post",
        headers: {
          Accept: "application/json",
          "Content-Type": "application/json",
        },
        body: JSON.stringify({
          jsonrpc: "2.0",
          id: "dontcare",
          method: "query",
          params: {
            request_type: "call_function",
            finality: "final",
            account_id: "dev-1619693973968-7584817",
            method_name: "getPlaces",
            args_base64: "e30=",
          },
        }),
      })
        .then((res) => {
          // a non-200 response code

          if (!res.ok) {
            // create error instance with HTTP status text
            const error = new Error(res.statusText);
            error.json = res.json();
            throw error;
          }

          return res.json();
        })
        .then((json) => {
          // set the response data

          data.value = JSON.parse(ABToStr(json.result.result));
          console.log(data.value);
        })
        .catch((err) => {
          error.value = err;
          // In case a custom JSON error response was provided
          if (err.json) {
            return err.json.then((json) => {
              // set the JSON response message
              error.value.message = json.message;
            });
          }
        })
        .then(() => {
          loading.value = false;
        });
    }

    onMounted(() => {
      fetchData();
    });

    console.log(data);

    return {
      data,
      loading,
      error,
    };
  },
};
</script>

<style scoped>
.places {
  background-color: #fafafa;
  padding: 2rem 4rem;
  display: flex;
  justify-content: space-between;
}
.card {
  background-color: white;
  width: 22rem;
  border-radius: 1rem;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}
.card-info {
  padding: 2rem;
}
.card img {
  width: 100%;
  height: 22rem;
  object-fit: cover;
  padding: 0rem !important;
  padding-top: 1rem;
  border-bottom-left-radius: 1rem;
  border-bottom-right-radius: 1rem;
}
</style>