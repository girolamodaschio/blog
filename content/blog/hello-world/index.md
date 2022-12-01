---
title: Hello World
date: "2022-12-01T08:12:03.284Z"
description: "Hello World"
---

I'll code, for myself, at least 25 minutes per day.
Starting today, Codecember the 1st!
Here we start with an API written in Python that tells the user how many days are missing until Christmas.

```py
from fastapi import FastAPI, status
from fastapi.responses import JSONResponse

import datetime

import uvicorn as uvicorn


app = FastAPI()


@app.get("/healthcheck")
def healthcheck():
    return JSONResponse(status_code=status.HTTP_200_OK, content={"Status": "Running"})


@app.get("/")
def main():
    xmas = datetime.datetime(datetime.datetime.now().year, 12, 25)
    today = datetime.datetime.now()
    days_to_xmas = (xmas - today).days

    return JSONResponse(status_code=status.HTTP_200_OK, content={"Days": days_to_xmas})


if __name__ == "__main__":
    uvicorn.run("main:app", host="0.0.0.0", port=9090, log_level="warning")
```

