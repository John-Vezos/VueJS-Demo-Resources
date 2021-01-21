<template>

  <!-- close the base dialog when click out of the border -->
  <base-dialog v-if="inputIsInvalid" title="Invalid Input" @close="confirmError">
    
    <!-- slot name="default" -->
    <template #default>
      <p> Unfortunately, at least one input value is invalid. </p>
      <p> Please check all inputs and make sure you enter at least a few character into each input field. </p>
    </template>

    <!-- slot name="actions" -->
    <template #actions>
      <base-button @click="confirmError"> Okay </base-button>
    </template>

  </base-dialog>

  <base-card>
    <!-- A form to create a new resource -->
    <form @submit.prevent="submitData">

      <div class="form-control">
        <label for="title"> Title </label>
        <input id="title" name="title" type="text" ref="titleInput" />
      </div>

      <!-- textarea instead text for longer description -->
      <div class="form-control">
        <label for="description"> Description </label>
        <textarea id="description" name="description" rows="3" ref="descriptionInput" ></textarea>
      </div>

      <!-- url input -->
      <div class="form-control">
        <label for="link"> Link </label>
        <input id="link" name="link" type="text" ref="linkInput" />
      </div>

      <div>
        <base-button type="submit"> Add Resource </base-button>
      </div>

    </form>
  </base-card>

</template>

<script>
export default {
  inject: ['addResource'],
  data() {
    return {
      inputIsInvalid: false,
    };
  },
  methods: {
    submitData() {
      const enteredTitle = this.$refs.titleInput.value;
      const enteredDescription = this.$refs.descriptionInput.value;
      const enteredLink = this.$refs.linkInput.value;

      // all fields required to be completed before send the form
      if (enteredTitle.trim() === '' ||
          enteredDescription.trim() === '' ||
          enteredLink.trim() === ''
      ) {
        this.inputIsInvalid = true;
        return;
      }
      this.inputIsInvalid = false;

      // add the new resource
      // Add a timestamp for the unique id
      const currId = new Date().toISOString();
      this.addResource(currId, enteredTitle, enteredDescription, enteredLink);

      fetch('https://vuejs-demo-resources-default-rtdb.europe-west1.firebasedatabase.app/resources.json', {
        method: 'POST',
        headers: {
          'Content-Type' : 'application/json',
        },
        body: JSON.stringify({
          id: currId,
          title: enteredTitle,
          description: enteredDescription,
          url: enteredLink,
        }),
      });

      // clear fields from form
      this.clearFormFields();
    },
    
    // close dialog alert
    confirmError() {
      this.inputIsInvalid = false;
    },

    // remove data from fields 
    clearFormFields() {
      this.$refs.titleInput.value = '';
      this.$refs.descriptionInput.value = '';
      this.$refs.linkInput.value = '';
    }
  },
}
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