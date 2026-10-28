# JavaScript 实战：从零开发一个本地待办事项管理工具

## 一、项目简介

本文将使用原生 JavaScript、HTML 和 CSS，开发一个简单实用的待办事项管理工具。项目无需安装第三方依赖，适合前端初学者练习 DOM 操作、事件处理、数组方法以及浏览器本地存储。

### 项目功能

- 添加新的待办事项
- 标记任务为已完成
- 删除不需要的任务
- 使用 `localStorage` 保存任务数据
- 刷新页面后保留已有记录

## 二、项目结构

创建一个文件夹，并添加以下文件：

```text
todo-app/
├── index.html
├── style.css
└── script.js
```

## 三、编写页面结构

在 `index.html` 中创建输入框、添加按钮和任务列表。

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>待办事项管理工具</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <main class="container">
    <h1>我的待办事项</h1>
    <form id="todo-form">
      <input id="todo-input" type="text"
             placeholder="请输入任务内容" required>
      <button type="submit">添加任务</button>
    </form>
    <ul id="todo-list"></ul>
  </main>
  <script src="script.js"></script>
</body>
</html>
```

## 四、实现核心功能

在 `script.js` 中编写任务管理逻辑。使用 `localStorage` 保存数据，并通过 DOM 创建任务节点。

```javascript
const form = document.querySelector("#todo-form");
const input = document.querySelector("#todo-input");
const list = document.querySelector("#todo-list");

let todos = JSON.parse(localStorage.getItem("todos") || "[]");

function saveTodos() {
  localStorage.setItem("todos", JSON.stringify(todos));
}

function renderTodos() {
  list.replaceChildren();

  todos.forEach(todo => {
    const item = document.createElement("li");
    const label = document.createElement("span");
    const complete = document.createElement("input");
    const remove = document.createElement("button");

    label.textContent = todo.text;
    complete.type = "checkbox";
    complete.checked = todo.completed;
    remove.textContent = "删除";

    if (todo.completed) {
      label.classList.add("completed");
    }

    complete.addEventListener("change", () => {
      todo.completed = complete.checked;
      saveTodos();
      renderTodos();
    });

    remove.addEventListener("click", () => {
      todos = todos.filter(task => task.id !== todo.id);
      saveTodos();
      renderTodos();
    });

    item.append(complete, label, remove);
    list.appendChild(item);
  });
}

form.addEventListener("submit", event => {
  event.preventDefault();

  const text = input.value.trim();
  if (!text) return;

  todos.push({
    id: crypto.randomUUID(),
    text,
    completed: false
  });

  saveTodos();
  renderTodos();
  input.value = "";
  input.focus();
});

renderTodos();
```

这里使用 `textContent` 显示用户输入，避免将任务文本直接解析为 HTML。任务通过唯一 ID 进行识别，删除操作不会依赖数组下标。

## 五、添加基础样式

在 `style.css` 中加入以下内容：

```css
body {
  margin: 0;
  padding: 30px 16px;
  font-family: Arial, sans-serif;
  background: #f4f6f8;
}

.container {
  max-width: 560px;
  margin: 0 auto;
  padding: 24px;
  background: #fff;
  border-radius: 12px;
}

#todo-form {
  display: flex;
  gap: 8px;
}

#todo-input {
  flex: 1;
  min-width: 0;
  padding: 10px;
}

button {
  padding: 8px 12px;
  cursor: pointer;
}

#todo-list {
  padding: 0;
  list-style: none;
}

#todo-list li {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-top: 12px;
}

#todo-list span {
  flex: 1;
  overflow-wrap: anywhere;
}

.completed {
  color: #888;
  text-decoration: line-through;
}
```

## 六、运行项目

1. 在本地创建 `todo-app` 文件夹。
2. 将上述代码分别保存到三个文件中。
3. 使用浏览器打开 `index.html`。
4. 输入任务并点击“添加任务”。
5. 尝试勾选完成状态、删除任务，再刷新页面检查数据是否保留。

项目依赖浏览器提供的 `localStorage` 和 `crypto.randomUUID()`。如果运行环境不支持 `crypto.randomUUID()`，可以替换为适用于目标环境的 ID 生成方案。

## 七、后续优化方向

完成基础功能后，可以继续扩展：

- 增加任务搜索与分类筛选。
- 添加任务截止日期和优先级。
- 支持导出、导入 JSON 数据。
- 使用模块化 JavaScript 拆分业务逻辑。
- 为关键功能编写自动化测试。

需要注意，`localStorage` 只适合保存当前浏览器中的本地数据，不提供跨设备同步，也不适合存储敏感信息。

## 八、总结

通过这个小项目，可以学习表单提交、DOM 操作、数组增删改查、事件监听和浏览器数据持久化等基础知识。整个项目不依赖框架，结构简单，适合作为 JavaScript 入门练习或 GitHub 个人项目的起点。

欢迎根据自己的需求继续扩展功能，并通过 Issue 或 Pull Request 分享改进建议。