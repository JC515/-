<template>
  <div class="word-container">
    <div class="personal-info-card-container">
      <div class="personal-info-card">
        <el-row :gutter="20">
          <el-col :span="24">
            <div class="personal-info">
              <p class="info-label">用户名:</p>
              <p class="info-value">{{ userInfo.username }}</p>
              <p class="info-label">邮箱:</p>
              <p class="info-value">{{ userInfo.email }}</p>
            </div>
          </el-col>
        </el-row>
      </div>
    </div>
  </div>
</template>

<style scoped>
.word-container {
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

.personal-info-card-container {
  width: 75%; /* Adjusted width for 4:3 aspect ratio */
  max-width: 800px; /* Maximum width */
  display: flex;
  justify-content: center;
}

.personal-info-card {
  width: 100%; /* Full width */
  height: 600px; /* Adjusted height for 4:3 aspect ratio */
  background: rgba(255, 255, 255, 0.8);
  border-radius: 10px;
  padding: 20px;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.1);
}

.personal-info {
  text-align: center;
}

.info-label {
  font-size: 1.2em;
  margin-bottom: 5px;
}

.info-value {
  font-size: 1.5em;
  font-family: 'Your Artistic Font', sans-serif;
  font-weight: bold;
  margin-bottom: 10px;
}
</style>

<script>
export default {
  data() {
    return {
      // ... existing data ...
      userInfo: {
        username: 'YourUsername',
        email: 'your.email@example.com',
      },
    };
  },
};
</script>
