### 오늘은 페이지를 국제화 하는 방법에 대해 진행하겠습니다 !

> 1. 새로운 프로젝트 만들기
>
> ```
> $ rails new locale_practice
>
> ```
>
> 1. Gem 추가하기
>
> ```
> # Gemfile
> gem 'devise'
> gem 'devise-i18n'
> ```
>
> - gem 설치하기
>
> ```
> $ bundle install
>
> ```
>
> 1. devise 추가하기
>
> ```
> $ rails g devise:install
>
> ```
>
> - development.rb 수정하기
>
> ```
> config.action_mailer.default_url_options = { host: 'localhost', port: 3000 }
> ```
>
> - devise User 생성하기
>
> ```
> $ rails g devise user
>
> ```
>
> - devise View 생성하기
>
> ```
> $ rails g devise:i18n:views
>
> ```
>
> 1. scaffold 만들기
>
> ```
> $ rails g scaffold Post title content:text
>
> ```
>
> 1. i18n 사용하기
>
> ```
> # config/application.rb 
> module LocalPractice
>  class Application < Rails::Application
>    config.time_zone = 'Seoul'
>    config.i18n.default_locale = :ko
>  end
> end
> ```
>
> - config/locales/devise.en.yml -> ko로 수정하는 방법
>
> ```
> # https://github.com/tigrish/devise-i18n 여기에 들어가보면 ko로 작성되어 있는 문서가 있다 !
> # 우리는 그거를 가져와서 사용할 꺼임 !
>
> $ rails g devise:i18n:locale ko
>
> ```
>
> - config/locales/ko.yml 만들기
>
> ```
> ko:
>  hello: "안녕하세요."
> ```
>
> - posts/index.html.erb 수정하기
>
> ```
>
> ```
>
> 1. scaffold i18n 사용하기
>
> ```
> # Gemfile 추가하기
> gem 'i18n_generators'
>
> $ bundle
> ```
>
> - ko 추가하기
>
> ```
> rails g i18n ko
>
> ```
>
> - posts/index.html.erb
>
> ```
> <p><%= I18n.locale.to_s %></p>
> <!-- 내가 어떤 로케일을 사용하는지 확인하는 코드 -->
> <p><%= t('.hello') %></p>
> <tr>
>  <th><%= t('.title') %></th>
>  <th><%= t('.content') %></th>
>  <th colspan="3"></th>
> </tr>
> ```
>
> ### 버튼을 눌러 한국어, 영어 페이지를 만들자 !
>
> 1. routes.rb 수정하기
>
> ```
> scope "(:locale)", locale: "/ko|en/" do
>  resources :posts
> end
> ```
>
> 1. ko, en 수정하기
>
> ```
> en:
>  posts:
>    index:
>      list: Posts List
>      hello: Hello
>      title: Title
>      content: Content
>      new: Write a new post
>
> # config/locales/translation_ko.yml
> posts:
>  index:
>    list: 게시물
>    hello: 안녕하세요
>    title: 제목
>    content: 내용
>    new: 새글쓰기
>
> ```
>
> 1. app/controllers/application_controller.rb
>
> ```
> class ApplicationController < ActionController::Base
>  # Prevent CSRF attacks by raising an exception.
>  # For APIs, you may want to use :null_session instead.
>  protect_from_forgery with: :exception
>  before_action :set_locale
>
>  def set_locale
>    I18n.locale = params[:locale]
>  end
> end
> ```
>
> 1. app/views/layouts/application.html.erb 수정
>
> ```
> <body>
>  <%= link_to "한국어", url_for(locale: :ko) %>
>  <%= link_to "English", url_for(locale: :en) %>
> </body>
> ```
>
> 1. app/views/posts/index.html.erb 수정
>
> ```
> <h1><%= t('.list') %></h1>
> <table>
>  <thead>
>    <tr>
>      <th><%= t('.title') %></th>
>      <th><%= t('.content') %></th>
>      <th colspan="3"></th>
>    </tr>
>  </thead>
> ```

### activeadmin 을 사용해보자 !

<https://activeadmin.info/documentation.html> 문서 참고 !

> 1. Getting Started
>
> ```
> # Gemfile
> gem 'activeadmin'
> ```
>
> 1. active_admin 설치하기
>
> ```
> $ rails generate active_admin:install
>
> ```
>
> 1. Migrate your db and start the server:
>
> ```
> $ rake db:migrate
> $ rs
>
> ```
>
> - admin 유저 사용하기
>
> ```
> $ rake db:seed
>
> ```
>
> - Visit
>
>    
>
>   ```
>   http://localhost:3000/admin
>   ```
>
>    
>
>   and log in using:
>
>   - **User**: [admin@example.com](mailto:admin@example.com)
>   - **Password**: password
>
> - 모델의 admin 페이지를 만들고 싶으면 !
>
> ```
> $ rails generate active_admin:resource [MyModelName]
> ```