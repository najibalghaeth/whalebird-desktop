<template>
<div id="authorize">
  <el-header>
    <el-row>
      <el-col :span="24" class="close">
        <el-button type="text" icon="el-icon-close" @click="close" class="close-button">
        </el-button>
      </el-col>
    </el-row>
  </el-header>
  <el-container>
    <el-form ref="form" :model="authorizeForm" label-width="120px" label-position="top" class="authorize-form" v-on:submit.prevent="authorizeSubmit">
      <el-form-item label="Please paste authorization code from your browser:">
        <el-input v-model="authorizeForm.code"></el-input>
      </el-form-item>
      <!-- Dummy form to guard submitting with enter -->
      <el-form-item class="hidden">
        <el-input></el-input>
      </el-form-item>
      <el-form-item class="submit">
        <el-button type="primary" @click="authorizeSubmit">Submit</el-button>
      </el-form-item>
    </el-form>
  </el-container>
</div>
</template>

<script>
export default {
  name: 'authorize',
  data () {
    return {
      authorizeForm: {
        code: ''
      }
    }
  },
  methods: {
    authorizeSubmit () {
      this.$store.dispatch('Authorize/submit', this.authorizeForm.code)
        .then((id) => {
          this.$router.push({ path: `/${id}/home` })
        })
        .catch(() => {
          this.$message({
            message: 'Could not authorize the code',
            type: 'error'
          })
        })
    },
    close () {
      return this.$router.push({ path: '/' })
    }
  }
}
</script>

<style lang="scss" scoped>
#authorize /deep/ {
  background-color: #292f3f;
  color: #ffffff;
  text-align: center;
  min-height: 100%;

  .close {
    text-align: right;

    .close-button {
      font-size: 24px;
    }
  }

  .authorize-form {
    width: 500px;
    margin: 0 auto;
  }

  .el-form-item__label {
    color: #f0f3f9;
  }

  .el-input__inner {
    background-color: #373d48;
    color: #ffffff;
    border: 0;
  }

  .hidden {
    display: none;
  }
}
</style>
