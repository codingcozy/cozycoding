---
title: "Node.js와 Express에서 세션 관리하기 초간단 가이드"
description: ""
coverImage: "/assets/img/2024-09-10-ManageSessionsinNodejsandExpress_0.png"
date: 2024-09-10 18:41
ogImage: 
  url: /assets/img/2024-09-10-ManageSessionsinNodejsandExpress_0.png
tag: Tech
originalTitle: "Manage Sessions in Node.js and Express"
link: "https://medium.com/@Zihan.Lin/manage-sessions-in-node-js-and-express-584a9b19d10b"
isUpdated: false
---


<img src="/assets/img/2024-09-10-ManageSessionsinNodejsandExpress_0.png" />

# 소개

익스프레스 응용 프로그램 내에서 세션을 구현하려면 미들웨어로 사용할 NPM 모듈인 'express-session'을 사용할 수 있습니다.

세션 미들웨어는 일반적으로 세션 데이터의 생성, 검색 및 관리를 처리하며, 일반적으로 세션 스토어(인메모리 스토어, 데이터베이스 또는 Redis와 같은 서드파티 서비스)에 저장됩니다.

<div class="content-ad"></div>

# 의존성 설치

```js
npm i express-session connect-redis redis
```

# 세션 미들웨어 설정

이제 express-session을 설치했으니 미들웨어를 구성해 세션을 만들 수 있도록 설정해보겠습니다.

<div class="content-ad"></div>

설정할 수 있는 몇 가지 옵션이 있습니다:

```js
import RedisStore from "connect-redis";
import session from "express-session";
import { createClient } from "redis";

let redisClient = createClient({
  url: REDIS_URL,
});
redisClient
  .connect()
  .then(() => {
    console.log("Redis에 연결되었습니다.");
  })
  .catch(console.error);

app.use(
  session({
    store: new RedisStore({
      client: redisClient,
    }),
    secret: SESSION_SECRET,
    resave: false, // 필수: 가볍게 세션을 유지하기 위한 강제 세션 유지(touch)
    saveUninitialized: false, // 권장: 데이터가 있는 경우에만 세션 저장
    cookie: {
      secure: process.env.NODE_ENV === "production",
      httpOnly: true,
      maxAge: 600000 * 60 * 24 * 60,
    },
  })
);
```

세션 미들웨어가 올바르게 구성되고 Express 애플리케이션에 통합되면 각 수신 요청 객체에 세션 속성을 만듭니다.

이 세션 속성을 통해 해당 요청과 연결된 현재 세션 데이터에 액세스할 수 있습니다. 이 세션 객체를 통해 현재 사용자 세션의 세션 데이터를 읽고 쓰거나 수정할 수 있습니다.

<div class="content-ad"></div>

# 세션 데이터에 액세스하고 수정하기

세션 미들웨어를 구성했으므로 이제 세션을 활용하고 인증 프로세스와 결합할 수 있습니다.

```js
import { Request, Response } from "express";
import { UserModel } from "../models/userModel";
import bcrypt from "bcryptjs";

declare module "express-session" {
  export interface SessionData {
    user: { id: string; username: string };
  }
}

export const signup = async (req: Request, res: Response) => {
  const { username, password } = req.body;
  try {
    const hashpassword: string = await bcrypt.hash(password, 12);
    const newUser = await UserModel.create({
      username,
      password: hashpassword,
    });
    req.session.user = {
      id: newUser._id.toString(),
      username: newUser.username,
    };
    return res.status(201).json({
      status: "success",
      data: {
        user: newUser,
      },
    });
  } catch (error) {
    return res.status(400).json({ status: "fail", error: error.message });
  }
};

export const login = async (req: Request, res: Response) => {
  const { username, password } = req.body;
  try {
    const user = await UserModel.findOne({ username });
    if (!user) {
      return res.status(404).json({
        status: "fail",
        message: "user not found",
      });
    }
    const passwordIsCorrect = await bcrypt.compare(password, user.password);
    if (passwordIsCorrect) {
      req.session.user = { id: user._id.toString(), username: user.username };
      return res.status(200).json({ status: "success" });
    } else {
      return res
        .status(400)
        .json({ status: "fail", message: "incorrect password" });
    }
  } catch (error) {
    return res.status(400).json({ status: "fail", error: error.message });
  }
};

export const logout = async (req: Request, res: Response) => {
  req.session.destroy((err) => {
    if (err) {
      return res.status(500).json({
        status: "fail",
        message: "Could not log out, please try again",
      });
    }
    res.clearCookie("connect.sid"); // 세션 쿠키 삭제
    return res
      .status(200)
      .json({ status: "success", message: "로그아웃이 성공적으로 완료되었습니다" });
  });
};
```

일반적으로 다음과 같이 작동합니다:

<div class="content-ad"></div>

- 사용자가 성공적으로 로그인하면 관련 사용자 정보(예: _id 및 username)를 req.session.user 객체에 설정합니다: req.session.user = ' id: user.\_id.toString(), username: user.username ';
- Redis 스토어를 사용하도록 구성된 세션 미들웨어가 다음 단계를 자동으로 처리합니다:
1. 세션 데이터(req.session.user 객체)를 직렬화하여 Redis 스토어에 저장하고 고유한 세션 ID와 연결합니다.
2. 고유한 세션 ID를 클라이언트 브라우저의 쿠키로 설정합니다. 이 쿠키는 일반적으로 connect.sid와 같은 이름으로 지정됩니다.
- 사용자가 이후 요청을 보낼 때마다 세션 미들웨어는 다음을 수행합니다:
1. 클라이언트의 쿠키(connect.sid 쿠키)에서 세션 ID를 추출합니다.
2. 세션 ID를 사용하여 Redis 스토어에서 해당 세션 데이터를 검색합니다.
3. 검색된 세션 데이터를 req.session 객체에 첨부하여 응용 프로그램 코드에서 액세스할 수 있도록 합니다.

# 보호된 경로를 위한 미들웨어

```js
import { Request, Response, NextFunction } from "express";

export const authMiddleware = (
  req: Request,
  res: Response,
  next: NextFunction
) => {
  if (req.session.user) {
    return next(); // 사용자가 인증되었습니다. 다음 미들웨어나 라우트 핸들러로 이동
  } else {
    return res.status(401).json({ status: "fail", message: "인가되지 않음" });
  }
};
```

```js
app.use("/api/v1/protected", authMiddleware, protectedRouter);
```

<div class="content-ad"></div>

축하합니다! express-session을 사용하여 세션을 구현하는 기본 사항을 배웠습니다. 이 단계를 따르면 ExpressJS 애플리케이션에서 세션을 효과적으로 처리하여 사용자 상태를 유지하고 사용자에게 맞춤 경험을 제공할 수 있습니다.