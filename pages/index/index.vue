<template>
  <view class="container">
    <form class="login-form">
      <view class="form-group">
        <input type="text" v-model="username" placeholder="用户名" />
      </view>
      <view class="form-group">
        <input type="password" v-model="password" placeholder="密码" />
      </view>
      <view class="form-group">
        <button @click="submit">登录</button>
      </view>
    </form>
  </view>
</template>

<script>
export default {
  data() {
    return {
      username: '我是用户名',
      password: '我是密码'
    }
  },
  methods: {
    submit() {
	  alert('用户名：' + this.username + '，密码：' + this.password +'，恭喜你登录成功')
    }
  }
}
</script>

<style>
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100%;
}

.login-form {
  margin-top: 100px;
  width: 300px;
  padding: 20px;
  border: 1px solid #428fc1;
  border-radius: 4px;
}

.form-group {
  margin-bottom: 16px;
}

input {
  width: 100%;
  height: 40px;
  marin: 8px 12px;
  padding: 2px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

button {
  width: 100%;
  height: 40px;
  background-color: #007aff;
  color: #fff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
</style>
