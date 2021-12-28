<template>
  <div class="pc-start">
    <div class="pc-brand">
      <span class="pc-brand__text">{{ title }}</span>
    </div>
    <pc-drawing-pad v-model="drawing" ref="pad" />
    <pc-button
      label="undo"
      @click="undo()" />
      <pc-button
        label="clear"
        @click="clear()" />
    <div style="display: flex; flex-direction: row; transform: scale(0.5);">
      <pc-drawing-pad
        v-model="normalizedDrawing"
        :height="255"
        :width="255"
        :read-only="true" />
      <pc-drawing-pad
        v-model="normalizedDrawing"
        :height="255"
        :width="255"
        :read-only="true" />
      <pc-drawing-pad
        v-model="normalizedDrawing"
        :height="255"
        :width="255"
        :read-only="true" />
      <pc-drawing-pad
        v-model="normalizedDrawing"
        :height="255"
        :width="255"
        :read-only="true" />
      <pc-drawing-pad
        v-model="normalizedDrawing"
        :height="255"
        :width="255"
        :read-only="true" />
    </div>
    <h1>guess: {{ word }}</h1>
    <pc-button
      icon="right"
      label="Start"
      @click="start()" />
  </div>
</template>

<script>
/* eslint-disable */
import PcButton from '~/components/button.vue';
import PcDrawingPad from '~/components/drawing-pad.vue';
import axios from 'axios';

import { throttle } from 'lodash';

const guess = throttle(async (pad, drawing) => {
  const result = await axios.post('http://localhost:4000/api/guess', { drawing });
  pad.normalizedDrawing = result.data.normalizedDrawing;
  pad.word = result.data.word;
}, 100);

export default {
  components: {
    PcButton,
    PcDrawingPad,
  },

  data() {
    return {
      title: 'Pictionary',
      drawing: [],
      normalizedDrawing: [[], []],
      word: '',
    };
  },

  watch: {
    drawing(drawing) {
      console.log(drawing)
      guess(this, drawing);
    },
  },
  
  methods: {
    undo() {
      this.$refs.pad.undo();
    },
    
    clear() {
      this.$refs.pad.clear();
    },
  },
};
</script>

<style>
.pc-start {
  align-items: center;
  display: flex;
  flex-direction: column;
}

.pc-brand {
  align-items: center;
  display: flex;
  margin: 50px auto;
}
</style>
