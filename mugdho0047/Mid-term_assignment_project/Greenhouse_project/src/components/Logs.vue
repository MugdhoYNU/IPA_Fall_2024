<script setup>
import { ref, onMounted } from 'vue';
import { useRouter } from 'vue-router';

const router = useRouter();
const logs = ref([]);

onMounted(() => {
    const user = localStorage.getItem("user");
    if (!user) {
        router.push({ name: 'Login' });
    }
});

const fetchLogs = async () => {
  try {
    const response = await fetch('../../DB/logs.json'); 
    logs.value = await response.json();
  } catch (error) {
    console.error('Error fetching logs:', error);
  }
};

onMounted(fetchLogs);

const logout = async () => {
    localStorage.removeItem("user");
    router.push({ name: 'Login' });
};
</script>

<template>
  <div id="logs">
    <nav class="navbar">
      <div class="logo">Greenhouse</div>
      <ul class="nav-links">
        <li><router-link to="/dashboard">Home</router-link></li>
        <li><router-link to="/over-view">Overview</router-link></li>
        <li><router-link to="/settings">Settings</router-link></li>
        <li><router-link to="/logs">Logs</router-link></li>
        <li><a @click="logout">Logout</a></li>
      </ul>
    </nav>

    <main>
      <header>
        <h1>Logs</h1>
        <p>System Logs for Greenhouse Monitoring</p>
      </header>

      <section class="logs">
        <div 
          v-for="(log, index) in logs" 
          :key="index" 
          class="log-card"
        >
          <div class="log-card-icon" :style="{ backgroundColor: log.level === 'error' ? '#e74c3c' : log.level === 'warning' ? '#f39c12' : '#2ecc71' }">
            <span>{{ log.level === 'error' ? '⚠️' : log.level === 'warning' ? '⚡' : '✅' }}</span>
          </div>
          <div class="log-card-content">
            <h2>{{ log.message }}</h2>
            <p class="log-time">{{ new Date(log.timestamp).toLocaleString() }}</p>
            <p class="log-action">Action: {{ log.action }}</p>
            <p class="log-user">User: {{ log.user }}</p>
          </div>
        </div>
      </section>
    </main>
  </div>
</template>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.dashboard {
  position: relative;
  background-color: #f4f7fc;
  height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-start;
}

.navbar {
  position: fixed;
  top: 0;
  left: 0;
  height: 10%;
  width: 100%;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1.2rem 2rem;
  background-color: #34495e;
  color: #ffffff;
  border-bottom: 1px solid #2c3e50;
  box-shadow: 0 3px 5px rgba(0, 0, 0, 0.1);
  z-index: 100;
}

.logo {
  font-size: 2rem;
  font-weight: bold;
  color: #ffffff;
}

.nav-links {
  list-style: none;
  display: flex;
  gap: 20px;
}

.nav-links li a {
  text-decoration: none;
  color: #ecf0f1;
  font-size: 1rem;
  transition: color 0.3s ease;
}

.nav-links li a:hover {
  color: #1abc9c;
}

main {
  padding: 2rem 3rem;
}

header h1 {
  font-size: 2.2rem;
  font-weight: 700;
  color: #2c3e50;
  margin-bottom: 20px;
}

header p {
  font-size: 1rem;
  color: #d7d7d7;
  font-weight: 300;
}

.logs {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 20px;
  margin-top: 10px;
}

.log-card {
  display: flex;
  align-items: center;
  padding: 1.5rem;
  border-radius: 16px;
  background: linear-gradient(135deg, #ffffff, #f7f9fc);
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.05);
  transition: transform 0.2s ease, box-shadow 0.2s ease;
}

.log-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 15px 30px rgba(0, 0, 0, 0.15);
}

.log-card-icon {
  width: 60px;
  height: 60px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 50%;
  margin-right: 1.2rem;
}

.log-card-content h2 {
  font-size: 1.2rem;
  color: #34495e;
  margin-bottom: 6px;
}

.log-time {
  font-size: 0.9rem;
  color: #7f8c8d;
}

.log-action, .log-user {
  font-size: 0.85rem;
  color: #95a5a6;
  margin-top: 6px;
}

@media (max-width: 768px) {
  .logs {
    grid-template-columns: 1fr;
  }

  main {
    padding: 1.5rem;
  }

  .navbar {
    padding: 1.2rem 1.5rem;
  }

  header h1 {
    font-size: 1.8rem;
  }

  header p {
    font-size: 0.9rem;
  }

  .log-card {
    padding: 1rem;
  }

  .log-card-content h2 {
    font-size: 1rem;
  }

  .log-time {
    font-size: 0.8rem;
  }

  .log-action, .log-user {
    font-size: 0.75rem;
  }
}

@media (max-width: 480px) {
  .navbar {
    flex-direction: column;
    align-items: flex-start;
  }

  .nav-links {
    gap: 10px;
    overflow-x: auto;
    max-width: 100%;
  }

  header h1 {
    font-size: 1.5rem;
  }

  header p {
    font-size: 0.85rem;
  }

  .logs {
    margin-top: 20px;
  }

  .log-card {
    padding: 0.8rem;
  }

  .log-card-content h2 {
    font-size: 0.9rem;
  }

  .log-time {
    font-size: 0.75rem;
  }

  .log-action, .log-user {
    font-size: 0.7rem;
  }
}
</style>