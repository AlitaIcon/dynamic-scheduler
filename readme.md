#  动态的带结果保存的定时任务管理器-DynamicScheduler
## 安装
```ini
pip install dynamic-scheduler
```
## 介绍
    主要是结合 apscheduler 完成定时任务动态修改定时任务功能、增加定时任务结果缓存功能
## test
1. 运行下方demo
2. 重新config类，指定数据库schema
3. 尝试修改数据库表 cron中cron表达式
4. 验证定时任务是否修改
5. 查看apscheduler_jobs表任务结果

## demo
```python
from DynamicScheduler.dynamic_scheduler import DynamicSchedulerProxy
from DynamicScheduler.scheduler_utils import SchedulerConfig

import datetime, os, threading
def task():
    # print(datetime.datetime.now())
    # time.sleep(10)
    print('start time: ', datetime.datetime.now(), os.getpid(), threading.current_thread().name)
    return datetime.datetime.now()

class Config(SchedulerConfig):
    DEFAULT_DB_URL = 'postgresql+psycopg2://postgres:123456@{your_ip}:5432/postgres?utf-8'
    SCHEDULER_SCHEMA = 'public'

s = DynamicSchedulerProxy(config_class=Config)

s.add_job_with_default(func=task, cron_str='* * */5', id='task')
s.clear_history()  # 先清除缓存表
s.add_result_listener()
s.start()
```
