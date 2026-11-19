<script setup>
import { ref } from 'vue'

const ISSUES = 'https://github.com/liuguanyi688-del/naruto-substitute-timer/issues'
const copied = ref(false)

// 复制反馈地址：老内置浏览器没有 clipboard API 时退回到 execCommand
async function copy() {
  try {
    await navigator.clipboard.writeText(ISSUES)
  } catch {
    const t = Object.assign(document.createElement('textarea'), { value: ISSUES })
    document.body.append(t)
    t.select()
    const ok = document.execCommand('copy')
    t.remove()
    if (!ok) return // 复制不了：地址就在按钮上，长按也能选
  }
  copied.value = true
  setTimeout(() => (copied.value = false), 2000)
}
</script>

<template>
  <div class="feedback" role="group" aria-label="BUG 反馈入口">
    <a class="fb-main" :href="ISSUES" target="_blank" rel="noopener">
      <svg viewBox="0 0 24 24" aria-hidden="true">
        <!-- GitHub mark（simple-icons, CC0）-->
        <path d="M12 .297c-6.63 0-12 5.373-12 12 0 5.303 3.438 9.8 8.205 11.385.6.113.82-.258.82-.577 0-.285-.01-1.04-.015-2.04-3.338.724-4.042-1.61-4.042-1.61C4.422 18.07 3.633 17.7 3.633 17.7c-1.087-.744.084-.729.084-.729 1.205.084 1.838 1.236 1.838 1.236 1.07 1.835 2.809 1.305 3.495.998.108-.776.417-1.305.76-1.605-2.665-.3-5.466-1.332-5.466-5.93 0-1.31.465-2.38 1.235-3.22-.135-.303-.54-1.523.105-3.176 0 0 1.005-.322 3.3 1.23.96-.267 1.98-.399 3-.405 1.02.006 2.04.138 3 .405 2.28-1.552 3.285-1.23 3.285-1.23.645 1.653.24 2.873.12 3.176.765.84 1.23 1.91 1.23 3.22 0 4.61-2.805 5.625-5.475 5.92.42.36.81 1.096.81 2.22 0 1.606-.015 2.896-.015 3.286 0 .315.21.69.825.57C20.565 22.092 24 17.592 24 12.297c0-6.627-5.373-12-12-12" />
      </svg>
      <span class="txt">
        <small>BUG 反馈 · 忍者识别问题</small>
        <b>GitHub Issues</b>
      </span>
      <span class="hint">打开反馈页 ↗</span>
    </a>
    <button class="fb-copy" type="button" @click="copy" :aria-live="'polite'">
      {{ copied ? '已复制 ✓' : '复制地址' }}
    </button>
  </div>
</template>

<style scoped>
.feedback {
  display: inline-flex;
  align-items: stretch;
  gap: 0;
  margin-top: 24px;
  border: 1px solid rgba(255, 143, 171, 0.6);
  border-radius: 16px;
  background: rgba(10, 12, 28, 0.55);
  backdrop-filter: blur(10px);
  box-shadow: 0 0 24px rgba(255, 77, 109, 0.35);
  overflow: hidden;
  transition: box-shadow 0.2s, transform 0.2s;
}
.feedback:hover {
  transform: translateY(-1px);
  box-shadow: 0 0 34px rgba(255, 77, 109, 0.6);
}
.fb-main {
  display: inline-flex;
  align-items: center;
  gap: 12px;
  padding: 10px 16px 10px 10px;
  color: #fff;
  text-decoration: none;
  text-align: left;
}
.fb-main:focus-visible,
.fb-copy:focus-visible {
  outline: 2px solid #ff8fab;
  outline-offset: -2px;
}
svg {
  flex: none;
  width: 40px;
  height: 40px;
  padding: 7px;
  border-radius: 11px;
  background: linear-gradient(135deg, #24292f, #12181f);
  fill: #fff;
}
.txt {
  display: flex;
  flex-direction: column;
  line-height: 1.2;
}
small {
  font-size: 13px;
  color: rgba(255, 255, 255, 0.78);
}
b {
  font-size: 22px;
  letter-spacing: 0.5px;
}
.hint {
  margin-left: 4px;
  padding: 5px 11px;
  border-radius: 999px;
  background: linear-gradient(135deg, #ff4d6d, #c9184a);
  font-size: 12px;
  font-weight: 600;
  white-space: nowrap;
}
.fb-copy {
  border: none;
  border-left: 1px solid rgba(255, 143, 171, 0.45);
  background: rgba(255, 77, 109, 0.14);
  color: #ffd7de;
  font-size: 13px;
  font-weight: 600;
  padding: 0 16px;
  cursor: pointer;
  transition: background 0.15s;
}
.fb-copy:hover {
  background: rgba(255, 77, 109, 0.3);
}
</style>