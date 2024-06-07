### Jinja2 简介
Jinja2 是一个现代且设计优雅的Python模板引擎，广泛应用于各种Python web框架（如Flask）。它的主要特点包括：
- 表达式和控制结构支持，如循环和条件语句。
- 能够自动转义HTML，防止XSS攻击。
- 支持模板继承，便于复用和组织模板。
- 丰富的内置过滤器和测试器。

### 安装 Jinja2
你可以使用 `pip` 来安装 Jinja2：
```sh
pip install jinja2
```

### 使用 Jinja2 渲染字典数据到 HTML 模板
以下是一个简单的示例，展示如何使用 Jinja2 将字典中的数据渲染到 HTML 模板：
1. 创建一个 HTML 模板文件，例如 `template.html`：
```html
<!DOCTYPE html>
<html>
<head>
    <title>{{ title }}</title>
</head>
<body>
    <h1>{{ heading }}</h1>
    <p>{{ message }}</p>
</body>
</html>
```

2. 使用 Python 脚本渲染模板：
```python
from jinja2 import Environment, FileSystemLoader

# 设置模板文件夹
file_loader = FileSystemLoader('templates')
env = Environment(loader=file_loader)

# 加载模板
template = env.get_template('template.html')

# 定义要渲染的字典数据
data = {
    'title': 'My Page',
    'heading': 'Hello, World!',
    'message': 'This is a message rendered with Jinja2.'
}

# 渲染模板并传入数据
output = template.render(data)

# 输出渲染后的HTML
print(output)
```

在这个示例中，`template.html` 文件存储在 `templates` 文件夹中。Python 脚本加载模板并将字典数据渲染到模板中，最终输出渲染后的HTML内容。

## 案例
这里是对如何使用Jinja2实现模板继承和包含功能的详细讲解和笔记。

### 1. 文件结构
首先，创建项目的文件结构：
```
/templates
    main.html
    thread-default.html
    thread-bangumi.html
script.py
```

- **templates**：存放所有的HTML模板文件。
- **main.html**：主模板，包含帖子内容的总体框架。
- **thread-default.html**：默认帖子模板。
- **thread-bangumi.html**：番剧帖子模板。
- **script.py**：Python脚本，用于渲染模板。

### 2. main.html
主模板 `main.html` 包含页面的基本框架和帖子循环逻辑。通过 `include` 标签动态加载不同的帖子模板。
```html
<!DOCTYPE html>
<html>
<head>
    <title>Forum</title>
</head>
<body>
    <h1>Welcome to the Forum</h1>
    
    {% for thread in threads %}
        {% if thread.type == 'default' %}
            {% include 'thread-default.html' %}
        {% elif thread.type == 'bangumi' %}
            {% include 'thread-bangumi.html' %}
        {% endif %}
    {% endfor %}
</body>
</html>
```

### 3. thread-default.html
默认帖子模板 `thread-default.html` 定义了默认帖子的HTML结构。
```html
<div class="thread">
    <h2>{{ thread.title }}</h2>
    <p>{{ thread.content }}</p>
</div>
```

### 4. thread-bangumi.html
番剧帖子模板 `thread-bangumi.html` 定义了番剧帖子的HTML结构，并加上特定的样式类 `bangumi`。
```html
<div class="thread bangumi">
    <h2>{{ thread.title }} (Bangumi)</h2>
    <p>{{ thread.content }}</p>
</div>
```

### 5. script.py
Python脚本 `script.py` 负责加载模板并传入数据，渲染生成最终的HTML页面。
```python
from jinja2 import Environment, FileSystemLoader

# 设置模板文件夹
file_loader = FileSystemLoader('templates')
env = Environment(loader=file_loader)

# 加载主模板
template = env.get_template('main.html')

# 定义帖子数据
threads = [
    {'type': 'default', 'title': 'Default Thread 1', 'content': 'Content for default thread 1.'},
    {'type': 'bangumi', 'title': 'Bangumi Thread 1', 'content': 'Content for bangumi thread 1.'},
    {'type': 'default', 'title': 'Default Thread 2', 'content': 'Content for default thread 2.'}
]

# 渲染模板并传入数据
output = template.render(threads=threads)

# 输出渲染后的HTML
print(output)
```

### 6. 运行脚本
运行 `script.py`：
```sh
python script.py
```

### 7. 渲染结果
脚本执行后，将生成包含不同帖子类型的完整HTML页面。输出如下：
```html
<!DOCTYPE html>
<html>
<head>
    <title>Forum</title>
</head>
<body>
    <h1>Welcome to the Forum</h1>
    
    <div class="thread">
        <h2>Default Thread 1</h2>
        <p>Content for default thread 1.</p>
    </div>
    
    <div class="thread bangumi">
        <h2>Bangumi Thread 1 (Bangumi)</h2>
        <p>Content for bangumi thread 1.</p>
    </div>
    
    <div class="thread">
        <h2>Default Thread 2</h2>
        <p>Content for default thread 2.</p>
    </div>
    
</body>
</html>
```

