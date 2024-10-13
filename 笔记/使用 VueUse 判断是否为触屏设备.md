
### 使用 VueUse 判断是否为触屏设备

在现代 Web 开发中，判断用户设备类型（如触屏设备或非触屏设备）是一个常见需求。VueUse 提供了一些非常方便的组合式函数，可以帮助我们轻松实现这一点。本文将介绍如何使用 VueUse 来判断设备是否为触屏设备。

#### 安装 VueUse

首先，我们需要安装 VueUse。你可以使用 npm 或 yarn 来安装：

```bash
npm install @vueuse/core
```

或者

```bash
yarn add @vueuse/core
```

#### 使用 `useMediaQuery` 检测触屏设备

VueUse 提供了 `useMediaQuery` 组合式函数，它允许我们使用 CSS 媒体查询来检测设备特性。我们可以使用 `(pointer: coarse)` 媒体查询来判断设备是否支持粗略指针（通常表示触屏设备）。

以下是一个完整的示例，展示如何在 Vue 组件中使用 `useMediaQuery` 来判断设备类型：

```vue
<template>
  <div>
    <p>{{ deviceType }}</p>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import { useMediaQuery } from '@vueuse/core';

const deviceType = ref('');

// 使用 useMediaQuery 检测是否为触屏设备
const isTouchDevice = useMediaQuery('(pointer: coarse)');

onMounted(() => {
  deviceType.value = isTouchDevice.value ? 'Touch Device' : 'Non-Touch Device';
});
</script>
```

#### 解释代码

1. **导入必要的模块**：我们从 `vue` 中导入 `ref` 和 `onMounted`，并从 `@vueuse/core` 中导入 `useMediaQuery`。
2. **定义响应式变量**：使用 `ref` 定义一个响应式变量 `deviceType`，用于存储设备类型。
3. **使用 `useMediaQuery`**：调用 `useMediaQuery` 并传入媒体查询字符串 `(pointer: coarse)`，返回一个响应式引用 `isTouchDevice`。
4. **在组件挂载时设置设备类型**：使用 `onMounted` 钩子，在组件挂载时根据 `isTouchDevice` 的值设置 `deviceType`。

#### 总结

通过使用 VueUse 提供的 `useMediaQuery` 组合式函数，我们可以轻松地检测用户设备是否为触屏设备。这种方法不仅简洁高效，而且充分利用了 Vue 3 的组合式 API 和响应式系统。

希望这篇笔记对你有所帮助！如果你有任何问题或需要进一步的帮助，请随时告诉我。😊

---