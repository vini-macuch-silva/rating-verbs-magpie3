<template>
  <Experiment title="Rating verbs">
    <Screen :title="'Welcome!'">
      In this task you will be asked to think about the meaning of different verbs. It should take no longer than 5 minutes to complete.
        <br />
        <br />
      If you are ready to read the instructions, please go ahead and click the button below..
      <button @click="$magpie.nextScreen()">Begin the experiment</button>
    </Screen>

    <Screen :title="'Instructions'">
      <Slide>
      This study is about verbs that describe changes, such as the verb <b>to decrease</b>. Your task is to rate how much the verb relates to the two changes depicted by the pictures displayed on the screen, as shown below. The picture on the left depicts a change in size. The picture on the right depicts a change in vertical position. You can drag the slider towards either of the pictures to indicate which type of change is a better fit for the verb in question.
        <br />
        <br />
      If you think the verb doesn't relate particularly well to either type of change, or if you think it relates equally to both, you can leave the slider right in between both pictures.
        <br />
        <br />
      There is no right or wrong in this task. We ask you to follow your intuition.
      <div style="text-align: center;"><img src="../public/images/instructions_view.png" style="position:relative; width:615px; height:165px;"/></div>
      <button @click="$magpie.nextScreen()">Go to consent</button>
    </Slide>
    </Screen>

    <Screen :title="'Informed consent'">
      <ul>
        <li>I confirm that the purpose of the study has been explained and that I have understood it.</li>
          <br />
          <br />
        <li>I understand that my participation in this study is voluntary and that I am free to withdraw from the study at any time until submission of the form, without giving a reason and without consequence.</li>
          <br />
          <br />
        <li>I understand that data will be identifiable only via the Prolific ID, which will not be shared with any published data. I understand that all published data will be treated as confidential and anonymised.</li>
          <br />
          <br />
        <li>I understand that there are no known risks or hazards associated with participating in this study.</li>
      </ul>
        <br />
        <br />
        By pressing the arrow to proceed to the next screen, you are electronically signing the consent form, thereby confirming that you have read and understood the above information and that you agree to participate in the study.
      <button @click="$magpie.nextScreen()">Go to trials</button>
    </Screen>

    We iterate over our experiment trials
    <template v-for="(trial, i) in items_sorted">
      <Screen :key="i"
              :progress="i / items_sorted.length">
        <Slide>
          <p><strong>{{ trial.qud }}</strong></p>
          <p v-html="trial.question"></p>
            <div class="flexbox">
              <img :src="trial.picture1" class="padding" style="max-width:90px; max-height:90px;" />
              <SliderInput :initial="50" :response.sync="$magpie.measurements.rating" />
              <img :src="trial.picture2" style="max-width:90px; max-height:90px;" />
              <Record :data="{
                 'qud': trial.qud,
                 'optionLeft': trial.optionLeft,
                 'optionRight': trial.optionRight,
                 'polarity': trial.polarity,
                 'item_id': trial.item_id,
                 'trial_nr': i + 1}"
               />
            </div>
          <template v-if="typeof $magpie.measurements.rating !== undefined">
            <button @click="$magpie.saveAndNextScreen()">Next</button>
          </template>
        </Slide>
      </Screen>
    </template>

    <Screen>
      Thank you for participating in our study!
      <button @click="$magpie.nextScreen()">Next</button>
    </Screen>

    <PostTestScreen />

    <SubmitResultsScreen />
  </Experiment>
</template>

<script>
import _ from 'lodash';
import Slider from 'vue-slider-component';
import 'vue-slider-component/theme/default.css';
import items from '../trials/trials.csv';

var items_sorted = _.shuffle(items)

export default {
  name: 'App',
  data() {
    return {
      items_sorted: items_sorted
    };
  },
  computed: {
    // Expose lodash to template code
    _() {
      return _;
    }
  }
};

</script>

<style>
.flexbox {
  display: flex;
 }
</style>