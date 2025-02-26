---
layout: mypost
title: "Year 2 Project: Backend Development"
categories: [Year 2 Project]
lang: en-us
---

Link to [Chinese Version](https://juntong20xx.github.io/posts/2025/02/20/year-2-project-%E5%90%8E%E7%AB%AF%E5%BC%80%E5%8F%91.html).

In the frontend development, we've completed the design of backend APIs.

According to previous designs, the API functionality is divided into device list management and login management.

The backend uses FastAPI, whose advantage is direct JSON interaction with the frontend.

## List Management

Configuration of items parameters:

```python
@app.get("/items")
async def get_items() -> List[Dict[str, int|str]] ...

@app.put("/items")
async def update_items(item) -> Dict[str, int|str] ...
```

The frontend doesn't have item addition functionality. During testing, use curl commands to send PUT requests with new configurations:

```bash
curl -X PUT http://localhost:8000/items \
  -H "Content-Type: application/json" \
  -d '{"key": "new_device", "value":100}'
```

## Login Management

Login implementation uses Time-based One-Time Password (TOTP) algorithm through `pyotp` library:

```python
def verify_otp(code: str) -> bool:
    totp = pyotp.TOTP(SECRET_KEY)
    return totp.verify(code)
```

Returns 401 Unauthorized for invalid codes. Returns JSON data upon successful verification.

## Testing Record

Run using `uvicorn src.app:app`.

Test frontend's dynamic backend URL configuration by adding `--port` parameter to modify running port.

## Deployment

Following FastAPI official documentation[^1], deploy using Uvicorn.

After deployment, configure systemd service file at `/etc/systemd/system/y2p-backend.service`:

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

After configuration, start service with `sudo systemctl enable --now y2p-backend`.

Everything works smoothly.

[^1]: https://fastapi.tiangolo.com/deployment/manually/#_1