---
title: "Passport.js의 로그인 확인 절차, authenticate는 로그인에서 사용하자."
date: 2021-10-15 12:00
categories: js
---

passport.authenticate는 form의 username과 password가 같이 전달되었다고 가정하고 코드가 실행합니다.

```
app.post('/login', passport.authenticate('local', { successRedirect: '/',
                                                    failureRedirect: '/login' }));
```

그래서 로그인의 경우가 딱들어맞는 경우입니다.
`로그인을 했는지 안했는지`를 확인하려고 아래과 같은 코드를 사용한다면 항상 failureRedirect가 실행이 될 것입니다.

```
app.get('/member-only', passport.authenticate('local', {failureRedirect: '/login'}), (req,res) => {
    // 로그인을 이미 한 경우에만 실행할 코드
});
```

즉 위의 코드는 잘못된 코드입니다. 로그인 확인 여부는 `req.isAuthenticated()`를 사용하면 됩니다. 미들웨어를 2개 만들어서 편하게 사용해봅시다.

```

// 미들웨어
// 로그인한 유저만 다음 단계를 실행하고, 로그인안 한 유저는 로그인 페이지로 이동
isLoggedIn = (req, res, next) => {
    if (req.isAuthenticated()) {
        next();
    } else {
        res.redirect('/login'); // 로그인 페이지로 이동
    }
}
// 로그인 안한 유저만 다음 단계를 실행하고, 로그인 한 유저는 해당 페이지로 접근을 못하게 한다.
isNotLoggedIn = (req, res, next) => {
    if (!req.isAuthenticated()) {
        next();
    } else {
        res.redirect('/'); // 홈페이지로 이동
    }
}


app.get('/register', isNotLoggedIn, (req,res) => {
    // 회원가입 페이지는, 로그인을 안한 유저만 접근 가능
});
```
