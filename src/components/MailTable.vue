<template>
  <table class="mail-table">
    <tbody>
      <tr
        v-for="email in unarchivedEmails"
        :key="email.id"
        :class="[email.read ? 'read' : '', 'clickable']"
        @click="openEmail(email)"
      >
        >
        <td>
          <input type="checkbox" name="" id="" />
        </td>
        <td>{{ email.from }}</td>
        <td>
          <p>
            <strong>{{ email.subject }}</strong> - {{ email.body }}
          </p>
        </td>
        <td class="date">{{ format(new Date(email.sentAt), "MMM do yyyy") }}</td>
        <td>
          <button @click="archiveEmail(email)">Archive</button>
        </td>
      </tr>
    </tbody>
  </table>
  <ModalView v-if="openedEmail" @closeModal="openedEmail = null">
    <MailView :email="openedEmail" @closeModal="openedEmail = null" />
  </ModalView>
</template>

<script>
import { format } from "date-fns";
import axios from "axios";
import MailView from "./MailView.vue";
import ModalView from "./ModalView.vue";

export default {
  async setup() {
    // await new Promise(resolve => setTimeout(resolve, 1000)); leaving it here in case of need
    let response = await axios.get("http://localhost:3000/emails");
    let emails = response.data;
    return {
      format,
      emails,
      openedEmail: null
    };
  },
  components: {
    MailView,
    ModalView
  },
  computed: {
    sortedEmails() {
      return this.emails.sort((e1, e2) => {
        return e1.sentAt < e2.sentAt ? 1 : -1;
      });
    },
    unarchivedEmails() {
      return this.sortedEmails.filter(e => !e.archived);
    }
  },
  methods: {
    openEmail(email) {
      email.read = true;
      this.updateEmail(email);
      this.openedEmail = email;
    },
    archiveEmail(email) {
      email.archived = true;
      this.updateEmail(email);
    },
    updateEmail(email) {
      axios.put(`http://localhost:3000/emails/${email.id}`, email);
    }
  }
};
</script>

<style scoped></style>
