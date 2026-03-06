<template>
  <div class="container">
    <header class="header">
      <div class="logo">🚀</div>
      <h1>CI/CD 测试项目</h1>
      <p class="subtitle">Vue + GitHub Actions + ACR</p>
    </header>

    <main class="main">
      <!-- 状态卡片 -->
      <div class="cards">
        <div class="card success">
          <div class="card-icon">✅</div>
          <div class="card-title">构建状态</div>
          <div class="card-value">{{ buildStatus }}</div>
        </div>
        
        <div class="card info">
          <div class="card-icon">📦</div>
          <div class="card-title">镜像标签</div>
          <div class="card-value">{{ imageTag }}</div>
        </div>
        
        <div class="card warning">
          <div class="card-icon">⏱️</div>
          <div class="card-title">部署时间</div>
          <div class="card-value">{{ deployTime }}</div>
        </div>
      </div>

      <!-- 流程图 -->
      <div class="flow-section">
        <h2>部署流程</h2>
        <div class="flow">
          <div class="flow-item" :class="{ active: step >= 1 }">
            <div class="flow-icon">💻</div>
            <div class="flow-text">代码提交</div>
            <div class="flow-status">{{ step >= 1 ? '✓' : '○' }}</div>
          </div>
          <div class="flow-arrow">→</div>
          <div class="flow-item" :class="{ active: step >= 2 }">
            <div class="flow-icon">🔨</div>
            <div class="flow-text">GitHub Actions</div>
            <div class="flow-status">{{ step >= 2 ? '✓' : '○' }}</div>
          </div>
          <div class="flow-arrow">→</div>
          <div class="flow-item" :class="{ active: step >= 3 }">
            <div class="flow-icon">☁️</div>
            <div class="flow-text">推送 ACR</div>
            <div class="flow-status">{{ step >= 3 ? '✓' : '○' }}</div>
          </div>
          <div class="flow-arrow">→</div>
          <div class="flow-item" :class="{ active: step >= 4 }">
            <div class="flow-icon">🚀</div>
            <div class="flow-text">ECS 部署</div>
            <div class="flow-status">{{ step >= 4 ? '✓' : '○' }}</div>
          </div>
        </div>
      </div>

      <!-- 系统信息 -->
      <div class="info-section">
        <h2>系统信息</h2>
        <table class="info-table">
          <tr>
            <td>当前时间</td>
            <td>{{ currentTime }}</td>
          </tr>
          <tr>
            <td>浏览器</td>
            <td>{{ browserInfo }}</td>
          </tr>
          <tr>
            <td>页面加载耗时</td>
            <td>{{ loadTime }}ms</td>
          </tr>
        </table>
      </div>

      <!-- 操作按钮 -->
      <div class="actions">
        <button class="btn primary" @click="refresh">🔄 刷新页面</button>
        <button class="btn secondary" @click="simulateDeploy">▶️ 模拟部署动画</button>
      </div>
    </main>

    <footer class="footer">
      <p>Built with ❤️ | Vue 3 + Docker + ACR</p>
      <p class="hash">Commit: {{ commitHash }}</p>
    </footer>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      buildStatus: '成功',
      imageTag: 'latest',
      deployTime: new Date().toLocaleString('zh-CN'),
      currentTime: new Date().toLocaleString('zh-CN'),
      browserInfo: navigator.userAgent.split(' ')[0],
      loadTime: 0,
      commitHash: 'a1b2c3d',
      step: 4
    }
  },
  mounted() {
    // 计算页面加载时间
    this.loadTime = Math.round(performance.now())
    
    // 更新时间
    setInterval(() => {
      this.currentTime = new Date().toLocaleString('zh-CN')
    }, 1000)
  },
  methods: {
    refresh() {
      window.location.reload()
    },
    simulateDeploy() {
      this.step = 0
      const interval = setInterval(() => {
        this.step++
        if (this.step >= 4) {
          clearInterval(interval)
          this.deployTime = new Date().toLocaleString('zh-CN')
        }
      }, 800)
    }
  }
}
</script>

