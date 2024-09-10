---
title: "Angular의 새로운 canActivateFn 가드 함수 사용 방법과 기능"
description: ""
coverImage: "/assets/img/2024-09-10-NewAngularcanActivateFnGuardfunction_0.png"
date: 2024-09-10 18:39
ogImage: 
  url: /assets/img/2024-09-10-NewAngularcanActivateFnGuardfunction_0.png
tag: Tech
originalTitle: "New Angular canActivateFn Guard function"
link: "https://medium.com/@meghanathblogger/new-angular-canactivatefn-guard-function-bce74f7d06ea"
isUpdated: false
---


canActivateFn은 Angular 14에서 소개된 기능적 라우트 가드입니다. 이는 전통적인 클래스 기반 CanActivate 가드와 동일한 목적을 제공하지만 클래스 대신 간단한 함수를 사용하여 가드 로직을 정의할 수 있습니다. 이 접근 방식은 특히 간단한 권한 확인에 대해 더 간결하고 가독성 있는 코드를 작성할 수 있게 해줍니다.


![NewAngular canActivateFn Guard](/assets/img/2024-09-10-NewAngularcanActivateFnGuardfunction_0.png)


auth.guard.ts

```js
import { CanActivateFn, Router } from '@angular/router';
import { inject } from '@angular/core';
import { AuthService } from './auth.service';
```


<div class="content-ad"></div>

```js
export const authGuard: CanActivateFn = (route, state) => {
  const authService = inject(AuthService);
  const router = inject(Router);
  if (authService.isAuthenticated()) {
    return true;
  } else {
    router.navigate(['/login']);
    return false;
  }
};
```

이 코드는 더 간결하고 가독성이 좋아요. 😊

Auth.service.ts

```js
// auth.service.ts
import { Injectable } from '@angular/core';
```

<div class="content-ad"></div>

```js
@Injectable({
  providedIn: 'root',
})
export class AuthService {
  private authenticated = false;
  isAuthenticated(): boolean {
    // 여기에 인증 로직을 구현하세요
    return this.authenticated;
  }
  login(): void {
    // 로그인 작업 수행
    this.authenticated = true;
  }
  logout(): void {
    // 로그아웃 작업 수행
    this.authenticated = false;
  }
}
```

그런 다음, Inject() 함수를 사용하여 Auth 서비스를 로드했습니다;

그런 다음, 파라미터 없이 새로운 AuthGuard 함수를 CanActivate 라우트 배열에 추가하십시오.

```js
const routes: Routes = [
  {
    path: '',
    component: DashboardComponent,
    canActivate: [AuthGuard],
  }
```

<div class="content-ad"></div>

auth.guard.spec.ts

```js
import { TestBed } from '@angular/core/testing';
import { Router } from '@angular/router';
import { authGuard } from './auth.guard';
import { AuthService } from './auth.service';
import { RouterTestingModule } from '@angular/router/testing';
```

```js
describe('authGuard', () => {
  let canActivateFn: CanActivateFn;
  let authServiceSpy: jasmine.SpyObj<AuthService>;
  let routerSpy: jasmine.SpyObj<Router>;
  
  beforeEach(() => {
    const authSpy = jasmine.createSpyObj('AuthService', ['isAuthenticated']);
    const router = jasmine.createSpyObj('Router', ['navigate']);
    
    TestBed.configureTestingModule({
      imports: [RouterTestingModule],
      providers: [
        { provide: AuthService, useValue: authSpy },
        { provide: Router, useValue: router }
      ]
    });
    
    canActivateFn = authGuard;
    authServiceSpy = TestBed.inject(AuthService) as jasmine.SpyObj<AuthService>;
    routerSpy = TestBed.inject(Router) as jasmine.SpyObj<Router>;
  });
  
  it('should allow access if user is authenticated', () => {
    authServiceSpy.isAuthenticated.and.returnValue(true);
    const result = canActivateFn(null, null);
    expect(result).toBeTrue();
    expect(authServiceSpy.isAuthenticated).toHaveBeenCalled();
    expect(routerSpy.navigate).not.toHaveBeenCalled();
  });
  
  it('should redirect to login if user is not authenticated', () => {
    authServiceSpy.isAuthenticated.and.returnValue(false);
    const result = canActivateFn(null, null);
    expect(result).toBeFalse();
    expect(authServiceSpy.isAuthenticated).toHaveBeenCalled();
    expect(routerSpy.navigate).toHaveBeenCalledWith(['/login']);
  });
});
```

Thank you!