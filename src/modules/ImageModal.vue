<template>
  <div class="modal-container" v-if="isVisible" >
    <div class="modal-content">
      <span class="exit" @click="close"><i class="fa fa-close"></i></span>
      <div class="move-back" v-if="currentIndex > 0" @click="showPrevious">
        <i class="fa fa-angle-left"></i>
      </div>
      <div class="move-forward" v-if="currentIndex < (list.length - 1)" @click="showNext">
        <i class="fa fa-angle-right"></i>
      </div>
      <div class="main-image-container">
        <img :src="currentImage.webformatURL" :alt="currentImage.tags" class="image img-fluid">
      </div>
      <div class="preview">
        <div class="thumb-container">
          <img :src="list[(currentIndex - 1)].previewURL" :alt="list[(currentIndex - 1)].tags" class="image img-fluid" v-if="currentIndex > 0">
        </div>
        <div class="thumb-container details">
          <span>
            <i class="fa fa-thumbs-up"></i>
            {{currentImage.likes}}
          </span>
          <span>
            <i class="fa fa-heart"></i>
            {{currentImage.favorites}}
          </span>
          <span>
            <i class="fa fa-file-image-o"></i>
            {{`${currentImage.imageHeight} x ${currentImage.imageWidth}`}}
          </span>
          <span>
            <i class="fa fa-download"></i>
            {{currentImage.downloads}}
          </span>
        </div>
        <div class="thumb-container">
          <img :src="list[(currentIndex + 1)].previewURL" :alt="list[(currentIndex + 1)].tags" class="image img-fluid" v-if="currentIndex < (list.length - 1)">
        </div>
      </div>
      <div class="operation">
        <a :href="currentImage.pageURL" target="_blank"><button class="btn btn-primary">Download</button></a>
        </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ImageModal',
  props: {
    isVisible: Boolean,
    startIndex: Number,
    list: Array
  },
  data () {
    return {
      currentIndex: null
    }
  },
  computed: {
    currentImage () {
      return this.list[this.currentIndex]
    }
  },
  methods: {
    close () {
      this.$emit('close')
    },
    showNext () {
      this.currentIndex++
    },
    showPrevious () {
      this.currentIndex--
    }
  },
  watch: {
    isVisible (yes) {
      if (yes) {
        this.currentIndex = this.startIndex
      } else {
        this.currentIndex = null
      }
    }
  }
}
</script>

<style lang="scss" scoped>
  .modal-container {
    position: absolute;
    z-index: 9998;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, .9);
    display: flex;
    justify-content: center;
    align-items: center;
    .modal-content {
      position: relative;
      width: 50%;
      max-height: 90%;
      background-color: #222;
      display: flex;
      flex-direction: column;
      align-items: center;  
      padding: 20px;
      .exit {
        position: absolute;
        right: -25px;
        top: -25px;
        color: #ddd;
        font-size: 24px;
        cursor: pointer;
      }
      .move-back,
      .move-forward {
        width: 60px;
        height: 60px;
        border-radius: 50%;
        background-color: #007bff;
        font-size: 60px;
        color: #ddd;
        position: absolute;
        top: 50%;
        transform: translateY(-50%);
        cursor: pointer;
        i {
          position: absolute;
          top: 47%;
          transform: translate(-50%, -50%);
        }
      }
      .move-back {
        left: -65px;
        i {
          left: 45%;
        }
      }
      .move-forward {
        right: -65px;
        i {
          left: 55%;
        }
      }
      .main-image-container {
        width: 100%;
        overflow: hidden;
        margin-bottom: 20px;
      }
      .preview {
        width: 100%;
        display: flex;
        justify-content: space-between;
        margin-bottom: 20px;
        .thumb-container {
          width: 30%;
          height: 100px;
          overflow: hidden;
        }
        .details {
          display: flex;
          flex-direction: column;
          align-items: flex-start;
          justify-content: flex-end;
          color: #fff;
          font-size: 14px;
          padding: 8px;
          i {
            margin-right: 5px;
          }
        }
      }
      .operation {
        width: 100%;
        display: flex;
        justify-content: center;
        button {
          cursor: pointer;
        }
      }
    }
  }
  .image {
    width: 100%;
    height: auto;
  }
</style>