### 8. 详细讲解

- **模板文件夹 (`templates`)**：存放所有HTML模板文件，方便管理和组织。
- **主模板 (`main.html`)**：作为页面的整体框架，包含循环逻辑，用于加载和显示不同类型的帖子。
  - `{% for thread in threads %}`：循环遍历传入的 `threads` 数据列表。
  - `{% include 'thread-default.html' %}` 和 `{% include 'thread-bangumi.html' %}`：根据帖子类型动态加载不同的模板文件。
- **帖子模板 (`thread-default.html` 和 `thread-bangumi.html`)**：定义不同类型帖子的具体HTML结构。
- **Python脚本 (`script.py`)**：
  - `Environment` 和 `FileSystemLoader`：用于设置模板环境和加载模板文件夹。
  - `env.get_template('main.html')`：加载主模板。
  - `template.render(threads=threads)`：渲染模板并传入 `threads` 数据列表。
  - `print(output)`：输出渲染后的HTML。

通过这种方式，可以灵活地在主模板中包含不同的子模板，动态生成符合需求的HTML页面。这种方法适用于各种动态内容渲染场景，特别是在Web应用开发中。


## 自动格式化模板
在 Visual Studio Code (VSCode) 中，为了自动格式化 Django 模板，你可以使用相关的扩展和配置。以下是启用 Django 模板自动格式化的步骤：

### 1. 安装必要的扩展

#### a. Django Template Formatter
这是一个专门为 Django 模板提供格式化功能的 VSCode 扩展。

- 打开 VSCode。
- 进入扩展面板（左侧栏中的方块图标）。
- 搜索并安装 **Django Template Formatter** 扩展。

#### b. Prettier - Code formatter
这是一个通用的代码格式化工具，它支持多种语言和文件类型。你可以使用它来格式化 Django 模板。

- 打开 VSCode。
- 进入扩展面板（左侧栏中的方块图标）。
- 搜索并安装 **Prettier - Code formatter** 扩展。

### 2. 配置 VSCode

#### a. 设置 Prettier 作为默认格式化工具

1. 打开 VSCode 的设置（通过 `Ctrl+,` 或 `Cmd+,`）。
2. 搜索 `default formatter`，选择 **Prettier** 作为默认格式化工具。

```json
{
    "editor.defaultFormatter": "esbenp.prettier-vscode"
}
```

#### b. 配置文件关联

确保 VSCode 将 Django 模板文件识别为 HTML 文件。你可以在设置中添加以下内容：

1. 打开 VSCode 的设置（通过 `Ctrl+,` 或 `Cmd+,`）。
2. 搜索 `files.associations`，添加以下配置，将 `.html` 文件关联为 Django HTML：

```json
{
    "files.associations": {
        "*.html": "django-html"
    }
}
```

#### c. 启用格式化

启用保存时自动格式化功能：

1. 打开 VSCode 的设置（通过 `Ctrl+,` 或 `Cmd+,`）。
2. 搜索 `format on save` 并启用。

```json
{
    "editor.formatOnSave": true
}
```

### 3. 验证配置

创建一个 Django 模板文件，例如 `example.html`，并写入一些未格式化的HTML代码。保存文件，Prettier 应该会自动格式化代码。

### 4. 使用 Django Template Formatter

如果你使用的是 **Django Template Formatter** 扩展，它提供了命令来格式化文件。

1. 打开命令面板（通过 `Ctrl+Shift+P` 或 `Cmd+Shift+P`）。
2. 搜索并选择 `Format Document`。

### 5. 手动配置（如果需要）

如果上述步骤没有效果，您可能需要手动配置 Prettier 和 Django Template Formatter 的设置文件。以下是一个示例配置：

在项目根目录下创建或编辑 `.prettierrc` 文件：

```json
{
    "singleQuote": true,
    "trailingComma": "all",
    "tabWidth": 4,
    "useTabs": false
}
```

在项目根目录下创建或编辑 `.vscode/settings.json` 文件：

```json
{
    "editor.defaultFormatter": "esbenp.prettier-vscode",
    "editor.formatOnSave": true,
    "files.associations": {
        "*.html": "django-html"
    },
    "emmet.includeLanguages": {
        "django-html": "html"
    }
}
```

### 总结

通过以上步骤，您可以在 VSCode 中启用 Django 模板的自动格式化功能。主要是通过安装适当的扩展（如 Prettier 和 Django Template Formatter），并进行相应的配置来实现自动格式化功能。这样可以确保你的 Django 模板代码始终保持整洁和一致。