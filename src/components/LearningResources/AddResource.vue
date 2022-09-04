<template>
  <base-dialog v-if="inputIsInvalid"
               @close="confirmError"
               title="Invalid Input">
    <template v-slot:default>
      <p>Please input at least one value</p>
    </template>
    <template v-slot:actions>
      <base-button @click="confirmError()">Okay</base-button>
    </template>

  </base-dialog>
  <base-card>
    <form @submit.prevent="submitData">
      <div class="form-control">
        <label for="title">Title</label>
        <input type="text"
               id="title"
               name="title"
               ref="title">
      </div>
      <div class="form-control">
        <label for="description">Description</label>
        <textarea id="description"
                  name="description"
                  rows="3"
                  ref="descInput"></textarea>
      </div>
      <div class="form-control">
        <label for="link">Link</label>
        <input id="link"
               type="url"
               name="link"
               ref="linkInput">
      </div>
      <base-button type="submit">Add Resource</base-button>
    </form>
  </base-card>
</template>
<script>

import BaseCard from '@/components/UI/BaseCard';
import BaseButton from "@/components/UI/BaseButton";
import BaseDialog from "@/components/UI/BaseDialog";

export default {
  inject: ['addResource'],
  data() {
    return {
      inputIsInvalid: false,
    }
  },
  components: {BaseDialog, BaseButton, BaseCard},
  methods: {
    submitData() {
      const enteredTitle = this.$refs.title.value;
      const enteredDescription = this.$refs.descInput.value;
      const enteredUrl = this.$refs.linkInput.value;
      // trim() ist a javascript method to get value of input even if user spaces
      if (enteredTitle.trim() === '' ||
          enteredDescription.trim() === '' ||
          enteredUrl.trim() === '') {
        this.inputIsInvalid = true;
        return;
      }
      this.addResource(enteredTitle, enteredDescription, enteredUrl);
    },
    confirmError() {
      this.inputIsInvalid = false;
    },
  }
};
</script>

<style scoped>
label {
  font-weight: bold;
  display: block;
  margin-bottom: 0.5rem;
}

input,
textarea {
  display: block;
  width: 100%;
  font: inherit;
  padding: 0.15rem;
  border: 1px solid #ccc;
}

input:focus,
textarea:focus {
  outline: none;
  border-color: #3a0061;
  background-color: #f7ebff;
}

.form-control {
  margin: 1rem 0;
}
</style>