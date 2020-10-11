<template>
  <el-container class="container cell">
    <el-row :gutter="20">
      <el-col class="review" :xs="24" :sm="12" :md="8" :lg="6" :xl="6" v-for="(review, index) in reviews" :key="index">
        <el-card class="review__card" shadow="hover">
          <div class="review__buttons" slot="header">
            <el-button size="mini" type="success" icon="el-icon-edit" @click="review.edited = true">Edit</el-button>
            <el-button size="mini" type="danger" icon="el-icon-remove" @click="removeReview(review.id)">Remove</el-button>
          </div>
          <p>{{review.id}}</p>
          <h3 class="review__title">{{ review.title }}</h3>
          <p class="review__content">{{ review.content }}</p>
          <img class="review__image" v-if="review.image_url" :src="review.image_url" alt="">
        </el-card>
        <el-dialog :title="review.title" :visible="review.edited">
          <el-input v-model="review.title" placeholder="Title" clearable class="input"></el-input>
          <el-input type="textarea" v-model="review.content" placeholder="Content" clearable class="input"></el-input>
          <el-button type="success" @click="review.edited = false">Save</el-button>
        </el-dialog>
      </el-col>
      <el-col :xs="24" :sm="12" :md="8" :lg="6" :xl="6">
        <el-button class="add_button" type="danger" icon="el-icon-circle-plus" @click="visible = true">Add</el-button>
        <el-dialog title="Add new review" :visible="visible">
          <el-input v-model="newReview.title" placeholder="Title" clearable class="input"></el-input>
          <el-input type="textarea" v-model="newReview.content" placeholder="Content" clearable class="input"></el-input>
          <label for="file_upload">Upload image: </label>
          <input type="file" id="file_upload" ref="file" @change="upload()">
          <el-button type="success" icon="el-icon-upload" @click="click">Upload</el-button>
          <p>{{file.name}}</p>
          <br>
          <el-button type="success" @click="createNewReview()">Save</el-button>
        </el-dialog>
      </el-col>
    </el-row>
  </el-container>
</template>

<script>
import axios from "axios"
export default {
  name: 'Home',
  data: function () {
    return {
      lastId: 5,
      reviews: [
        {
          id: 1,
          title: "Bla bla bla bla",
          content: "Another bla bla, but here is a content.",
          edited: false,
          image_url: "https://i.ibb.co/ZBkM9sd/forest-digital-art-wallpaper-2560-1440.jpg"
        },
        {
          id: 2,
          title: "Bla bla bla bla",
          content: "Another bla bla, but here is a content.",
          edited: false,
          image_url: "https://i.ibb.co/ZBkM9sd/forest-digital-art-wallpaper-2560-1440.jpg"
        },
        {
          id: 3,
          title: "Bla bla bla bla",
          content: "Another bla bla, but here is a content.",
          edited: false,
          image_url: "https://i.ibb.co/ZBkM9sd/forest-digital-art-wallpaper-2560-1440.jpg"
        },
        {
          id: 4,
          title: "Bla bla bla bla",
          content: "Another bla bla, but here is a content.",
          edited: false,
          image_url: "https://i.ibb.co/ZBkM9sd/forest-digital-art-wallpaper-2560-1440.jpg"
        },
      ],
      newReview: {
        id: this.lastId,
        title: "",
        content: "",
        edited: false,
        image_url: ""
      },
      visible: false,
      file: ''
    }
  },
  methods: {
    removeReview(id) {
      let reviewFromList = this.reviews.find(elem => elem.id === id)
      let index = this.reviews.indexOf(reviewFromList)
      this.reviews.splice(index, 1)
    },
    reset() {
      this.newReview = {}
      this.file = ''
      this.visible = false
    },
    async createNewReview() {
      let formData = new FormData()
      formData.append("key", "c071bfc4e4b6565492db460f81144f9d")
      formData.append("image", this.file)
      await axios.post("https://api.imgbb.com/1/upload", formData, {headers: {'Content-Type': 'multipart/form-data'}}).then(response => {
        this.newReview.image_url = response.data.data.url
      })
      this.newReview.id = this.lastId
      this.lastId++;
      this.reviews.push(this.newReview)
      this.reset()
    },
    click() {
      this.$refs.file.click()
    },
    upload() {
      this.file = this.$refs.file.files[0]
    }
  }
}
</script>

<style lang="scss">
.container {
  margin: 10px auto;
  #file_upload {
    display: none;
  }
  .add_button {
    margin: .5rem 0;
  }
  .review {
    margin: .5rem 0;
    .review__buttons {
      display: flex;
      justify-content: space-between;
    }
    .review__title {
      font-size: 16px;
      margin: 10px 0;
      &:after {
        content: ".";
      }
    }
    .review__image {
      max-width: 100%;
    }
  }
  .input {
    width: 100%;
    margin: .5rem 0;
  }
}
</style>