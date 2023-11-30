<template>
  <div class="word-container">
    <div class="personal-info-card-container">
      <div class="personal-info-card">
        <el-row :gutter="20">
          <el-col :span="24">
            <div class="personal-info">
              <p class="info-label">账号: {{ id }}</p>
              <el-divider />
              <p class="info-label">用户名: {{ username }}</p>
              <el-divider />
              <p class="info-label">邮箱: {{ email }}</p>
              <el-divider />
              <p class="info-label">年龄: {{ age }}</p>
              <el-divider />
              <p class="info-label">性别: {{ getGenderLabel(gender) }}</p>
              <el-divider />
              <p class="info-label">注册时间: {{ formatDate(created) }}</p>
            </div>
          </el-col>
        </el-row>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    nowUserId: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      id: '',
      created: '',
      email: '',
      age: '',
      username: '',
      gender: '',
    };
  },
  created() {
    console.log('当前用户id为：', this.nowUserId)
    this.getUserInfo();
  },
  methods: {
    async getUserInfo() {
      try {
        const response = await fetch('http://localhost:8080/user/get?id=' + this.nowUserId, {
          method: 'get',
          headers: {
            'Content-Type': 'application/json',
          },
        });

        if (!response.ok) {
          console.error('Error fetching user information:', response);
        }

        const data = await response.json();
        this.id = data.data.id;
        this.created = data.data.created;
        this.email = data.data.email;
        this.age = data.data.age;
        this.username = data.data.username;
        this.gender = data.data.gender;
        console.log('获取到的用户信息：', data);
        console.log('用户id：', this.id);
        console.log('name', this.username);
      } catch (error) {
        console.error('Error fetching user information:', error);
      }
    },
    formatDate(date) {
      const options = {
        year: 'numeric',
        month: '2-digit',
        day: '2-digit',
        hour12: false, // Use 24-hour format
        timeZone: 'Asia/Shanghai', // Set to the Chinese time zone
      };

      return new Date(date).toLocaleDateString('zh-CN', options);
    },

    getGenderLabel(gender) {
      return gender === 1 ? '男' : (gender === 2 ? '女' : '未知');
    },
  },
};
</script>

<style scoped>
.word-container {
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

.personal-info-card-container {
  width: 90%;
  max-width: 800px;
  display: flex;
  justify-content: center;
}

.personal-info-card {
  width: 100%;
  height: 600px;
  background: rgba(255, 255, 255, 0.8);
  border-radius: 15px;
  padding: 30px;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.2);
}

.personal-info {
  text-align: center;
}

.info-label {
  font-size: 1.2em;
  margin-bottom: 10px;
  text-align: left;
  color: #333;
  font-weight: bold;
}
</style>