<style scoped>
.container {
  min-height: 100vh;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
}

.header {
  text-align: center;
  padding: 60px 20px 40px;
}

.logo {
  font-size: 64px;
  margin-bottom: 16px;
  animation: float 3s ease-in-out infinite;
}

@keyframes float {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-10px); }
}

h1 {
  font-size: 42px;
  font-weight: 700;
  margin-bottom: 8px;
  text-shadow: 2px 2px 4px rgba(0,0,0,0.2);
}

.subtitle {
  font-size: 18px;
  opacity: 0.9;
}

.main {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px 40px;
}

/* 卡片 */
.cards {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 24px;
  margin-bottom: 40px;
}

.card {
  background: rgba(255,255,255,0.15);
  backdrop-filter: blur(10px);
  border-radius: 16px;
  padding: 28px;
  text-align: center;
  border: 1px solid rgba(255,255,255,0.2);
  transition: transform 0.3s, box-shadow 0.3s;
}

.card:hover {
  transform: translateY(-5px);
  box-shadow: 0 20px 40px rgba(0,0,0,0.2);
}

.card-icon {
  font-size: 40px;
  margin-bottom: 12px;
}

.card-title {
  font-size: 14px;
  opacity: 0.8;
  margin-bottom: 8px;
  text-transform: uppercase;
  letter-spacing: 1px;
}

.card-value {
  font-size: 24px;
  font-weight: 600;
}

.card.success { border-top: 4px solid #4ade80; }
.card.info { border-top: 4px solid #60a5fa; }
.card.warning { border-top: 4px solid #fbbf24; }

/* 流程图 */
.flow-section {
  background: rgba(255,255,255,0.1);
  border-radius: 16px;
  padding: 32px;
  margin-bottom: 32px;
}

.flow-section h2 {
  text-align: center;
  margin-bottom: 28px;
  font-size: 24px;
}

.flow {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-wrap: wrap;
  gap: 12px;
}

.flow-item {
  background: rgba(255,255,255,0.1);
  border-radius: 12px;
  padding: 16px 20px;
  text-align: center;
  min-width: 100px;
  opacity: 0.5;
  transition: all 0.3s;
}

.flow-item.active {
  opacity: 1;
  background: rgba(255,255,255,0.25);
  transform: scale(1.05);
}

.flow-icon {
  font-size: 28px;
  margin-bottom: 6px;
}

.flow-text {
  font-size: 13px;
  margin-bottom: 4px;
}

.flow-status {
  font-size: 16px;
  font-weight: bold;
  color: #4ade80;
}

.flow-arrow {
  font-size: 24px;
  opacity: 0.6;
}

/* 信息表格 */
.info-section {
  background: rgba(255,255,255,0.1);
  border-radius: 16px;
  padding: 32px;
  margin-bottom: 32px;
}

.info-section h2 {
  text-align: center;
  margin-bottom: 24px;
  font-size: 24px;
}

.info-table {
  width: 100%;
  max-width: 600px;
  margin: 0 auto;
  border-collapse: collapse;
}

.info-table td {
  padding: 16px;
  border-bottom: 1px solid rgba(255,255,255,0.1);
}

.info-table td:first-child {
  font-weight: 500;
  opacity: 0.8;
  width: 40%;
}

/* 按钮 */
.actions {
  display: flex;
  gap: 16px;
  justify-content: center;
  flex-wrap: wrap;
}

.btn {
  padding: 14px 32px;
  border-radius: 10px;
  border: none;
  font-size: 16px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s;
}

.btn.primary {
  background: white;
  color: #667eea;
}

.btn.secondary {
  background: transparent;
  color: white;
  border: 2px solid rgba(255,255,255,0.5);
}

.btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 10px 20px rgba(0,0,0,0.2);
}

/* 页脚 */
.footer {
  text-align: center;
  padding: 40px 20px;
  opacity: 0.8;
}

.footer p {
  margin-bottom: 8px;
}

.hash {
  font-family: monospace;
  font-size: 12px;
  opacity: 0.6;
}
</style>
