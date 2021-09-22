<template>
  <v-dialog v-model="value" max-width="500px" persistent @keydown.esc="cancelAction()">
    <v-card>
      <v-card-title class="headline">
        {{title}}
      </v-card-title>
      <v-card-text>
        <div v-html="text"></div>

        <v-checkbox
            v-if="confirmCheckboxLabel"
            v-model="confirmCheckbox"
            hide-details
        >
          <template v-slot:label>
            <span
                :class="checkboxLabelClass"
            >
              {{ confirmCheckboxLabel }}
            </span>
          </template>
        </v-checkbox>
      </v-card-text>


      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn
            :color="cancelColor || 'red'"
            v-if="!this.loading"
            @click="cancelAction"
            text
            :loading="this.loading"
        >
          {{cancelText}}
        </v-btn>
        <v-btn
            :color="confirmColor || 'green'"
            @click="confirmAction"
            text
            :loading="this.loading"
            :disabled="!isConfirmEnabled"
        >
          {{confirmText}}
        </v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
export default {
  props: ["title", "text", "cancelText", "confirmText", "value", "cancelColor", "confirmColor", "confirmCheckboxLabel", "confirmCheckboxLabelColor"],
  data() {
    return {
      loading: false,
      confirmCheckbox: false,
    };
  },
  watch: {
    value: function() {
      this.resetState();
    }
  },
  computed: {
    isConfirmEnabled() {
      if(!this.confirmCheckboxLabel) return true

      return this.confirmCheckbox
    },
    checkboxLabelClass() {
      if(!this.confirmCheckboxLabelColor) return "red--text"

      return `${this.confirmCheckboxLabelColor}--text`
    }
  },
  methods: {
    resetState() {
      this.loading = false;
    },
    confirmAction() {
      this.loading = true;
      this.$emit("confirmAction");
    },
    cancelAction() {
      this.loading = true;
      this.$emit("cancelAction");
    }
  }
};
</script>
