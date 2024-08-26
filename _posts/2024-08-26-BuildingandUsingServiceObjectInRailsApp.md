---
title: "Rails 앱에서 서비스 객체를 구축하고 활용하는 방법"
description: ""
coverImage: "/assets/img/2024-08-26-BuildingandUsingServiceObjectInRailsApp_0.png"
date: 2024-08-26 19:09
ogImage: 
  url: /assets/img/2024-08-26-BuildingandUsingServiceObjectInRailsApp_0.png
tag: Tech
originalTitle: "Building and Using Service Object In Rails App"
link: "https://medium.com/@Iggy01/building-and-using-service-object-in-rails-app-f922df8b0153"
isUpdated: false
---


아래는 당신의 Rails 어플리케이션이 비즈니스 로직이 컨트롤러와 모델에 서서히 들어가는 것과 관련하여 원활히 진행되고 있다는 것을 보여줍니다. 이것들은 시스템의 기본 구성 요소이며, 여기서 서비스 객체가 주요한 역할을 합니다. 그들의 주요 관심사는 코드베이스 내에서 순서를 유지하는 것입니다. 이제 여러분에게 Rails 어플리케이션에서 서비스 객체를 구현하는 방법을 보여드리겠습니다.

## 왜 우리는 서비스 객체를 정당화할까요:

그러나 약간의 배경 지식은 나중에 참고하기 위한 참조로 매우 도움이 되겠습니다. 서비스 객체 및 유사한 개념이 왜 필요한지에 대해 간단히 설명하겠습니다. 이들은 여러분에게 다음과 같은 기능을 제공합니다:

<div class="content-ad"></div>

- 역할 분리 된 비즈니스 로직: 컨트롤러와 모델은 각각의 가장 중요한 기능에만 집중해야 합니다.
- 테스트 용이성 향상: 비즈니스 로직을 쉽게 테스트하거나 상수를 노출시키지 않고 테스트를 작성할 수 있습니다.
- 투명성 향상: 자체 문서화가 된 깔끔한 코드를 작성하여 프로젝트 수명 동안 유지 보수하기 쉽게 만듭니다!

서비스 객체 — 서비스 객체는 하나의 책임만을 가지도록 의도되었거나 다른 말로 하면 한 가지만을 수행하거나 일부 업무 지식을 포함해야 합니다. 이는 깔끔하고 정리가 잘 된 코드를 유지하는 많은 층위가 있는 응용 프로그램에 대해 매우 유용한 도구가 됩니다.

## 단계 1: 설정

이미 Rails 앱이 없는 경우 새로 만들어 보겠습니다. 터미널을 열고 다음 명령을 실행하세요:

<div class="content-ad"></div>

```js
rails new service_objects_demo
cd service_objects_demo
```

이렇게 하면 service_objects_demo라는 이름의 새로운 Rails 애플리케이션이 생성되고 프로젝트 디렉토리로 이동합니다.

## 단계 2: 서비스 객체 생성

유저 생성을 위한 서비스 객체를 생성해보겠습니다. CreateUserService라고 부르겠습니다. 먼저 서비스 객체를 위한 디렉토리가 있는지 확인해주세요. 아직 없다면 생성해주세요:

<div class="content-ad"></div>


```js
mkdir app/services
```

이제, CreateUserService 클래스를 만들어보겠습니다. 이 서비스는 사용자를 생성하는 논리를 처리할 것입니다:

```js
# app/services/create_user_service.rb
class CreateUserService
  def initialize(user_params)
    @user_params = user_params
  end

  def call
    user = User.new(@user_params)
    if user.save
      user
    else
      user.errors.full_messages
    end
  end
end
```

여기서 CreateUserService 클래스는 사용자 매개변수를 인수로 사용하여 새로운 사용자를 만들려고 시도하고 성공하면 사용자를 반환하거나 실패하면 오류 메시지를 반환합니다.


<div class="content-ad"></div>

## 단계 3: 서비스 객체를 컨트롤러에서 사용하기

이제 서비스 객체를 UsersController에서 활용해 봅시다. 다음은 create 액션에서 사용하는 방법입니다:

```js
# app/controllers/users_controller.rb
class UsersController < ApplicationController
  def create
    service = CreateUserService.new(user_params)
    result = service.call

    if result.is_a?(User)
      render json: result, status: :created
    else
      render json: { errors: result }, status: :unprocessable_entity
    end
  end

  private

  def user_params
    params.require(:user).permit(:name, :email, :password, :password_confirmation)
  end
end
```

서비스 객체를 사용함으로써 컨트롤러의 create 액션은 주로 HTTP 요청 및 응답을 처리하는 주요 역할에 더 집중할 수 있습니다.

