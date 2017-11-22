## Общее

* В роутах в случае использования ресурсов всегда включай только нужные action'ы
```resources :news, only: [:index, :show]```
* Названия всех моделей и всех полей моделей, видимые в интерфейсе сайта или админке, должны быть переведены на язык сайта
* Перевод моделей и полей моделей должен находиться в ```config/locales/ru.models.yml```

## Требования к rails фронтэнду

* В шаблонах, все ссылки на внешние картинки - строго image_tag
* Картинки-заглушки лежат в /public/tmp/my_pic.png, image_tag "/tmp/my_pic.png"
* Картинки со смысловой нагрузкой лежат в /app/assets/images/my_pic.png, image_tag "my_pic.png"

### Организация

Все стили и js находятся в webpack и описаны в общем разделе. Asset Pipeline не используется (кроме active admin, rails admin).

### Организация CSS

Пример хорошей организации CSS:

    app/assets/stylesheets
    ├── application.css (в идеале - пустой)
    └── ckcontent.css # CSS для страницы в CKEditor в админке


### Организация JS

Пример хорошей организации JS:

    javascripts
    ├── action_cable.js (код специфичный для action cable)
    └── application.js (в идеале - пустой, допустимо использовать для i18n данных и т.д.)


## Turbolinks

* Для всех новых проектов используй Turbolinks.

## Rails ujs

* Для всех новых проектов используй rails-ujs из npm.

## RocketCMS

Все настройки rocket_cms (в т.ч. патчи моделей) должны быть строго в файле ```config/initializers/rocket_cms.rb```
