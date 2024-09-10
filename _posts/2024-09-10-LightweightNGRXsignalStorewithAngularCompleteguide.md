---
title: "Angular에서 가벼운 NGRX signalStore 사용하기 완벽한 가이드"
description: ""
coverImage: "/assets/img/2024-09-10-LightweightNGRXsignalStorewithAngularCompleteguide_0.png"
date: 2024-09-10 18:35
ogImage: 
  url: /assets/img/2024-09-10-LightweightNGRXsignalStorewithAngularCompleteguide_0.png
tag: Tech
originalTitle: "Lightweight NGRX signalStore with Angular. Complete guide."
link: "https://medium.com/itnext/lightweight-ngrx-signalstore-with-angular-complete-guide-af1c39c01276"
isUpdated: false
---


이 기사에서는 ngrx 신규 signalStore를 사용하여 기본 CRUD 작업을 수행하는 방법에 대한 완전한 가이드를 보여드리겠습니다.

![image](/assets/img/2024-09-10-LightweightNGRXsignalStorewithAngularCompleteguide_0.png)

# NGRX signalStore란 무엇인가요?

# CRUD는 무엇의 약자인가요?

<div class="content-ad"></div>

# 시작하기

저는 간단한 CRUD 작업을 처리하기 위해 NGRX를 사용하는 방법을 보여주기 위해 Angular 프로젝트를 처음부터 생성할 것입니다. Angular 응용 프로그램을 만든 후에 NGRX에서 필요한 모든 패키지를 설치할 것입니다. 아래에 설명된 단계를 따라 주세요:

- npm에서 전역 패키지로 Angular CLI를 설치 → 터미널을 열고 다음 명령을 실행하세요:

```js
npm i -g @angular/cli
```

<div class="content-ad"></div>

2. 이제 새로운 Angular 프로젝트를 생성해 봅시다 → 다음 명령어를 실행하면 라우팅을 추가할지 물어보게 됩니다 (난 수락했어요, 초기 라우트를 구성해 줍니다) 그리고 선호하는 스타일시트 형식을 선택할 수 있습니다 (나는 SASS를 좋아해서 SCSS를 사용하기로 선택했어요).

```js
ng new crud-ngrx-signals-angular
```

3. 선호하는 IDE에서 프로젝트를 열고 비즈니스에 들어가 봅시다.

# NGRX 신호 패키지 설치하기

<div class="content-ad"></div>

ngrx에서 제공하는 signalStore를 사용하려면 ngrx에서 제공하는 패키지가 필요합니다. 시작해 봅시다. 새 터미널을 열고 다음을 실행하세요.

```js
ng add @ngrx/signals@latest
```

또는 npm을 사용하고 싶다면:

```js
npm install @ngrx/signals --save
```

<div class="content-ad"></div>

참고로 전체 문서는 여기에서 확인하실 수 있습니다.

# 구현

이 예제를 쇼케이스로 사용하기 위해 매우 간단한 온라인 예약 스토어를 CRUD 작업과 함께 사용하겠습니다. 따라서 책 목록을 가져오거나, 새 책을 만들거나, 기존 책 이름을 업데이트하거나, 물론 삭제할 수 있는 페이지를 만들겠습니다.

- 간단한 'book.interface.ts'라는 책 인터페이스를 보관할 폴더를 만들어보겠습니다.

<div class="content-ad"></div>


이제 Book signalStore를 book.store.ts라는 파일에 만들어 보겠습니다.


<div class="content-ad"></div>

