# RocketScience Styleguide

[Требования к макетам, прием в верстку](PSD.md)

[Требования к фронтэнду](FRONTEND.md)

[Требования к верстке и бэкэнду для Ruby On Rails](RAILS.md)

Для PHP кода используй стайлгайд [PSR-2](http://www.php-fig.org/psr/psr-2/)

## Основное

* Не переписывай существующий код чтобы он соответствовал этому стайлгайду.
* Не нарушай стайлгайд без хорошей причины.
* Причина хороша, если ты можешь убедить других что она хороша.

## Язык

"Избегай" значит не делай без хорошей причины.

"Никогда" значит хорошей причины не существует.

## Git

* Не коммитьте мерджи - используйте [rebase workflow] или в простейшем случае [rebase]
* Пишите качественные, внятные [коммит-сообщения].
* Автор коммита должен однозначно идентифицироваться. ```Your Name```, ```Developer```, ```PC``` - плохие варианты
```sh
git config --global user.name "Фамилия Имя либо login из Gitlab"
git config --global user.email ВашEmailНаКоторыйGitlab
```
* Каждый коммит должен содержать время, потраченное на него, в круглых скобках или без них, в самом конце текста (можно на новой строчке, чтобы не засорять лог), в формате (2h) или 5m
* Указывай в коммит-сообщении номер задачи. Закрывай задачи из коммитов. Пример: ```модели page, project fixes #123 10m```, "поправлена страница регистрации #20 2h"
* В Git сохраняются UNIX права доступа к файлу. Правильный способ их сбросить в нормальное состояние:
```sh
find . -type d -exec chmod 755 {} +
find . -type f -exec chmod 644 {} +
chmod +x bin/*
```
* Никогда не коммить файлы, которые создает твоя ОС, IDE или еще что-то (см [global gitignore])
* Обязательно периодически ребейзить вашу ветку на мастер
* Конфликты мерджа решает автор кода который не в мастере.

[rebase]: https://github.com/rs-pro/styleguide/wiki/Git-Rebase
[rebase workflow]: http://mettadore.com/analysis/a-simple-git-rebase-workflow-explained/
[коммит-сообщения]: http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html
[global gitignore]: https://gist.github.com/subfuzion/db7f57fff2fb6998a16c

## Формат (общее)

* Избегай inline комментариев
* Избегай строк длиннее 80 символов.
* Избегай файлов больше 100 строк кода (не считая пустых строк и комментариев).
* Удаляй пробелы в конце всех строк.
* Используй Unix-style line endings (`\n`) ([git eol]).
* Добавляй пустую строку в конце файла [eof]
* Не делай орфографических ошибок, особенно в английских названиях переменных. Если не знаете перевод, посмотрите его [в словаре] или [google translate]. Это затрудняет поиск по коду и дальнейшую работу с кодом.
* Не выравнивай символы друг с другом на разных строчках.

```Ruby
# плохо
fruits = {
  apple:  1,
  pear:   2,
  banana: 3,
}

# хорошо
fruits = {
  apple: 1,
  pear: 2,
  banana: 3,
}
```

* Если разбиваешь список аргументов, каждый аргумент должен быть на своей строчке, также как и закрывающая скобка.

```Ruby
# bad
super_method(arg0, arg1,
  arg2, arg3, arg4)

# good
super_method(
  arg0,
  arg1,
  arg2,
  arg3,
  arg4
)
  ```

* Если разбиваешь хэш, каждый элемент и закрывающая скобка должны быть на своей строчке.
* Используй 2 пробела для оступов (не используй табы).

[git eol]: https://help.github.com/articles/dealing-with-line-endings
[в словаре]: http://slovari.yandex.ru/
[google translate]: http://translate.google.com/
[dot guideline example]: https://github.com/thoughtbot/guides/blob/master/style/samples/ruby.rb#L11
[eof]: http://unix.stackexchange.com/questions/23903/should-i-end-my-text-script-files-with-a-newline

## Бэкэнд, шаблоны

### Общее

* Все URL видимые пользователем в адресной строке и индексируемые ПС должны содержать SLUG вместо ID. [в api и внутр. урлах id допустимы]

## Ruby

Для ruby-кода мы используем [styleguide гитхаба].

Код может и должен быть автоматически проверен на соответствие этому стайлгайду при помощи [rubocop] с настройками github.

[styleguide гитхаба]: https://github.com/styleguide/ruby
[rubocop]: https://github.com/github/rubocop-github

## HTML

* Используй SLIM или Pug. HAML допустим.
* Ставь пробел после =. Это значительно улучшает читаемость кода.
* Не используй input[type=image] для стилизации кнопок.

### Партиалы

При верстке, старайся выделять повторяющиеся вещи в партиалы. Хорошие кандидаты в партиал - меню, карточка товара\новости и т.д., любой элемент повторяющийся более чем в одном месте. Также можно выделять в партиалы крупные неповторяющиеся куски кода для улучшения читаемости основной вьюхи.

Элементы, повторяющиеся в трех местах и более обязательно нужно выделять в партиал.

### Организация файлов шаблонов

Пример хорошей организации:

    views
    ├── blocks # блоки, повторяющиеся в разных разделах (шапка, подвал, сайдбар)
    │   ├── news # если несколько блоков относятся к определенному контроллеру, положи их в отдельную папку
    │   │  ├── _large.slim
    │   │  └── _small.slim
    │   ├── _sidebar.slim
    │   └── _footer.slim
    ├── projects
    │   ├── _sidebar.slim
    │   ├── _project.slim # один объeкт для списка, render collection: @projects
    │   ├── show # так тоже можно, вместо того что выше и если блоков в show больше 1-2.
    │   │  └── _sidebar.slim
    │   ├── show.html.slim
    │   └── index.html.slim
    ├── news
    │   ├── _news.html.slim # блок новости, повторяющийся в наскольких шаблонах
    │   ├── index.html.slim
    │   └── show.html.slim
    ├── pages
    │   └── show.html.slim
    └── layouts
        ├── application.slim
        ├── print.slim
        └── email.slim

**Обрати внимание, что пути к блокам во views повторяют пути к блокам в webpack**

**Отдельного контроллера для папки во /views может и не быть. Важно логическое соответствие названия папки и содержимого партиала. Например, карточка товара _card.html.slim помещается в папку /items или /products.**


## Внесение изменений в styleguide

Создай pull request со своим предложением.

## Источники

[thoughtbot styleguide](https://github.com/thoughtbot/guides)

[github styleguide](https://github.com/styleguide)

[ruby style guide](https://github.com/bbatsov/ruby-style-guide)

[rails style guide](https://github.com/bbatsov/rails-style-guide)
