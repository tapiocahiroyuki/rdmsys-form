<template>
  <FmPartsBase :label="label" :required="required" :id="id" :errors="errors">
      <div class="px-3 px-md-0">
        <b-form-group v-slot="{ ariaDescribedby }" :id="id">
            <b-form-radio
              v-for="(item, key) in options"
              v-model="formValue"
              :state="hasError"
              :required="required || null"
              :aria-describedby="ariaDescribedby"
              :name="name"
              :value="(item.value) || item"
              :key="key"
              @change="updateValue"
              :disabled="item.disabled" 
              >{{ (item.text) || item }}</b-form-radio
            >
          </b-form-group>
      </div>
  </FmPartsBase>
</template>

<script>
import Mixin from "./Mixin.vue";

export default {
  mixins: [Mixin],
};
</script>