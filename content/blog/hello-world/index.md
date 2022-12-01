---
title: Hello World
date: "2022-12-01T08:12:03.284Z"
description: "12/01"
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

    return {'Days' : days_to_xmas}


if __name__ == "__main__":
    uvicorn.run("main:app", host="0.0.0.0", port=9090, log_level="warning")
```


What I've done:
Since 25 minutes is a really short period, I've decided to use FastAPI(), one of the fastest API frameworks for Python.
After importing all need elements, I created two application endpoints. 
First, 'healthcheck', allow the user to verify if the application is working or not.
Second, root, uses the standard DateTime library, to define the time delta between December 25 of the current year, and now.
Finally, I create a JSON where the key is "Days" and the value is the integer of the number of days between now and Christmas.





