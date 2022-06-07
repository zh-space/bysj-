<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
    <el-form-item label="帖子标题" prop="title">
      <el-input v-model="dataForm.title" placeholder="帖子标题"></el-input>
    </el-form-item>
    <el-form-item label="帖子内容" prop="article">
      <el-input v-model="dataForm.article" placeholder="帖子内容"></el-input>
    </el-form-item>
    <el-form-item label="帖子图片" prop="image">
      <el-input v-model="dataForm.image" placeholder="帖子图片"></el-input>
    </el-form-item>
    <el-form-item label="发表时间" prop="createDate">
      <el-input v-model="dataForm.createDate" placeholder=""></el-input>
    </el-form-item>
    <el-form-item label="帖子类型" prop="type">
      <el-input v-model="dataForm.type" placeholder="帖子类型"></el-input>
    </el-form-item>
    <el-form-item label="帖子点赞数" prop="thumbs">
      <el-input v-model="dataForm.thumbs" placeholder="帖子点赞数"></el-input>
    </el-form-item>
    </el-form>
    <span slot="footer" class="dialog-footer">
      <el-button @click="visible = false">取消</el-button>
      <el-button type="primary" @click="dataFormSubmit()">确定</el-button>
    </span>
  </el-dialog>
</template>

<script>
  export default {
    data () {
      return {
        visible: false,
        dataForm: {
          postsId: 0,
          title: '',
          article: '',
          image: '',
          createDate: '',
          type: '',
          thumbs: ''
        },
        dataRule: {
        
          type: [
            { required: true, message: '帖子类型不能为空', trigger: 'blur' }
          ],
          article: [
            { required: true, message: '帖子内容不能为空', trigger: 'blur' }
          ]
        }
      }
    },
    methods: {
      init (id) {
        this.dataForm.postsId = id || 0
        this.visible = true
        this.$nextTick(() => {
          this.$refs['dataForm'].resetFields()
          if (this.dataForm.postsId) {
            this.$http({
              url: this.$http.adornUrl(`/giao/user/posts/info/${this.dataForm.postsId}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.title = data.posts.title
                this.dataForm.article = data.posts.article
                this.dataForm.image = data.posts.image
                this.dataForm.createDate = data.posts.createDate
                this.dataForm.type = data.posts.type
                this.dataForm.thumbs = data.posts.thumbs
              }
            })
          }
        })
      },
      // 表单提交
      dataFormSubmit () {
        this.$refs['dataForm'].validate((valid) => {
          if (valid) {
            this.$http({
              url: this.$http.adornUrl(`/giao/user/posts/${!this.dataForm.postsId ? 'save1' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'postsId': this.dataForm.postsId || undefined,
                'title': this.dataForm.title,
                'article': this.dataForm.article,
                'image': this.dataForm.image,
                'createDate': this.dataForm.createDate,
                'type': this.dataForm.type,
                'thumbs': this.dataForm.thumbs
              })
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.$message({
                  message: '操作成功',
                  type: 'success',
                  duration: 1500,
                  onClose: () => {
                    this.visible = false
                    this.$emit('refreshDataList')
                  }
                })
              } else {
                this.$message.error(data.msg)
              }
            })
          }
        })
      }
    }
  }
</script>
