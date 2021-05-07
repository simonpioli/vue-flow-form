<script>
/*
  Copyright (c) 2020 - present, DITDOT Ltd. - MIT Licence
  https://github.com/ditdot-dev/vue-flow-form
  https://www.ditdot.hr/en
*/

import axios from "axios";
import LanguageModel from '../../models/LanguageModel'
import {QuestionType} from '../../models/QuestionModel'
import TextType from './TextType.vue'

export default {
  extends: TextType,
  name: QuestionType.ValidatedPhone,
  data() {
    return {
      inputType: 'tel',
      canReceiveFocus: true
    }
  },
  props: {
    validationEndpoint: String
  },
  methods: {
    async validate() {
      if (this.hasValue) {
        const self = this;
        return await this.validateRemote()
          .then(response => {
            self.$emit('send-validation-message', 'Please enter a valid UK phone number.');
            return response.data.result;
          }).catch(error => {
            console.error(error);
            return false;
          });
      }
      this.$emit('send-validation-message', 'Please enter a phone number.');
      return !this.question.required || this.hasValue;
    },

    validateRemote() {
      return axios.post(this.validationEndpoint, this.dataValue);
    }
  }
}
</script>
