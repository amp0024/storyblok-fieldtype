<template>
  <div class="blok__comp-detail">
    <p v-if="!augmentedImage">No image selected, please upload an image first.</p>
    <template v-else>
      <p>
        <a
          class="detail-picker__remove"
          @click="$emit('remove')"
        >
          <i class="uk-icon-trash uk-margin-small-left"></i>
        </a>
        Click on the image to set a marker for the detail position.
      </p>
      <div class="detail-picker">
        <img
          :src="augmentedImage.replace('a.storyblok.com', 'img2.storyblok.com/300x0')"
          @click="setCoordinates"
        >
        <span
          :style="`top:${y}%;left:${x}%;`"
          class="detail-picker__marker"
        ></span>
      </div>
      <div class="uk-flex tree__form-group">
        <div>
          <label class="form__topic">
            X %
            <input
              v-model="x"
              class="uk-form-small"
              @input="updateValue"
            >
          </label>
        </div>

        <div class="uk-margin-small-left">
          <label class="form__topic">
            Y %
            <input
              v-model="y"
              class="uk-form-small"
              @input="updateValue"
            >
          </label>
        </div>
      </div>

      <div class="tree__form-group">
        <label
          :for="`title-${_uid}`"
          class="form__topic"
        >
          Title
        </label>
        <input
          v-model="title"
          :id="`title-${_uid}`"
          class="uk-width-1-1"
          @input="updateValue"
        >
      </div>

      <div class="tree__form-group">
        <label
          :for="`text-${_uid}`"
          class="form__topic"
        >
          Text
        </label>
        <textarea
          v-model="text"
          :id="`text-${_uid}`"
          class="uk-width-1-1"
          rows="5"
          @input="updateValue"
        ></textarea>
      </div>

      <div class="tree__form-group">
        <span class="form__topic">
          Image
        </span>
        <image-upload
          v-model="image"
          @input="updateValue"
        ></image-upload>
      </div>
    </template>
  </div>
</template>

<script>
import ImageUpload from './ImageUpload.vue';

export default {
  components: {
    ImageUpload,
  },
  props: {
    augmentedImage: {
      type: String,
      required: true,
    },
    value: {
      type: Object,
      required: true,
    },
  },
  data() {
    // We use the values of the `value`
    // property as the initial values
    // of the fields of the component.
    return {
      image: this.value.image,
      text: this.value.text,
      title: this.value.title,
      x: this.value.x,
      y: this.value.y,
    };
  },
  methods: {
    setCoordinates(e) {
      const { height, width } = e.target;

      this.x = parseFloat((e.offsetX / (width / 100)).toFixed(2));
      this.y = parseFloat((e.offsetY / (height / 100)).toFixed(2));

      this.updateValue();
    },
    updateValue() {
      this.$emit('input', {
        image: this.image,
        text: this.text,
        title: this.title,
        x: this.x,
        y: this.y,
      });
    },
  },
};
</script>