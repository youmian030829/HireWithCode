<script setup lang="ts">
import { onMounted, reactive, ref } from "vue";
import { marked } from "marked";
import { Right } from '@element-plus/icons-vue';
import { ElMessage } from "element-plus";

let currentPage = ref(1); // 当前页面
const parsedMarkdown = ref(); // 解析后的 Markdown 文本
/** 接受挑战表单值 */
const acceptChanllengeForm = ref({
  githubId: "",
  email: ""
})
/** 提交作品表单值 */
const putForm = ref({
  githubUrl: "",
  address: ""
})

onMounted(() => {
  setTimeout(() => {
    currentPage.value ++; // 两秒后自动切换到下一页
    parseMarkdownToHTML(); // 解析 Markdown 文本
  }, 2000);
})

// 加载 README.md 文件并解析为 HTML 文本
const parseMarkdownToHTML = async () => {
  const response = await fetch('../README.md'); // 加载 README.md 文件
  const markdownText = await response.text();
  parsedMarkdown.value = marked.parse(markdownText); // 解析 Markdown 文本
}

/** 接受挑战 */
const handleAccpet = () => {
  currentPage.value ++; // 切换到下一页
}

/** 提交接受挑战表单 */
const handleSubmit = () => {
  // 静态页面没有连接数据库，这里做简略判断
  if (!acceptChanllengeForm.value.githubId || !acceptChanllengeForm.value.email) {
    ElMessage.error("请填写完整的表单！");
  } else if (acceptChanllengeForm.value.githubId !== "youmian030829" || acceptChanllengeForm.value.email !== "3111457981@qq.com") {
    ElMessage.error("githubID 或邮箱错误！");
  } else {
    currentPage.value ++; // 切换到下一页
  }
}

/** 提交作品 */
const handlePut = () => {
  ElMessage.success("作品提交成功！");
}
</script>

<template>
  <div class="all-screens">
    <div v-if="currentPage === 1" class="first-screen">
      <img src="./assets/logo.png" style="width: 100px;height: 100px;" alt="Logo" />
      <h1 class="title">欢迎来到 infist 线上面试环节，期待你的加入！</h1>
    </div>
    <div v-else-if="currentPage === 2" class="second-screen">
      <div class="submit-btn-box">
        <el-button type="primary" @click="handleAccpet" size="large">
          接受挑战<el-icon><Right /></el-icon>
        </el-button>`
      </div>
      <div v-html="parsedMarkdown"></div>
    </div>
    <div v-else-if="currentPage === 3" class="third-screen">
      <el-form :model="acceptChanllengeForm" label-width="auto" class="form-box">
        <el-form-item label="githubId">
          <el-input v-model="acceptChanllengeForm.githubId" placeholder="请输入githubId" />
        </el-form-item>
        <el-form-item label="邮箱">
          <el-input v-model="acceptChanllengeForm.email" class="second-input" placeholder="请输入邮箱" />
        </el-form-item>
        <div class="submit-btn-box">
           <el-button type="primary" @click="handleSubmit()">
            接受挑战
          </el-button>`
        </div>
      </el-form>
    </div>
    <div v-else class="fourth-screen">
      <el-form :model="acceptChanllengeForm" label-width="auto" class="form-box">
        <el-form-item label="githubId">
          <el-input v-model="putForm.githubUrl" placeholder="请输入 github 仓库 url" />
        </el-form-item>
        <el-form-item label="Vercel 在线体验地址">
          <el-input v-model="putForm.address" class="second-input" placeholder="请输入 Vercel 在线体验地址" />
        </el-form-item>
        <div class="submit-btn-box">
           <el-button type="primary" @click="handlePut()">
            提交作品
          </el-button>`
        </div>
      </el-form>
    </div>
    </div>
</template>

<style lang="less">
.all-screens {
  height: 100vh;
  width: 100vw;

  .first-screen, .second-screen, .third-screen, .fourth-screen {
    height: 100%;
    background-color: aliceblue;
  }

  .first-screen {
    display: flex;
    justify-content: center;
    align-items: center;
    color: #333;
    animation: fadeIn 1s ease; /* 淡入动画 */
  }
  @keyframes fadeIn {
    from {
      opacity: 0;
    }
    to {
      opacity: 1;
    }
  }

  .second-screen {
    padding: 10px 0 0 10px;

    .el-icon {
      font-size: 17px;
      animation: moveRight 0.5s ease-in-out infinite alternate;
      margin-left: 5px;
    }
    @keyframes moveRight {
        from {
            transform: translateX(0);
        }
        to {
            transform: translateX(5px);
        }
    }
  }

  .submit-btn-box {
    display: flex;
    justify-content: center;
    align-items: center;
    margin-top: 10px;
  }

  .third-screen, .fourth-screen {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;

    .form-box {
      width: 500px;

      .second-input {
        margin-top: 10px;
      }
    }
  }
}
</style>
