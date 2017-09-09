<template>
  <div class="thumbnail-container" @click="openImage"
      @mouseenter="toggleDetails" @mouseleave="toggleDetails">
    <div class="image-container">
      <img :src="imageData.previewURL" :alt="imageData.tags"></img>
      <div class="details" v-if="showDetails">
        <!-- <span>
          <i class="fa fa-thumbs-up"></i>{{imageData.likes}}
        </span>
        <span>
          <i class="fa fa-heart"></i>{{imageData.favorites}}
        </span> -->
        <span>
          <i class="fa fa-file-image-o"></i>{{`${imageData.imageHeight} x ${imageData.imageWidth}`}}
        </span>
        <!-- <span>
          <i class="fa fa-download"></i>{{imageData.downloads}}
        </span> -->
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Thumbnail',
  props: {
    index: Number,
    imageData: Object
  },
  data () {
    return {
      showDetails: false
    }
  },
  methods: {
    toggleDetails () {
      this.showDetails = !this.showDetails
    },
    openModal () {
      this.$emit('openImage', this.index)
    }
  }
}
</script>

<style lang="scss" scoped>
  .thumbnail-container {
    cursor: pointer;
    height: 150px;
    width: 150px;
    margin: 0 10px 10px 0;
    display: flex;
    align-items: center;
    .image-container {
      position: relative;
      .details {
        display: flex;
        flex-direction: column;
        align-items: flex-start;
        justify-content: flex-end;
        background-color: rgba(0, 123, 255, .8);
        color: #fff;
        font-size: 12px;
        position: absolute;
        z-index: 2;
        top: 0;
        left: 0;
        height: 100%;
        width: 100%;
        padding: 8px;
        i {
          margin-right: 5px;
        }
      }
    }
  }
</style>
