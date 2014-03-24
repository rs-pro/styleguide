# RocketScience Styleguide


## Основное

* Будь последователен.
* Не переписывай существующий код чтобы он соответствовал этому стайлгайду.
* Не нарушай стайлгайд без хорошей причины.
* Причина хороша, если ты можешь убедить других что она хороша.

## Язык

"Избегай" значит не делай без хорошей причины.

"Никогда" значит хорошей причины не существует.

## Git

* Не коммитьте мерджи - используйте [rebase workflow] или в простейшем случае [rebase]
* Пишите качественные, внятные [коммит-сообщения].
* Каждый коммит должен содержать время, потраченное на него, в круглых скобках или без них, в самом конце текста (можно на новой строчке, чтобы не засорять лог), в формате (2h) или 5m

[rebase]: https://github.com/rs-pro/styleguide/wiki/Git-Rebase
[rebase workflow]: http://mettadore.com/analysis/a-simple-git-rebase-workflow-explained/
[коммит-сообщения]: http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html

## Формат (общее)

* Избегай inline комментариев
* Избегай строк длиннее 80 символов.
* Избегай файлов больше 100 строк кода (не считая пустых строк и комментариев).
* Используй SASS @import\@mixin, concerns и т.д.
* Удаляй пробелы в конце всех строк.
* Используй Unix-style line endings (`\n`) ([git eol]).
* Добавляй пустую строку в конце файла [eof]
* Не делай орфографических ошибок, особенно в английских названиях переменных. Если не знаете перевод, посмотрите его [в словаре].
* Не выравнивай символы друг с другом на разных строчках.
* Если разбиваешь список аргументов, каждый аргумент должен быть на своей строчке, также как и закрывающая скобка.
* Если разбиваешь хэш, каждый элемент и закрывающая скобка должны быть на своей строчке.
* Используй 2 пробела для оступов (не используй табы).

[git eol]: https://help.github.com/articles/dealing-with-line-endings
[в словаре]: http://slovari.yandex.ru/
[dot guideline example]: https://github.com/thoughtbot/guides/blob/master/style/samples/ruby.rb#L11
[eof]: http://unix.stackexchange.com/questions/23903/should-i-end-my-text-script-files-with-a-newline

## Ruby

* Отделяй многострочные блоки 1 пустой строкой с обоих сторон.
* Use spaces around operators, after commas, colons and semicolons, around { and before }.

```ruby
sum = 1 + 2
a, b = 1, 2
1 > 2 ? true : false; puts "Hi"
[1, 2, 3].each { |e| puts e }
```

* Не добавляй пробелы после `(`, `[` или перед `]`, `)`.

```ruby
some(arg).other
[1, 2, 3].length
```

* Не добавляй пробел после !.

```ruby
!array.include?(element)
```

* Indent when as deep as case.

```ruby
case
when song.name == "Misty"
  puts "Not again!"
when song.duration > 120
  puts "Too long!"
when Time.now.hour > 21
  puts "It's too late"
else
  song.play
end

kind = case year
       when 1850..1889 then "Blues"
       when 1890..1909 then "Ragtime"
       when 1910..1929 then "New Orleans Jazz"
       when 1930..1939 then "Swing"
       when 1940..1950 then "Bebop"
       else "Jazz"
       end
```

* Отделяй методы 1 пустой строкой и чтобы разделить метод на параграфы.

```ruby
def some_method
    data = initialize(options)

    data.manipulate!

    data.result
end

def some_method
    result
end
```

[дополнительно](https://github.com/styleguide/ruby)

## CSS

* Используй SASS синтакс
* Используй SCSS синтакс для файлов в css синтаксисе, которые ты не хочешь конвертировать
* Все ссылки на внешние картинки - строго image-url вместо url
* Никогда не используй ```#= require_tree .```

### CSS ресет

* Хорошие CSS ресеты:
```css
*
  margin: 0
  padding: 0
```

[normalize](http://necolas.github.io/normalize.css/)

### Организация кода

Пример хорошей организации CSS:

    stylesheets
    ├── mixins
    │   ├── browser_helpers.css.sass
    │   ├── responsive_helpers.css.sass
    │   └── variables.css.sass
    ├── plugins
    │   ├── jquery.fancybox-1.3.4.css
    │   └── reset.css.sass
    ├── pages
    │   ├── issues.css.sass
    │   └── profile.css.sass
    ├── extra
    │   └── editor.css.sass
    ├── shared
    │   ├── forms.css.sass
    │   └── markdown.css.sass
    ├── application.css.sass
    └── ckcontent.css.sass
    
**Весь CSS, использующийся только на одной конкретной странице, должен быть в отдельном файле, соответствующем этой странице. Все селекторы, относящиеся к конкретной странице, должны начинаться с класса ```.#{params[:controller]}_#{params[:action]}```**

SASS\SCSS файлы необходимо всегда загружать через @import, простые CSS файлы - через require. В простом CSS недопустимы ссылки на картинки и другие внешние файлы, кроме случая когда они лежат в папке ```public/```

```sass
#= require_tree ./plugins
#= require my_awesome_styles

@import ../globals/basic

.rule { ... }
```

### Названия классов

Никогда не используй в CSS классы с префиксом ```js-```. js- только для JS файлов.

Используй префикс is- для классов состояния которые разделяются между CSS и JS.

## Специфичность (классы vs. ids)

Элементы, которые встречаются на странице всегда строго 1 раз, должны использовать ID, иначе классы. Если сомневаетесь, используйте класс.

Хорошие кандидаты в id: header, footer, уникальные модальные окна.
Плохие: navigation, item listings, item view pages (ex: issue view).

When styling a component, start with an element + class namespace (prefer class names over ids), prefer direct descendant selectors by default, and use as little specificity as possible. Here is a good example:

    <ul class="category-list">
      <li class="item">Category 1</li>
      <li class="item">Category 2</li>
      <li class="item">Category 3</li>
    </ul>
    ul.category-list { // element + class namespace
      &>li { // direct descendant selector > for list items
        list-style-type: disc;
      }
      a { // minimal specificity for all links
        color: #ff0000;
      }
    }

### CSS Specificity guidelines

If you must use an id selector ```(#selector)``` make sure that you have no more than one in your rule declaration. A rule like ```#header .search #quicksearch { ... }``` is considered harmful.
When modifying an existing element for a specific use, try to use specific class names. Instead of ```.listings-layout.bigger use rules like .listings-layout.listings-bigger.``` Think about ack/greping your code in the future.
The class names disabled, mousedown, danger, hover, selected, and active should always be namespaced by a class (```button.selected``` is a good example).


## Источники

[thoughtbot styleguide](https://github.com/thoughtbot/guides)

[github styleguide](https://github.com/styleguide)

[ruby style guide](https://github.com/bbatsov/ruby-style-guide)
