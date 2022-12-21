<template>
  <div class="email-display">
    <div class="close-modal" @click="emit('closeModal')">X</div>
    <div>
      <button>Archive</button>
      <button @click="toggleRead">{{ email.read ? "Mark Unread (r)" : "Mark Read (r)" }}</button>
      <button>Newer</button>
      <button>Older</button>
    </div>
    <h2 class="mb-0">
      Subject: <strong>{{ email.subject }}</strong>
    </h2>
    <div>
      <em>From {{ email.from }} on {{ format(new Date(email.sentAt), "MMM do yyyy") }}</em>
    </div>
    <div v-html="marked(email.body)" />
  </div>
</template>

<script>
import { format } from "date-fns";
import marked from "marked";
import axios from "axios";
// import useKeydown from "../composables/use-keydown";

export default {
  setup(props, { emit }) {
    let email = props.email;
    let toggleRead = () => {
      email.read = !email.read;
      axios.put(`http://localhost:3000/emails/${email.id}`, email);
    };

    // usekeydown([
    //   {
    //     key: "r",
    //     fn: toggleRead
    //   }
    // ]);

    return {
      format,
      marked,
      emit
    };
  },
  props: {
    email: {
      type: Object,
      required: true
    }
  }
};
</script>

<style scoped>
.close-modal {
  cursor: pointer;
}
</style>
