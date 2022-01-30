<template>
  <div class="main-form bg-[#F2F5C8] min-h-screen">
    <div class="container mx-auto py-6">
      <h2 class="text-2xl text-center mb-4 text-[#219F94]">BIO Data</h2>
      <div class="mt-10 sm:mt-0">
        <div class="mt-5 md:mt-0 md:col-span-2">
          <form @submit.prevent="handleSubmit">
            <div class="shadow overflow-hidden sm:rounded-md">
              <div class="px-4 py-5 bg-white sm:p-6">
                <div class="grid grid-cols-6 gap-6">
                  <div class="col-span-3">
                    <label
                      for="first-name"
                      class="block text-sm font-medium text-gray-700"
                      >First name</label
                    >
                    <input
                      type="text"
                      name="first-name"
                      v-model="userInfo.fname"
                      class="mt-1 p-2 block w-full shadow-sm border border-gray-300 rounded-md"
                    />
                    <span v-if="validation.hasError('userInfo.fname')">
                      <p class="input-error mb-0">
                        {{ validation.firstError("userInfo.fname") }}
                      </p>
                    </span>
                  </div>
                  <div class="col-span-3">
                    <label
                      for="last-name"
                      class="block text-sm font-medium text-gray-700"
                      >Last name</label
                    >
                    <input
                      type="text"
                      name="last-name"
                      v-model="userInfo.lname"
                      class="mt-1 p-2 block w-full shadow-sm border border-gray-300 rounded-md"
                    />
                    <span v-if="validation.hasError('userInfo.fname')">
                      <p class="input-error mb-0">
                        {{ validation.firstError("userInfo.fname") }}
                      </p>
                    </span>
                  </div>
                  <div class="col-span-4">
                    <label
                      for="email-address"
                      class="block text-sm font-medium text-gray-700"
                      >Email address</label
                    >
                    <input
                      type="email"
                      name="email-address"
                      v-model="userInfo.email"
                      class="mt-1 p-2 block w-full shadow-sm border border-gray-300 rounded-md"
                    />
                    <span v-if="validation.hasError('userInfo.fname')">
                      <p class="input-error mb-0">
                        {{ validation.firstError("userInfo.fname") }}
                      </p>
                    </span>
                  </div>
                  <div class="col-span-3">
                    <label
                      for="country"
                      class="block text-sm font-medium text-gray-700"
                      >Country</label
                    >
                    <select
                      v-model="userInfo.country"
                      name="country"
                      class="mt-1 p-2 block w-full py-2 px-3 border border-gray-300 bg-white rounded-md shadow-sm"
                    >
                      <option>India</option>
                      <option>Canada</option>
                      <option>Mexico</option>
                    </select>
                  </div>

                  <div class="col-span-6">
                    <label class="block text-sm font-medium text-gray-700">
                      Photo
                    </label>
                    <div class="mt-1 flex items-center">
                      <div class="upload-example">
                        <div class="flex">
                          <Cropper
                            ref="cropper"
                            class="upload-example-cropper cropper mb-4"
                            :src="previewImage"
                            :stencil-props="{
                              aspectRatio: 1 / 1,
                            }"
                            @change="change"
                            v-if="previewImage && !showImage"
                          />
                          <img
                            :src="userInfo.photo"
                            alt=""
                            class="h-40 w-40"
                            v-if="showImage"
                          />
                        </div>
                        <div class="button-wrapper mt-4">
                          <button
                            class="button bg-white py-2 px-3 border border-gray-300 rounded-md shadow-sm text-sm leading-4 font-medium text-gray-500"
                          >
                            <input
                              type="file"
                              accept="image/*"
                              @change="uploadImage"
                            />
                          </button>
                          <button
                            type="button"
                            class="bg-[#219F94] py-2 px-3 border border-gray-300 rounded-md shadow-sm text-sm leading-4 font-medium text-white"
                            @click="showImage = true"
                          >
                            Save Image
                          </button>
                        </div>
                      </div>
                    </div>
                  </div>
                  <div class="col-span-6">
                    <label class="block text-sm font-medium text-gray-700 my-3">
                      Write about yourself
                    </label>
                    <Editor
                      api-key="2l1qico6ishl7leme3f8zwsw5h07jf87cb18i3gcaxt9haxg"
                      :init="{
                        height: 500,
                        menubar: true,
                        plugins: [
                          'advlist autolink lists link image charmap print preview anchor',
                          'searchreplace visualblocks code fullscreen',
                          'insertdatetime media table paste code help wordcount',
                        ],
                        toolbar:
                          'undo redo | formatselect | bold italic backcolor | \
           alignleft aligncenter alignright alignjustify | \
           bullist numlist outdent indent | removeformat | help',
                      }"
                      v-model="userInfo.bio"
                    />
                  </div>
                </div>
              </div>
              <div class="px-4 py-3 bg-gray-50 text-right sm:px-6">
                <button
                  type="submit"
                  class="inline-flex justify-center py-2 px-4 border border-transparent shadow-sm text-sm font-medium rounded-md text-white bg-[#219F94]"
                >
                  Save
                </button>
              </div>
            </div>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Editor from "@tinymce/tinymce-vue";
import { Cropper } from "vue-advanced-cropper";
import "vue-advanced-cropper/dist/style.css";
import { Validator } from "simple-vue-validator";

export default {
  data() {
    return {
      editorValue: "",
      showImage: false,
      previewImage: null,
      userInfo: {
        fname: "",
        lname: "",
        email: "",
        country: "",
        photo: "",
        bio: "",
      },
    };
  },
  validators: {
    "userInfo.fname": function (value) {
      return Validator.value(value).required();
    },
    "userInfo.lname": function (value) {
      return Validator.value(value).required();
    },
    "userInfo.email": function (value) {
      return Validator.value(value).required().email();
    },
  },
  components: {
    Editor,
    Cropper,
  },
  methods: {
    change({ canvas }) {
      this.userInfo.photo = canvas.toDataURL();
    },
    uploadImage(e) {
      const image = e.target.files[0];
      const reader = new FileReader();
      reader.readAsDataURL(image);
      reader.onload = (e) => {
        this.previewImage = e.target.result;
      };
    },
    async handleSubmit() {
      var validation = await this.$validate();
      if (validation) {
        console.log(this.userInfo);
      }
    },
  },
};
</script>

<style scoped>
.cropper {
  height: 300px;
  width: 300px;
  background-color: #fff;
}

.input-error {
  color: red;
}
</style>
