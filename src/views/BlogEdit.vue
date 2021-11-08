<template>
  <div>
    <Header></Header>

    <div class="m-content">

      <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="100px" class="demo-ruleForm">
        <el-form-item label="title" prop="title">
          <el-input v-model="ruleForm.title"></el-input>
        </el-form-item>

        <el-form-item label="description" prop="description">
          <el-input type="textarea" v-model="ruleForm.description"></el-input>
        </el-form-item>

        <el-form-item label="content" prop="content">
          <mavon-editor v-model="ruleForm.content"></mavon-editor>
        </el-form-item>

        <el-form-item>
          <el-button type="primary" @click="submitForm('ruleForm')">submit</el-button>
          <el-button type="primary" @click="updateForm('ruleForm')">update</el-button>
          <el-button @click="resetForm('ruleForm')">reset</el-button>
        </el-form-item>
      </el-form>

    </div>



  </div>
</template>

<script>
  import Header from "../components/Header";
  export default {
    name: "BlogEdit.vue",
    components: {Header},
    data() {
      return {
        ruleForm: {
          id: '',
          title: '',
          description: '',
          content: ''
        },
        rules: {
          title: [
            { required: true, message: 'please enter a title', trigger: 'blur' },
            { min: 3, max: 25, message: 'length must between 3 and 25', trigger: 'blur' }
          ],
          description: [
            { required: true, message: 'please enter the description', trigger: 'blur' }
          ],
          content: [
            { required: true, message: 'please enter the content', trigger: 'blur' }
          ]
        }
      };
    },
    methods: {
      submitForm(formName) {
        this.$refs[formName].validate((valid) => {
          if (valid) {

            const _this = this
            this.$axios.put('/blogs', this.ruleForm, {
              headers: {
                "Authorization": localStorage.getItem("token")
              }
            }).then(res => {
              console.log(res)
              _this.$alert('success', 'alert', {
                confirmButtonText: 'confirm',
                callback: action => {
                  _this.$router.push("/blogs")
                }
              });

            })

          } else {
            console.log('error submit!!');
            return false;
          }
        });
      },
      resetForm(formName) {
        this.$refs[formName].resetFields();
      },

      updateForm(formName) {
        this.$refs[formName].validate((valid) => {
          if (valid) {

            const _this = this
            this.$axios.post('/blogs', this.ruleForm, {
              headers: {
                "Authorization": localStorage.getItem("token")
              }
            }).then(res => {
              console.log(res)
              _this.$alert('success', 'alert', {
                confirmButtonText: 'confirm',
                callback: action => {
                  _this.$router.push("/blogs")
                }
              });

            })

          } else {
            console.log('error submit!!');
            return false;
          }
        });
      }
    },
    created() {
      const blogId = this.$route.params.blogId
      console.log(blogId)
      const _this = this
      if(blogId) {
        this.$axios.get('/blog/' + blogId).then(res => {
          const blog = res.data.data
          _this.ruleForm.id = blog.id
          _this.ruleForm.title = blog.title
          _this.ruleForm.description = blog.description
          _this.ruleForm.content = blog.content
        })
      }

    }
  }
</script>

<style scoped>
  .m-content {
    text-align: center;
  }
</style>
