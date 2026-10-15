# 项目报告 — 基于 Spark 的音乐专辑数据分析展示

本报告按照数据流与处理阶段整理复现流程、技术要点与运行命令，便于复现与教学。

## 七个环节概述

1. 原始数据采集
- 作用：获取 `albums.csv`，字段应包含 `album_title, genre, year_of_pub, num_of_tracks, num_of_sales, rolling_stone_critic, mtv_critic, music_maniac_critic`，以 UTF-8 编码保存。

2. 数据导入
- 作用：将 `albums.csv` 放到计算可访问位置（项目根或 HDFS）。
- 本仓库：将 `albums.csv` 放在仓库根目录即可（已就绪）。

3. 数据预处理
- 使用 PySpark 清洗与标准化：类型转换、去重、缺失值处理、字段规范化。
- 实现在 `code/main.py` 中，包含安全的数值转换与空值处理逻辑。

4. 分布式计算分析
- 输出：`code/static/data/` 下的 JSON 文件：
  - `genre.json` — 各流派专辑计数（仅展示 count>2000 的前十）
  - `genre-sales.json` — 各流派销量总和
  - `year-tracks-and-sales.json` — 按年汇总的专辑数与单曲数
  - `genre-year-sales.json` — 销量前五流派的年度销量
  - `genre-critic.json` — 销量前五流派在不同评分体系中的均分
- 建议生产运行：`spark-submit code/main.py`；开发单机可用 `python code/main.py`（已在 `main.py` 处理路径与兼容）。

5. 结果输出
- JSON 文件以 UTF‑8 保存，结构与前端模板匹配（参见 `code/templates` 与 `code/static/js`）。

6. 可视化展示
- 后端：`code/SparkFlask.py`（Flask）用于渲染模板与提供静态文件服务。
- 前端：ECharts（已包含 `code/static/js`）的模板位于 `code/templates`。
- 启动方式：
  - 开发：`python code/SparkFlask.py`
  - 生产：使用 WSGI/反向代理部署（示例：`gunicorn` 或 IIS/nginx + uwsgi）。

7. 结论与归档
- 将代码、`albums.csv`、生成的 JSON 与本报告一并归档；在实验报告中记录清洗规则与统计方法。

## 运行步骤（Windows 示例）

1. 创建并激活虚拟环境：
```powershell
python -m venv .venv
.\.venv\Scripts\activate
```

2. 安装依赖：
```powershell
pip install -r requirements.txt
```

3. 生成 JSON 数据（在项目根或进入 `code` 目录）：
```powershell
cd code
python main.py
```

4. 启动 Flask 展示页面：
```powershell
python SparkFlask.py
# 然后在浏览器打开 http://127.0.0.1:5000
```

## 注意事项与建议
- Spark（pyspark）包较大，Windows 下安装与运行可能较慢，建议在有网络的环境中耐心等待或使用离线 wheel。若需要使用集群，请用 `spark-submit` 并配置 `SPARK_HOME` 与 `JAVA_HOME`。
- 若 `albums.csv` 字段名或格式不同，请在 `code/main.py` 中调整字段映射或预处理逻辑。
- 若要改进：增加数据验证报告、生成更细粒度的中间 CSV、在前端加入交互筛选控件。

---

报告生成于本仓库根目录下的 `REPORT.md`，如需我继续等待并完成依赖安装、运行 `main.py` 并启动 Flask，我可以接着执行并把结果反馈给你。