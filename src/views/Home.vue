<template>
  <div class="home">

      <section class="banner">
          <div class="banner__import">
            <i class="el-icon-wallet"></i><br>
            <el-button>导入账号</el-button>
          </div>
      </section>

      <section class="user">
          <div v-for="(item, key) in users" :key="key" class="user__item">
              <div class="user__avatar">
                  <i class="el-icon-user"></i>
              </div>
              <i class="el-icon-delete"></i>
              <div class="user__name">{{item.name}}</div>
              <div class="user__money">{{item.money}}</div>
              <div class="user__token">AUU</div>
              <div class="user__address">{{item.address}}</div>
          </div>
      </section>

  </div>
</template>

<script>

export default {
  name: 'app',
  data() {
      return {
        users: [
            {
                name: 'test',
                money: 10000,
                address: 'fsdfasfa'
            },
            {
                name: 'test',
                money: 10000,
                address: 'fsdfasfa'
            },
            {
                name: 'test',
                money: 10000,
                address: 'fsdfasfa'
            },
            {
                name: 'test',
                money: 10000,
                address: 'fsdfasfa'
            },
            {
                name: 'test',
                money: 10000,
                address: 'fsdfasfa'
            }
        ]
      }
  }
}
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
    padding: 100px 15px 0;

    &__item {
        position: relative;
        padding-top: 55px;
        margin: 10px;
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
            content: '';
            display: block;
            position: absolute;
            width: 0;
            height: 0;
            right:50px;
            border-width: 43.3px 25px;
            border-style: solid;
            border-color: transparent #4d4b91 transparent transparent;
        }
        &::after {
            content: '';
            display: block;
            position: absolute;
            width: 0;
            height: 0;
            left:50px;
            border-width: 43.3px 25px;
            border-style: solid;
            border-color: transparent transparent transparent #4d4b91;
            top:0;
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
        font-size: 12px;
        color: #9ea1a2;
    }
}
</style>