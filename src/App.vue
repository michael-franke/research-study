<template>
  <Experiment title="Study on problem solving">
    <InstructionScreen :title="'Welcome'">
      In this study, you are asked to come up with a method to solve a problem.

      <br>

      Concretely, you will first have to read a background scenario. 
      Please read it very carefully. 
      Afterwards, you will be asked to come up with a potential solution to the problem described in said scenario. 
      Finally, you will be asked to rate the reliability of the text describing the background scenario.
    </InstructionScreen>

    <Screen>

      <Slide>
        <Record :data="{
              group: group,
              task: 'solution'
              }" />
        <p><div style='color:gray'>Background scenario</div></p>
        <p>{{background}}</p>
        <p><div style='color:gray'>Question</div></p>
        <p><strong>In your opinion what does Addison need to do to reduce crime?</strong></p>
        <TextareaInput
            :response.sync= "$magpie.measurements.response"
          />
        <!-- TODO: validate that at least a number of characters have been typed -->
          <button @click="$magpie.saveAndNextScreen();">Continue</button>
      </Slide>

    </Screen>

    <!-- <TextareaScreen v-if="chooseQuestion() > 0.75" :options="randomOption()"  -->
    <!--   question="In your opinion what does Addison need to do to reduce crime?" -->
    <!--   qud="<div style='textcolor:gray'>Background scenario</div> -->
    <!--        <br> -->
    <!--        Crime is a wild beast preying on the city of Addison. The crime rate in the once peaceful city has steadily increased over the past three years. In fact, these days it seems that crime is lurking in every neighborhood. In 2004, 46,177 crimes were reported compared to more than 55,000 reported in 2007. The rise in violent crime is particularly alarming. In 2004, there were 330 murders in the city, in 2007, there were over 500." /> -->


    <!-- <TextareaScreen v-if="chooseQuestion() > 0.5 && chooseQuestion() <= 0.75" :options="randomOption()" -->
    <!--   question="In your opinion what does Addison need to do to reduce crime?" -->
    <!--   qud="Crime is a virus infecting on the city of Addison. The crime rate in the once peaceful city has steadily increased over the past three years. In fact, these days it seems that crime is plaguing every neighborhood. In 2004, 46,177 crimes were reported compared to more than 55,000 reported in 2007. The rise in violent crime is particularly alarming. In 2004, there were 330 murders in the city, in 2007, there were over 500." /> -->

    <!-- <TextareaScreen v-if="chooseQuestion() > 0.25 && chooseQuestion() <= 0.5" :options="randomOption()" -->
    <!--   question="In your opinion what does Addison need to do to reduce crime?"  -->
    <!--   qud=" Crime is a wild beast preying on the city of Addison. The crime rate in the once peaceful city has steadily increased over the past three years. In fact, these days it seems that crime is lurking in every neighborhood. The rise in violent crime is particularly alarming." /> -->

    <!-- <TextareaScreen v-if="chooseQuestion() <= 0.25" :options="randomOption()"  -->
    <!--   question="In your opinion what does Addison need to do to reduce crime?" -->
    <!--   qud="Crime is a virus infecting on the city of Addison. The crime rate in the once peaceful city has steadily increased over the past three years. In fact, these days it seems that crime is plaguing every neighborhood. The rise in violent crime is particularly alarming." /> -->

    <Screen :validations="{
      response: {
        required: $magpie.v.required
      }
    }">
      <Slide >
        <Record :data="{
              group: group,
              task: 'reliability'
              }" />
        Please rate the reliability of the text.
        <RatingInput quid="Quelle" :right="'very reliable'" :left="'not reliable'" :response.sync="$magpie.measurements.response" />

        <button v-if="!$magpie.validateMeasurements.response.$invalid" @click="$magpie.saveAndNextScreen()">Submit</button>

      </Slide>
    </Screen>

    <Screen>

      <Slide>
        <Record :data="{
              group: group,
              task: 'affiliation'
              }" />
        <p>Please state your political affiliation.</p>
        <ForcedChoiceInput
            :response.sync= "$magpie.measurements.response"
            :options="['Democrat', 'Republican', 'neither', 'rather not say']"
          @update:response="$magpie.saveAndNextScreen();"/>
      </Slide>

    </Screen>


    <PostTestScreen />
    <SubmitResultsScreen />
  </Experiment>
</template>

<script>
import _ from 'lodash';


// determine group

var group = _.shuffle(["A", "B", "C", "D"])[0] // TODO: give these groups more revealing names!

var backgrounds = {
    "A": "Crime is a wild beast preying on the city of Addison. The crime rate in the once peaceful city has steadily increased over the past three years. In fact, these days it seems that crime is lurking in every neighborhood. In 2004, 46,177 crimes were reported compared to more than 55,000 reported in 2007. The rise in violent crime is particularly alarming. In 2004, there were 330 murders in the city, in 2007, there were over 500.",
    "B": "Background B",
    "C": "Background C",
    "D": "Background D",
}

var background = backgrounds[group]

console.log(group)

export default {
  methods: {
    chooseQuestion: function () {
      console.log(this.random);
      return this.random;
    },
    randomOption: function () {
      const leftOption = this.options[Math.floor(Math.random() * this.options.length)];
      const rightOption = this.options.find(option => option !== leftOption);
      return [leftOption, rightOption]
    }
  },
  name: 'App',
  data() {
    return {
        group: group,
        background: background,
      options: [
        'Reform educational practices and create after school programs',
        'Increase street patrols that look for criminals'
      ],
      random: Math.random(),
      disableButton: true
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
