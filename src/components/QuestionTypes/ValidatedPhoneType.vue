<script>
/*
  Copyright (c) 2020 - present, DITDOT Ltd. - MIT Licence
  https://github.com/ditdot-dev/vue-flow-form
  https://www.ditdot.hr/en
*/

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
  methods: {
    validate() {
      let valid = !this.question.required || this.hasValue;
      if (this.hasValue && this.question.remoteValidationMethod !== null) {
        const self = this;
        const remoteResult = this.question.remoteValidationMethod(this.dataValue);
        console.log(remoteResult);
        if (remoteResult) {
          console.log('Pass');
          return true;
        } else {
          console.log('Fail');
          self.$emit('send-validation-message', 'Please enter a valid UK phone number.');
          return false;
        }
      }
      if (!valid) {
        this.$emit('send-validation-message', 'Please enter a phone number.');
      }
      return valid;
    },

    // validateRemote() {
    //   let response = false;
    //   const xhr = new XMLHttpRequest();
    //   xhr.open('POST', this.question.validationEndpoint);
    //   xhr.setRequestHeader('Content-Type', 'application/json');
    //   xhr.onreadystatechange = function() {
    //     if (xhr.status === 200) {
    //       response = JSON.parse(xhr.responseText);
    //     }
    //   }
    //   return response;
    // }
  }
}
</script>
