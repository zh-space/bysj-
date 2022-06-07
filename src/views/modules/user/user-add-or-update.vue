<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
    <el-form-item label="个性签名" prop="autograph">
      <el-input v-model="dataForm.autograph" placeholder="个性签名"></el-input>
    </el-form-item>
    <el-form-item label="头像" prop="headPortrait">
      <el-input v-model="dataForm.headPortrait" placeholder="头像"></el-input>
    </el-form-item>
    <el-form-item label="用户名" prop="username">
      <el-input v-model="dataForm.username" placeholder=""></el-input>
    </el-form-item>
    <el-form-item label="密码" prop="password">
      <el-input v-model="dataForm.password" placeholder=""></el-input>
    </el-form-item>
    <el-form-item label="性别" prop="gender">
      <el-input v-model="dataForm.gender" placeholder=""></el-input>
    </el-form-item>
    <el-form-item label="年龄" prop="age">
      <el-input v-model="dataForm.age" placeholder=""></el-input>
    </el-form-item>
    <el-form-item label="邮箱" prop="email">
      <el-input v-model="dataForm.email" placeholder=""></el-input>
    </el-form-item>
     <el-form-item label="账号状态" prop="state">
      <el-input v-model="dataForm.state" placeholder=""></el-input>
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
          id: 0,
          autograph: '',
          headPortrait: '',
          username: '',
          password: '',
          gender: '',
          age: '',
          email: '',
          birthday: '',
          state: '',
          createDate: '',
          updateDate: '',
          disableDate: ''
        },
        dataRule: {
          autograph: [
            { required: true, message: '个性签名不能为空', trigger: 'blur' }
          ],
          headPortrait: [
            { required: true, message: '头像不能为空', trigger: 'blur' }
          ],
          username: [
            { required: true, message: '不能为空', trigger: 'blur' }
          ],
          password: [
            { required: true, message: '不能为空', trigger: 'blur' }
          ],
          gender: [
            { required: true, message: '不能为空', trigger: 'blur' }
          ],
          age: [
            { required: true, message: '不能为空', trigger: 'blur' }
          ],
          birthday: [
            { required: true, message: '不能为空', trigger: 'blur' }
          ],
          state: [
            { required: true, message: '不能为空', trigger: 'blur' }
          ],
          createDate: [
            { required: true, message: '不能为空', trigger: 'blur' }
          ],
          updateDate: [
            { required: true, message: '不能为空', trigger: 'blur' }
          ],
          disableDate: [
            { required: true, message: '不能为空', trigger: 'blur' }
          ]
        }
      }
    },
    methods: {
      init (id) {
        this.dataForm.id = id || 0
        this.visible = true
        this.$nextTick(() => {
          this.$refs['dataForm'].resetFields()
          if (this.dataForm.id) {
            this.$http({
              url: this.$http.adornUrl(`/giao/user/info/${this.dataForm.id}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.autograph = data.user.autograph
                this.dataForm.headPortrait = data.user.headPortrait
                this.dataForm.username = data.user.username
                this.dataForm.password = data.user.password
                this.dataForm.gender = data.user.gender
                this.dataForm.age = data.user.age
                this.dataForm.phone = data.user.phone
                this.dataForm.birthday = data.user.birthday
                this.dataForm.state = data.user.state
                this.dataForm.createDate = data.user.createDate
                this.dataForm.updateDate = data.user.updateDate
                this.dataForm.disableDate = data.user.disableDate
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
              url: this.$http.adornUrl(`/giao/user/${!this.dataForm.id ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'id': this.dataForm.id || undefined,
                'autograph': this.dataForm.autograph,
                'headPortrait': this.dataForm.headPortrait,
                'username': this.dataForm.username,
                'password': this.dataForm.password,
                'gender': this.dataForm.gender,
                'age': this.dataForm.age,
                'phone': this.dataForm.phone,
                'birthday': this.dataForm.birthday,
                'state': this.dataForm.state,
                'createDate': this.dataForm.createDate,
                'updateDate': this.dataForm.updateDate,
                'disableDate': this.dataForm.disableDate
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
