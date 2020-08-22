<template>
  <div id="uploads">
    <div v-for="(upload,index) in uploads" :key="index" class="upload">
      <div class="upload_title">{{ upload.title }}</div>
      <div class="upload_preview">
        <img v-for="(preview,index) in upload.preview_list" :key="index" :src="preview" alt="" class="preview_img">
      </div>
      <div @click="pickImage" class="upload_btn" :data-type="upload.type">{{ upload.btnText }}</div>
      <input @change="previewMultiImage" :data-type="upload.type" type="file" accept="image/jpg, image/png" multiple="multiple" />
    </div>
  </div>
</template>

<script>
export default {
  name: "Uploads",
  props: {
    msg: String,
  },
  data: function () {
    return {
      uploads: [
        { title: "服務前造型", btnText: "上傳圖片", type: "before", preview_list: []},
        { title: "服務前造型", btnText: "上傳圖片", type: "after", preview_list: [] },
      ],
      image_list: [],
    };
  },
  methods: {
    pickImage: function (event) {
      const uploadType = event.target.dataset.type
      if(uploadType === 'before'){
        this.uploads[0].preview_list = []
      }else {
        this.uploads[1].preview_list = []
      }
      event.target.nextElementSibling.click();
    },
    previewMultiImage: function (event) {
      const input = event.target;
      const inputType = event.target.dataset.type;
      let count = input.files.length;
      let index = 0;
      if (input.files) {
        while (count--) {
          const reader = new FileReader();
          reader.onload = (e) => {
            if(inputType === 'before'){
              this.uploads[0].preview_list.push(e.target.result);
            }else {
              this.uploads[1].preview_list.push(e.target.result);
            }
          };
          this.image_list.push(input.files[index]);
          reader.readAsDataURL(input.files[index]);
          index++;
        }
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
#uploads {
  display: flex;
  flex-direction: column;
  max-width: 500px;
  background-color: #f2f2f4;
  margin: 0 auto;
  padding: 10px;
  .upload {
    display: flex;
    justify-content: space-between;
    align-items: flex-start;
    height: 50px;
    border-top: 1px solid #e0e0e0;
    padding: 10px 0;

    &:last-child {
      border-bottom: 1px solid #e0e0e0;
    }

    &_title {
      color: #727272;
    }
    &_preview {
      flex: 1 calc(100% - 200px);
      display: flex;
      overflow: hidden;
      .preview_img {
        height: 50px;
      }
    }
    &_btn {
      background-color: #020022;
      border-radius: 10px;
      color: #ffffff;
      padding: 2px 20px;
    }
    input {
      display: none;
    }
  }
}
</style>