<div class="content-ad"></div>

## 단계 4: 더 많은 서비스 객체 추가하기

이제, 다른 사용자 관련 작업을 처리하기 위해 몇 가지 더 많은 서비스 객체를 생성해 봅시다: ShowUserService, UpdateUserService, 그리고 ForgotPasswordService.

ShowUserService:

```js
# app/services/show_user_service.rb
class ShowUserService
  def initialize(user_id)
    @user_id = user_id
  end

  def call
    User.find(@user_id)
  end
end
```

<div class="content-ad"></div>

UpdateUserService:

```js
# app/services/update_user_service.rb
class UpdateUserService
  def initialize(user, user_params)
    @user = user
    @user_params = user_params
  end

  def call
    if @user.update(@user_params)
      @user
    else
      @user.errors.full_messages
    end
  end
end
```

ForgotPasswordService:

```js
# app/services/forgot_password_service.rb
class ForgotPasswordService
  def initialize(email)
    @user = User.find_by(email: email)
  end

  def call
    if @user
      @user.send_reset_password_instructions
      "Reset password instructions sent."
    else
      "User not found."
    end
  end
end
```

<div class="content-ad"></div>

## 단계 5: 컨트롤러에서 서비스 객체 사용하기

새로운 서비스 객체를 사용하도록 컨트롤러를 업데이트하세요. UsersController에서 사용자를 표시하고 업데이트하는 방법은 다음과 같습니다:

```js
# app/controllers/users_controller.rb
class UsersController < ApplicationController
  def show
    service = ShowUserService.new(params[:id])
    user = service.call
    render json: user
  end

  def update
    service = UpdateUserService.new(current_user, user_params)
    result = service.call

    if result.is_a?(User)
      render json: result, status: :ok
    else
      render json: { errors: result }, status: :unprocessable_entity
    end
  end
end
```

비밀번호를 잊어버렸을 때의 기능을 위해 새로운 또는 기존 컨트롤러를 사용하세요.

<div class="content-ad"></div>


# app/controllers/passwords_controller.rb
class PasswordsController < ApplicationController
  def create
    service = ForgotPasswordService.new(params[:email])
    message = service.call
    render json: { message: message }
  end
end


## Step 6: Testing Service Objects

Service objects are great for testing because they encapsulate specific pieces of business logic. Here’s how you might test CreateUserService with RSpec:


# spec/services/create_user_service_spec.rb
require 'rails_helper'

RSpec.describe CreateUserService, type: :service do
  let(:user_params) { { name: 'John Doe', email: 'john@example.com', password: 'password', password_confirmation: 'password' } }

  context 'when valid parameters are provided' do
    it 'creates a new user' do
      service = CreateUserService.new(user_params)
      result = service.call

      expect(result).to be_a(User)
      expect(result).to be_persisted
    end
  end

  context 'when invalid parameters are provided' do
    it 'returns errors' do
      user_params[:email] = ''
      service = CreateUserService.new(user_params)
      result = service.call

      expect(result).to be_an(Array)
      expect(result).to include("Email can't be blank")
    end
  end
end


<div class="content-ad"></div>

## 서비스 객체 사용 최상의 방법

서비스 객체는 강력한 도구이지만, 최상의 방법을 따르는 것이 중요합니다. 여러분이 그들을 최대한 활용하기 위해서는 다음을 준수해야 합니다:

- 단일 책임 원칙: 각 서비스 객체가 한 가지 구체적인 작업을 처리하도록 보장하세요. 이렇게 하면 이해하기 쉽고 유지 보수 및 테스트가 용이해집니다.
- 명확한 명명: 서비스 객체를 명확하고 일관되게 명명하세요. CreateUserService, UpdateUserService 등과 같이 명확한 이름을 사용하세요.
- 오류 처리: 서비스 객체 내에서 오류를 적절히 처리하세요. 의미 있는 오류 메시지를 반환하여 컨트롤러가 사용자에게 적절한 피드백을 제공할 수 있도록 하세요.

테스트: 서비스 객체에 대해 포괄적인 테스트를 작성하여 예상대로 작동하는지 확인하세요. 엣지 케이스 및 오류 조건을 포함한 다양한 시나리오를 테스트하세요.

<div class="content-ad"></div>

# 결론

이 가이드를 따라가면 Rails 애플리케이션에서 서비스 객체를 만들고 사용하는 방법을 배우게 되어 코드를 더 깔끔하고 유지보수하기 쉽게 만들 수 있습니다. 서비스 객체는 컨트롤러와 모델이 주요 역할에 집중할 수 있도록 도와주며, 테스트 가능성을 향상시키고 가독성을 향상시킵니다.

즐거운 코딩 되세요!