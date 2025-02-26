---
layout: mypost
title: "Year 2 Project: 后端开发"
categories: [Year 2 Project]
lang: en-us
---

![FastAPI Logo](https://fastapi.tiangolo.com/img/logo-margin/logo-teal.png)

在[前端开发](https://juntong20xx.github.io/posts/2025/02/20/year-2-project-%E5%89%8D%E7%AB%AF%E5%BC%80%E5%8F%91.html)中，已经完成了对后端 API 的设计。

根据先前的设计，API 功能分为对设备列表管理和登录管理。

后端开发使用 [FastAPI](https://fastapi.tiangolo.com/)  其优势是可以直接返回 json 与前端交互。

## 列表管理

对 `items` 参数配置。

```python
@app.get("/items")
async def get_items() -> List[Dict[str, int|str]]
	...
    
@app.put("/items")
async def update_items(item) -> Dict[str, int|str]
	...
```

前端没有添加项目的功能，测试时使用 curl 指令发送包含新配置的 `PUT` 指令。

```bash
curl -X PUT http://localhost:8000/items \
     -H "Content-Type: application/json" \
     -d '{"key": "new_device", "value":100}'
```

![image-20250226120741459](items-test.png)

## 登录管理

登录采用基于时间的一次性密码算法（TOTP），通过`pyotp`库实现。

使用 `pyotp` 加载 TOTP secret 并验证前端的代码，代码示例如下：

```python
def verify_otp(code: str) -> bool:
    totp = pyotp.TOTP(SECRET_KEY)
    return totp.verify(code)
```

若验证错误，返回 401 认证错误。

验证通过则返回 json 数据。

测试记录：

![output](totp-test.gif)

## 成品测试

使用 `uvicorn src.app:app` 运行。

测试前端的后端 URL 动态配置功能，向命令添加 `--port` 参数修改运行端口。

## 部署

按照 FastAPI 官方文档 [^1]，使用 `Uvicorn` 进行部署。

部署后需要项目使用 `systemd` 服务文件 `/etc/systemd/system/y2p-backend.service`。

```ini
[Unit]
Description=Frp Server Clienet
After=network.target

[Service]
Type=simple
DynamicUser=yes
User=iot-arm
Group=iot-arm
Restart=on-failure
WorkingDirectory=/home/iot-arm/year2project-BackEnd
RestartSec=5s
ExecStart=/home/iot-arm/year2project-BackEnd/.venv/bin/uvicorn src.app:app --port 8123
LimitNOFILE=1048576

[Install]
WantedBy=multi-user.target
```

配置完成后，使用 `sudo systemctl enable --now y2p-backend` 启动服务。

一切顺利。



[^1]: https://fastapi.tiangolo.com/zh/deployment/manually/
