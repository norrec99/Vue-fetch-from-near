<template>
    <h1>Vue 3 and Fetch Example</h1>
<div>
    <ul v-if="!loading && data && data.length">
        <li v-for="(post, index) of data" :key="index">
        <p><strong>{{ post.name }}</strong></p>
        <p></p>
        </li>
    </ul>
</div>

  <p v-if="loading">
   Still loading..
  </p>
  <p v-if="error">


  </p>
</template>

<script>
function ABToStr (ab) {
    return new Uint8Array(ab).reduce((p, c) => p + String.fromCharCode(c), '');
}

import { ref, onMounted } from "vue";
export default {
  name: 'Posts',
  props: {
  },
  setup() {
    const data = ref(null);
    const loading = ref(true);
    const error = ref(null);

    function fetchData() {
    // Will be implemented next
    loading.value = true;
    // I prefer to use fetch
    // you can use use axios as an alternative
    return fetch('https://rpc.testnet.near.org', {
        method: 'post',
        headers: {
        'Accept': 'application/json',
        'Content-Type': 'application/json'
        },
        body: JSON.stringify({
                "jsonrpc": "2.0",
                "id": "dontcare",
                "method": "query",
                "params": {
                    "request_type": "call_function",
                    "finality": "final",
                    "account_id": "dev-1619083926564-1585583",
                    "method_name": "addPlace",
                    "args_base64": "e30="
                }
            })
    })
    .then(res => {
      // a non-200 response code

      if (!res.ok) {
        // create error instance with HTTP status text
        const error = new Error(res.statusText);
        error.json = res.json();
        throw error;
      }

      return res.json();
    })
    .then(json => {
      // set the response data


      data.value = JSON.parse(ABToStr(json.result.result))
      console.log(data.value)
    })
    .catch(err => {
      error.value = err;
      // In case a custom JSON error response was provided
      if (err.json) {
        return err.json.then(json => {
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

    console.log(data)

    return {
      data,
      loading,
      error
    };
  }
}
</script>