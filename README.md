# Рецепты по работе с webpack

### Что это?

Набор советов и решений проблем, с которыми я столкнулся при работе с webpack'ом. А, учитывая, что многие рецепты будут совсем базового уровня и других материалов на русском языке нет, то можно считать это «мини-учебником».

### Формат?

Пpока все в одном файле и вперемешку. Если разрастется и найдутся читатели, то категоризирую.

### Нужна помощь?

Конечно. И нужен именно чужой практический опыт: боролись с проблемой несколько часов/дней/недель? Создайте пулл с парой строк и общим описанием, помогу оформить и довести до ума.

## Рецепты

### Нужен ли мне webpack? [http://webpack.github.io](http://webpack.github.io)

Нужен, если:

* вы не хотите волноваться в каком формате ваши js-зависимости:  AMD, CJS, Plain, etc 
* вы не хотите волноваться о пре- и постпроцессорах и любой обработке, а хотите просто вызвать `require` и получит необходимый модуль
* вы хотите объединять не только js-код, но и стили, картинки и любые другие ресурсы, создавая независимые модули
* вам нужна асихронная подгрузка и/или объединение модулей в пакеты
* вы хотите моментальной сборки в девелопменте и горячей замены модулей без перезагрузки страницы
* вы не боитесь экспериментировать и пробовать что-то новое
* куча других приятных мелочей ...

### Что нужно знать?

* что такое системы сборки и зачем они нужны
* что такое nodejs & npm
* уметь установить webpack (*hint:* `npm install webpack`)

### Начальные

#### Как собрать coffescript-файлы в один bundle.js?

**Дано:**

`main.js`:

```js
var myApp = require("myapp.coffee");
myApp.init();
```

`myapp.coffee:`   

```coffee
module.exports = {...}
```

**Решение:**   

* Создать `webpack.config.js`:

```js
module.exports = {
  entry: './main.js',
  output: {
    filename: 'bundle.js'       
  },
  module: {
    loaders: [
      { test: /\.coffee$/, loader: 'coffee-loader' },
    ]
  }
};
```

* Добавить `coffee-loader` в `package.json`

**Объяснение:**   
Первое, что нужно понять, что webpack оперирует не папками и файлами, а модулями, т.е. ему именно важно, что вы написали внутри ваших файлов для построения дерева зависимостей.

Отсюда и первое отличие от других систем сборки: на вход подается не регулярное выражение а-ля `src/**/*.coffee`, а `entry point` вашего приложения. Любой же код, который вы подключите через `require` пройдет обработку через загрузчики.

Автор Webpack сторается держать ядро очень легковесным, «сваливая» всю работу на плагины и загрузчики.

Настройки того, на какие файлы какие загрузчики должны срабатывать описывается в разделе `loaders` файла конфигурации: сначала идет регулярное выражение, которое применяется к названию файла, а затем список загрузчиков, которые сработают на удовлетворившем условию файле.

#### TODO Как избежать сложных относительных путей, например: "./../../../../ui.js"
#### TODO Как подключать файлы глобально?
#### TODO Как подгружать файлы из bower?
#### TODO Как бить модули на пакеты
#### TODO Как выделять общие зависимости?
#### TODO Как асинхронно подключать пакеты и модули
#### TODO Как хранить настройки отдельно для production & development
#### TODO Как настроить hot module replacement?
#### TODO Как чинить «неправильный» AMD?