```js
import {Book} from "@app/book.interface";
import {patchState, signalStore, withHooks, withMethods, withState} from "@ngrx/signals";
import {inject} from "@angular/core";
import {pipe, tap} from "rxjs";
import {rxMethod} from "@ngrx/signals/rxjs-interop";
import {switchMap} from "rxjs/operators";
import {tapResponse} from "@ngrx/operators";
import {BookService} from "@app/book.service";

type BookStoreState= {
    isLoading: boolean;
    books: Book[];
}

const initialState: BookStoreState = {
    isLoading: false,
    books: [],
}

export const BookStore = signalStore(
    {providedIn: 'root'},
    withState(initialState),
    withMethods((store, service = inject(BookService)) => ({
        getBooks: rxMethod<void>(pipe(
            tap(() => patchState(store, {isLoading: true})),
            switchMap(() => service.getBooks().pipe(
                tapResponse({
                    next: (books) => patchState(store, {books}),
                    error: error => console.log(error),
                    finalize: () => patchState(store, {isLoading: false}),
                })
            ))
        )),
        createBook: rxMethod<Book>(pipe(
            tap(() => patchState(store, {isLoading: true})),
            switchMap((bookToCreate) => service.create(bookToCreate).pipe(
                tapResponse({
                    next: (book) => patchState(store, {
                        books: [...store.books(), book],
                    }),
                    error: error => console.log(error),
                    finalize: () => patchState(store, {isLoading: false}),
                })
            ))
        )),
        updateBook: rxMethod<Book>(pipe(
            tap(() => patchState(store, {isLoading: true})),
            switchMap((bookToUpdate) => service.update(bookToUpdate).pipe(
                tapResponse({
                    next: (book) => patchState(store, {
                        books: store.books().map((b) => b.id === book.id ? book : b),
                    }),
                    error: error => console.log(error),
                    finalize: () => patchState(store, {isLoading: false}),
                })
            ))
        )),
        deleteBook: rxMethod<Book>(pipe(
            tap(() => patchState(store, {isLoading: true})),
            switchMap((bookToDelete) => service.delete(bookToDelete).pipe(
                tapResponse({
                    next: (book) => patchState(store, {
                        books: store.books().filter(b => b.id !== book.id)
                    }),
                    error: error => console.log(error),
                    finalize: () => patchState(store, {isLoading: false}),
                })
            ))
        )),
    }))
);
```

4. 컴포넌트 로직 구현을 살펴보겠습니다. Observable ngrx 스토어에서 마이그레이션하는 것으로 주목해야 할 점은 코드가 가벼워졌다는 점입니다. 더 이상 해체 구독이 필요하지 않으며, 보일러플레이트도 없습니다. 간단한 함수만 있습니다.

```js
import { Component, OnInit } from '@angular/core';
import { Observable } from 'rxjs';
import { Book } from '@app/book.interface';
import { select, Store } from '@ngrx/store';

@Component({
    selector: 'app-root',
    templateUrl: './app.component.html',
    styleUrls: ['./app.component.scss']
})
export class AppComponent {
    public isLoading: Signal<boolean> = computed(() => this.bookStore.isLoading());
    public books: Signal<Book[]> = computed(() => this.bookStore.books());
    private bookStore = inject(BookStore);

    constructor() {
       effect(() => {
            this.bookStore.getBooks();
        }, {allowSignalWrites: true})
    }

    public onCreateBook(book: Book): void {
        this.bookStore.createBook(book);
    }

    public onUpdateGroup(book: Book): void {
        this.bookStore.updateBook(book);
    }

    public onDeleteGroup(book: Book): void {
        this.bookStore.deleteBook(book);
    }
}
```

5. 컴포넌트 HTML


<div class="content-ad"></div>

```javascript
<div class="header">
    <input #inputElement>
    <button (click)="onCreateBook(inputElement.value)">Create book</button>
</div>
<div class="container">
    | ID | Name | Action |
    | --- | --- | --- |
    | <tbody>
        <tr *ngFor="let book of books()">
            <td>{book.id}</td>
            <td>{book.name}</td>
            <td><button (click)="onDeleteBook(book)">Delete</button></td>
        </tr>
        </tbody> |
</div>
```

# 여기까지입니다

항상 감사합니다. 내 내용과 이야기를 읽어주셔서 감사합니다. 이 기사가 유용하게 활용되기를 바랍니다.

곧 또 방문해주시고 즐거운 코딩하세요!