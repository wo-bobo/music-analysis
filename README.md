# 基于 Spark 的音乐专辑数据分析展示

本项目复现并改进自厦大数据库实验室博客的音乐专辑分析示例。

先决条件
- Python 3.8+
- Java（Spark 运行需要 Java）
- 建议使用虚拟环境

安装依赖

```bash
python -m venv .venv
.\.venv\Scripts\activate
pip install -r requirements.txt
```

生成可视化数据（使用本地 Spark）

```bash
cd code
python main.py
```

运行 Web 应用（Flask）

```bash
cd code
python SparkFlask.py
```

说明
- `main.py` 会读取仓库根目录下的 `albums.csv`，并把生成的 JSON 写入 `code/static/data/` 目录，供前端模板使用。
- 如需在生产环境运行，请使用 `spark-submit` 或配置完整的 Spark 集群。