<template>
  <div class="home">

    <section class="banner">
      <div class="banner__import">
        <i class="el-icon-wallet"></i>
        <br />
        <el-button>导入账号</el-button>
      </div>
    </section>

    <section class="user" v-loading="loading">
      <div v-for="(item, key) in users" :key="key" class="user__item">
        <div class="user__avatar">
          <i class="el-icon-user"></i>
        </div>
        <i @click="deleteUser(item.wallet.signingKey.address)" class="el-icon-delete"></i>
        <div class="user__name">{{item.name}}</div>
        <div class="user__money">{{item.money}}</div>
        <div class="user__token">ETH</div>
        <div class="user__address">{{item.wallet.signingKey.address}}</div>
      </div>
      <div @click="dialogFormVisible = true" class="user__item add">
        <i class="el-icon-plus"></i>
        <div>新建账号</div>
      </div>
    </section>

    <el-dialog title="创建账户" :visible.sync="dialogFormVisible">
      <el-form :model="form" :rules="rules" ref="ruleForm" label-width="80px" label-position="left">
        <el-form-item label="账户名称" prop="name">
          <el-input v-model="form.name"></el-input>
        </el-form-item>
        <el-form-item label="设置密码" prop="pass">
          <el-input type="password" v-model="form.pass"></el-input>
        </el-form-item>
        <el-form-item label="确认密码" prop="checkPass">
          <el-input type="password" v-model="form.checkPass"></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="submitForm('ruleForm')">确 定</el-button>
      </div>
    </el-dialog>

    

  </div>
</template>

<script>
import { ethers } from "ethers";

export default {
  name: "Home",
  data() {
    var validatePass = (rule, value, callback) => {
       if (value !== this.form.pass) {
        callback(new Error('两次输入密码不一致!'))
      } else {
        callback()
      }
    }
    return {
      users: [],
      loading: false,
      dialogFormVisible: false,
      form: {
        name: '',
        pass: '',
        checkPass: ''
      },
      rules: {
        name: { required: true, message: '请输入账户名称', trigger: 'blur' },
        pass: [
          { required: true, message: '请输入密码', trigger: 'blur' },
          { min: 6, max: 12, message: '密码6-12位', trigger: 'blur' },
        ],
        checkPass: [
          { required: true, message: '请再次输入密码', trigger: 'blur' },
          { validator: validatePass, trigger: 'blur' }
        ]
      }
    }
  },
  created() {
    // console.log(ethers)
    this.getUsers()
  },
  methods: {
    getUsers() {
      this.users = []
      let users = JSON.parse(localStorage.getItem('wallet'))
      
      if (!users) return
      if (!users.length) return

      this.loading = true
      users.map(async (el, idx) => {
        let provider = ethers.getDefaultProvider()
        let privateKey = el.wallet.signingKey.privateKey
        let wallet = new ethers.Wallet(privateKey, provider)

        let balance = await wallet.getBalance()
        if (balance) {
          el.money = ethers.utils.formatEther(balance)
          this.users.push(el)
          if (idx == users.length - 1) {
            this.loading = false
          }
        }
      })
    },
    submitForm(formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          let wallet = ethers.Wallet.createRandom()
          let obj = {
            name: this.form.name,
            pass: this.form.pass,
            wallet
          }

          let walletCache = localStorage.getItem('wallet')
          if (walletCache) {
            let walletArr = JSON.parse(walletCache)
            walletArr.push(obj)
            localStorage.setItem('wallet', JSON.stringify(walletArr))
          } else {
            localStorage.setItem('wallet', JSON.stringify([obj]))
          }

          this.$notify({
            title: '创建成功',
            message: '秘钥：' + wallet.signingKey.privateKey,
            type: 'success',
            duration: 0
          })
          this.dialogFormVisible = false
          this.$refs[formName].resetFields()
          this.getUsers()
        }
      })
    },
    deleteUser(address) {
      this.$confirm('此操作将永久删除该账户, 是否继续?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        let index = this.users.findIndex(item => item.wallet.signingKey.address == address)
        this.users.splice(index, 1)
        localStorage.setItem('wallet', JSON.stringify(this.users))
        this.$message({
          type: 'success',
          message: '删除成功!'
        })
      }).catch(() => {
        this.$message({
          type: 'info',
          message: '已取消删除'
        })
      })
    }
  }
};
</script>

<style lang="scss" scoped>
.banner {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 300px;
  background: linear-gradient(to right, #302b61, #58559f);

  &__import {
    text-align: center;

    .el-icon-wallet {
      margin-bottom: 10px;
      font-size: 100px;
      color: #fff;
    }
  }
}

.user {
  display: flex;
  flex-wrap: wrap;
  align-content: space-around;
  width: 1350px;
  margin: 0 auto;
  padding: 100px 15px;

  &__item {
    position: relative;
    padding-top: 55px;
    margin: 10px;
    margin-bottom: 50px;
    width: 250px;
    height: 200px;
    background: #fff;
    box-sizing: border-box;
    text-align: center;
    .el-icon-delete {
      position: absolute;
      top: 10px;
      right: 10px;
      color: #9ea1a2;
      cursor: pointer;
    }
    &.add {
      display: flex;
      justify-content: center;
      align-items: center;
      flex-direction: column;
      padding: 0;
      cursor: pointer;
      .el-icon-plus {
        margin-bottom: 5px;
        font-size: 50px;
        font-weight: bolder;
        color: #999c9d;
      }
    }
  }
  &__avatar {
    display: flex;
    justify-content: center;
    align-items: center;
    position: absolute;
    top: -43.3px;
    left: 50%;
    transform: translateX(-50%);
    width: 50px;
    height: 86.6px;
    background-color: #4d4b91;
    &::before {
      content: "";
      display: block;
      position: absolute;
      width: 0;
      height: 0;
      right: 50px;
      border-width: 43.3px 25px;
      border-style: solid;
      border-color: transparent #4d4b91 transparent transparent;
    }
    &::after {
      content: "";
      display: block;
      position: absolute;
      width: 0;
      height: 0;
      left: 50px;
      border-width: 43.3px 25px;
      border-style: solid;
      border-color: transparent transparent transparent #4d4b91;
      top: 0;
    }
    .el-icon-user {
      font-size: 50px;
      color: #fff;
    }
  }
  &__name {
    color: #274154;
  }
  &__money {
    font-size: 20px;
    color: #2e295e;
  }
  &__address {
    margin-top: 10px;
    padding: 0 50px;
    font-size: 12px;
    color: #9ea1a2;
    word-break: break-word;
  }
}
</style>
<style lang="scss">
.el-notification {
  &__content {
    text-align: left;
    p {
      word-break: break-all;
    }
  }
}
</style>
