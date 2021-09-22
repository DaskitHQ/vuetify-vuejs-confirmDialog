<template>
  <confirmDialog
    :value="state.isOpen"
    :title="state.title"
    :text="state.text"
    :cancelText="state.cancelText"
    :confirmText="state.confirmText"
    :cancelColor="state.cancelColor"
    :confirmColor="state.confirmColor"
    :confirm-checkbox-label="state.confirmCheckboxLabel"
    :confirm-checkbox-label-color="state.confirmCheckboxLabelColor"
    v-on:cancelAction="() => this.emmitClose(false)"
    v-on:confirmAction="() => this.emmitClose(true)"
  />
</template>

<script>
export default {
  name: "PromiseVuetifyConfirm",
  data() {
    return {
      state: {
        isOpen: false,
        title: "",
        text: "",
        cancelText: "",
        confirmText: "",
        cancelColor: "",
        confirmColor: "",
        confirmCheckboxLabel: null,
        confirmCheckboxLabelColor: null,
        promiseResolver: undefined,
        promiseRejecter: undefined
      }
    };
  },
  methods: {
    commit: function(newState) {
      this.state = newState;
    },
    setTitle: function(title) {
      this.state.title = title;
    },
    setText: function(text) {
      this.state.text = text;
    },
    setCancelText: function(text) {
      this.state.cancelText = text;
    },
    setConfirmText: function(text) {
      this.state.confirmText = text;
    },
    setPromiseHandler: function(promiseRejecter, promiseResolver) {
      this.state.promiseRejecter = promiseRejecter;
      this.state.promiseResolver = promiseResolver;
    },
    show: function() {
      this.state.isOpen = true;
    },
    emmitClose: function(state) {
      if (this.state.promiseResolver) {
        this.state.promiseResolver(state);
        this.state.isOpen = false;
      }
    }
  }
};
</script>