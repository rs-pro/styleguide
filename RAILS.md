## Общее

* В роутах в случае использования ресурсов всегда включай только нужные action'ы
```resources :news, only: [:index, :show]```
* Названия всех моделей и всех полей моделей, видимые в интерфейсе сайта или админке, должны быть переведены на язык сайта
* Перевод моделей и полей моделей должен находиться в ```config/locales/ru.models.yml```

## Требования к rails фронтэнду

* В шаблонах, все ссылки на внешние картинки - строго image_tag
* Картинки-заглушки лежат в /public/tmp/my_pic.png, image_tag "/tmp/my_pic.png"
* Картинки со смысловой нагрузкой лежат в /app/assets/images/my_pic.png, image_tag "my_pic.png"
* Все меню - строго через [simple_navigation](https://github.com/codeplant/simple-navigation)
* Все формы больше 2 полей - строго через simple_form, без дополнительных классов и изврата.
Пример как никогда не делать 
```slim
# ОЧЕНЬ ПЛОХО
= simple_form_for @company, url: company_path(@company) do |c|
  .profile-setting-input
    = c.input :middle_name, class: 'form-basic__input--name', placeholder: "Отчество"
  .profile-setting-input
    = c.input :passport, class: 'form-basic__input--number', placeholder: "Номер паспорта"
```
Пример правильной реализации:
```slim
# Хорошо: код убран в кастомный form builder
= simple_form_for @contact_message, builder: MinimalFormBuilder, url: faq_index_path  do |f|
 = c.input :middle_name, icon: "name"
 # Хорошо: названия полей убраны в i18n
 = c.input :passport, icon: "number"
```

Пример i18n:
```yaml
# config/ru.models.yml
ru:
  activerecord:
    models:
      driver:
        instance: Техника
        name: ФИО
        company: Компания
```
Пример кастомного form builder (заменяет label на placeholder):
```ruby
# extra/minimal_form_builder.rb
class MinimalFormBuilder < SimpleForm::FormBuilder
  include ActionView::Helpers::TranslationHelper

  def input(attribute_name, options = {}, &block)
    type = options.fetch(:as, nil) || default_input_type(
      attribute_name,
      find_attribute_column(attribute_name),
      options
    )

    input = find_input(attribute_name, options, &block)

    if type != :boolean
      if options[:placeholder].nil?
        attr_human_name = if object.class.respond_to?(:human_attribute_name)
          object.class.human_attribute_name(attribute_name.to_s)
        else
          attribute_name.to_s.humanize
        end
        options[:placeholder] ||= input.send(:translate_from_namespace, :placeholders, attr_human_name)
      end
      options[:label] = false if options[:label].nil?
    end

    super
  end
end
```
Пример добавления иконок к полям:
```ruby
# config/initializers/simple_form.rb
module SimpleForm
  module Components
    module Icon
      def icon
        if options[:icon].present?
          if options[:tooltip].present?
            tooltip = options[:tooltip]
            tooltip_content = tooltip.is_a?(String) ? tooltip : translate(:tooltips)
            tooltip_content.html_safe if tooltip_content
          else
            tooltip_content = nil
          end

          template.content_tag(:span, "", class: "input-icon input-icon-#{options[:icon]}", title: tooltip_content)
        end
      end
    end
  end
end

SimpleForm::Inputs::Base.send(:include, SimpleForm::Components::Icon)

SimpleForm.setup do |config|
  config.wrappers :default, class: :input,
    hint_class: :field_with_hint, error_class: :field_with_errors do |b|
    ....
    b.use :icon
  end
end
```
Ещё:
https://github.com/plataformatec/simple_form
https://github.com/plataformatec/simple_form/wiki

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
