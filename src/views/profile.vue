<template>
    <div class="profile">
      <h2>个人中心</h2>
      <el-form :model="form" label-width="80px">
        <el-form-item label="用户名">
          <el-input v-model="form.username" disabled />
        </el-form-item>
        <el-form-item label="邮箱">
          <el-input v-model="form.email" />
        </el-form-item>
        <el-form-item label="头像">
          <el-upload
            class="avatar-uploader"
            action="/api/upload/avatar"
            :show-file-list="false"
            :on-success="handleAvatarSuccess"
            :before-upload="beforeAvatarUpload">
            <img v-if="form.avatar" :src="form.avatar" class="avatar">
            <i v-else class="el-icon-plus avatar-uploader-icon"></i>
          </el-upload>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="updateProfile">保存</el-button>
        </el-form-item>
      </el-form>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  export default {
    data() {
      return {
        form: {
          username: '',
          email: '',
          avatar: ''
        },
      };
    },
    created() {
      this.fetchProfile();
    },
    methods: {
      fetchProfile() {
        const userId = this.$route.query.id;
      axios.get(`http://localhost:8088/getUserById?id=${userId}`).then(response => {
        const user = response.data.data;
        this.form.username = user.username;
        this.form.email = user.email;
        this.form.avatar = user.avatar;
      }).catch(error => {
        this.$message.error('获取用户详情失败');
      });
      },
      updateProfile() {
        axios.post('http://localhost:8088/updateUserProfile', this.form).then(response => {
        this.$message.success('保存成功');
      }).catch(error => {
        this.$message.error('保存失败');
      });
    },
      handleAvatarSuccess(res, file) {
        this.form.avatar = URL.createObjectURL(file.raw);
      },
      beforeAvatarUpload(file) {
        const isJPG = file.type === 'image/jpeg';
        const isPNG = file.type === 'image/png';
        const isLt2M = file.size / 1024 / 1024 < 2;
  
        if (!isJPG && !isPNG) {
          this.$message.error('上传头像图片只能是 JPG 或 PNG 格式!');
        }
        if (!isLt2M) {
          this.$message.error('上传头像图片大小不能超过 2MB!');
        }
        return (isJPG || isPNG) && isLt2M;
      }
    }
  };
  </script>
  
  <style scoped>
  .profile {
    padding: 20px;
  }
  .avatar-uploader {
    display: inline-block;
  }
  .avatar-uploader .el-upload {
    border: 1px dashed #d9d9d9;
    border-radius: 6px;
    cursor: pointer;
    position: relative;
    overflow: hidden;
  }
  .avatar-uploader .el-upload:hover {
    border-color: #409EFF;
  }
  .avatar-uploader-icon {
    font-size: 28px;
    color: #8c939d;
    width: 178px;
    height: 178px;
    line-height: 178px;
    text-align: center;
  }
  .avatar {
    width: 178px;
    height: 178px;
    display: block;
  }
  </style>
  

   <!-- <template>
    <div class="profile">
        <h2>个人中心</h2>
        <el-form :model="form" label-width="80px">
            <el-form-item label="用户名">
                <el-input v-model="form.username" disabled />
            </el-form-item>
            <el-form-item label="邮箱">
                <el-input v-model="form.email" />
            </el-form-item>
            <el-form-item label="头像">
                <el-upload
                    class="avatar-uploader"
                    action="/api/upload/avatar"
                    :show-file-list="false"
                    :on-success="handleAvatarSuccess"
                    :before-upload="beforeAvatarUpload">
                    <img v-if="form.avatar" :src="form.avatar" class="avatar">
                    <i v-else class="el-icon-plus avatar-uploader-icon"></i>
                </el-upload>
            </el-form-item>
            <el-form-item>
                <el-button type="primary" @click="updateProfile">保存</el-button>
            </el-form-item>
        </el-form>
    </div>
</template>

<script>
import axios from 'axios'

export default {
    data() {
        return {
            form: {
                username: '',
                email: '',
                avatar: ''
            }
        };
    },
    created() {
        this.fetchProfile();
    },
    methods: {
        fetchProfile() {
            const userId = this.$route.query.id;
            axios.get(`http://localhost:8081/getUserById?id=${userId}`).then(response => {
                const user = response.data.data;
                this.form.username = user.username;
                this.form.email = user.email;
                this.form.avatar = user.avatar;
            }).catch(error => {
                this.$message.error('获取用户详情失败');
            });
        },
        updateProfile() {
            axios.post('http://localhost:808/updateUserProfile', this.form).then(response => {
                this.$message.success('保存成功');
            }).catch(error => {
                this.$message.error('保存失败');
            });
        },
        handleAvatarSuccess(res, file) {
            this.form.avatar = URL.createObjectURL(file.raw);
        },
        beforeAvatarUpload(file) {
            const isJPG = file.type === 'image/jpeg';
            const isPNG = file.type === 'image/png';
            const isLt2M = file.size / 1024 / 1024 < 2;

            if (!isJPG && !isPNG) {
                this.$message.error('上传头像图片只能是 JPG 或 PNG 格式!');
            }
            if (!isLt2M) {
                this.$message.error('上传头像图片大小不能超过 2MB!');
            }
            return (isJPG || isPNG) && isLt2M;
        }
    }
};
</script>

<style scoped>
.profile {
    padding: 20px;
}
.avatar-uploader {
    display: inline-block;
}
.avatar-uploader .el-upload {
    border: 1px dashed #d9d9d9;
    border-radius: 6px;
    cursor: pointer;
    position: relative;
    overflow: hidden;
}
.avatar-uploader .el-upload:hover {
    border-color: #409EFF;
}
.avatar-uploader-icon {
    font-size: 28px;
    color: #8c939d;
    width: 178px;
    height: 178px;
    line-height: 178px;
    text-align: center;
}
.avatar {
    width: 178px;
    height: 178px;
    display: block;
}
</style> -->
