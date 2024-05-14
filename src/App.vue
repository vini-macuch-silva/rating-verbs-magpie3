<template>
  <Experiment title="Rating words">
    <Screen :title="'Welcome!'">
      In this task you will be asked to think about the meaning of words used to describe changes. It should take no longer than 5 minutes to complete.
        <br />
        <br />
      If you are ready to read the instructions, please go ahead and click the button below.
      <button @click="$magpie.nextScreen()">Begin the experiment</button>
    </Screen>

    <Screen :title="'Instructions'">
      <Slide>
      This study is about words that describe changes, such as <b>decrease</b>. Your task is to rate how much the word relates to the two changes depicted by the pictures displayed on the screen, as shown in the example below. 
        <br />
        <br />
      The picture on the left depicts a change in vertical position, that is, a circle moving down. The picture on the right depicts a change in size, that is, a circle becoming smaller. 
        <br />
        <br />
      You can drag the slider towards either of the pictures to indicate which type of change is a better fit for the word in question. The closer you move the slider to the picture, the more you think the word represents this type of change.
        <br />
        <br />
      If you think the word doesn't relate particularly well to either type of change, or if you think it relates equally to both, you can leave the slider right in between both pictures, as shown below.
        <br />
        <br />
      There is no right or wrong in this task. We ask you to follow your intuition.
      <div style="text-align: center;"><img src="../public/images/instructions_view.png"/></div>
      <button @click="$magpie.nextScreen()">Go to further instructions</button>
    </Slide>
    </Screen>

    <Screen :title="'Instructions'">
      <Slide>
      Because the words have different meanings, the pictures are changing on each screen. Please make sure to check the pictures each time before you respond.        <br />
        <br />
        <br />
      <div style="text-align: center;"><img src="../public/images/instructions_view.png"/></div>
      <br />
      <div style="text-align: center;"><img src="../public/images/instructions_view-2.png"/></div>
      <button @click="$magpie.nextScreen()">Go to consent</button>
    </Slide>
    </Screen>

    <Screen :title="'Informed consent'">
      <ul>
        <li>I confirm that the purpose of the study has been explained and that I have understood it.</li>
          <br />
          <br />
        <li>I understand that my participation in this study is voluntary and that I am free to withdraw from the study at any time until submission of the form, without giving a reason and without consequence. I understand that I can do so simply by closing my browser window.</li>
          <br />
          <br />
        <li>I understand that data will be identifiable only via the Prolific ID, which will not be shared with any published data. I understand that all published data will be treated as confidential and anonymised. I also understand that I will be asked to provide optional demographic information at the end of the study.</li>
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
              <img :src="trial.picture1" style="max-width:90px; max-height:90px;" />
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

    <PostTestScreen />

    <Screen>
      Thank you for participating in our study!
      <button @click="$magpie.nextScreen()">Next</button>
    </Screen>

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

ul {
  list-style-position: inside;
}

</style>