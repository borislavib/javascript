Original Repository: [airbnb/javascript](https://github.com/airbnb/javascript)

# Airbnb JavaScript Style Guide() {

*Разумен подход за писане на JavaScript*


## <a name='TOC'>Съдържанине</a>

  1. [Типове](#types)
  1. [Обекти](#objects)
  1. [Масиви](#arrays)
  1. [Низове](#strings)
  1. [Функции](#functions)
  1. [Полета](#properties)
  1. [Променливи](#variables)
  1. [Деклариране и използване на променливи](#hoisting)
  1. [Условни изрази и равенства](#conditionals)
  1. [Блокове](#blocks)
  1. [Коментари](#comments)
  1. [Празни пространства](#whitespace)
  1. [Запетаи](#commas)
  1. [Точка и запетая](#semicolons)
  1. [Преобразуване на типове](#type-coercion)
  1. [Наименуване практики](#naming-conventions)
  1. [Достъпване](#accessors)
  1. [Конструктори](#constructors)
  1. [Събития](#events)
  1. [Модули](#modules)
  1. [jQuery](#jquery)
  1. [ES5 Съвместимост](#es5)
  1. [Тестване](#testing)
  1. [Изпъление](#performance)
  1. [Ресурси](#resources)
  1. [Къде се използват тези практики](#in-the-wild)
  1. [Превод](#translation)
  1. [JavaScript стилов пътеводител](#guide-guide)
  1. [Сътрудници](#contributors)
  1. [Лиценз](#license)

## <a name='types'>Типове</a>

  - **Примитивни**: Когато достъпвате примитивен тип се работи директно със неговата стойност. 

    + `string`
    + `number`
    + `boolean`
    + `null`
    + `undefined`

    ```javascript
    var foo = 1,
        bar = foo;

    bar = 9;

    console.log(foo, bar); // => 1, 9
    ```
  - **Съставни**: Когато достъпвате комплексен тип данни се работи с референция на стойността и. 

    + `object`
    + `array`
    + `function`

    ```javascript
    var foo = [1, 2],
        bar = foo;

    bar[0] = 9;

    console.log(foo[0], bar[0]); // => 9, 9
    ```

    **[[⬆]](#TOC)**

## <a name='objects'>Обекти</a> 
  - Използвайте втория вариант за създаване на обект.

    ```javascript
    // лоша практика
    var item = new Object();

    // добра практика
    var item = {};
    ```

  - Не използвайте [запазени думи](http://es5.github.io/#x7.6.1) за деклариране на променливи. Тези променливи няма да работят на IE8. [Повече](https://github.com/airbnb/javascript/issues/61)

    ```javascript
    // лоша практика
    var superman = {
      default: { clark: 'kent' },
      private: true
    };

    // добра практика
    var superman = {
      defaults: { clark: 'kent' },
      hidden: true
    };
    ```

  - Използвайте смислени синоними на мястото на запазени думи.

    ```javascript
    // лоша практика
    var superman = {
      class: 'alien'
    };

    // лоша практика
    var superman = {
      klass: 'alien'
    };

    // добра практика
    var superman = {
      type: 'alien'
    };
    ```
    **[[⬆]](#TOC)**

## <a name='arrays'>Масиви</a>

  - Използвайте варианта със скобите за създаване на масив

    ```javascript
    // лоша практика
    var items = new Array();

    // добра практика
    var items = [];
    ```

  - Ако не знаете дължината на масив, използвайте Array#push.

    ```javascript
    var someStack = [];


    // лоша практика
    someStack[someStack.length] = 'abracadabra';

    // добра практика
    someStack.push('abracadabra');
    ```

  - Ако имате нужда да копирате масив използвайте Array#slice. [jsPerf](http://jsperf.com/converting-arguments-to-an-array/7)

    ```javascript
    var len = items.length,
        itemsCopy = [],
        i;

    // лоша практика
    for (i = 0; i < len; i++) {
      itemsCopy[i] = items[i];
    }

    // добра практика
    itemsCopy = items.slice();
    ```

  - Да конвертирате подобен на масив обект към масив, използвайте Array#slice.

    ```javascript
    function trigger() {
      var args = Array.prototype.slice.call(arguments);
      ...
    }
    ```

    **[[⬆]](#TOC)**


## <a name='strings'>Низове</a>

  - Използвайте единични кавички `''` за Низове.

    ```javascript
    // лоша практика
    var name = "Bob Parr";

    // добра практика
    var name = 'Bob Parr';

    // лоша практика
    var fullName = "Bob " + this.lastName;

    // добра практика
    var fullName = 'Bob ' + this.lastName;
    ```

  - Низове, по-дълги от 80 символа трябва да се напишат на няколко реда, като се използва конкатенация.
  - Забележка: Ако се използват неправилно, . [jsPerf](http://jsperf.com/ya-string-concat) & [s](https://github.com/airbnb/javascript/issues/40)

    ```javascript
    // лоша практика
    var errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';

    // лоша практика
    var errorMessage = 'This is a super long error that \
    was thrown because of Batman. \
    When you stop to think about \
    how Batman had anything to do \
    with this, you would get nowhere \
    fast.';


    // добра практика
    var errorMessage = 'This is a super long error that ' +
      'was thrown because of Batman.' +
      'When you stop to think about ' +
      'how Batman had anything to do ' +
      'with this, you would get nowhere ' +
      'fast.';
    ```

  - Когато искате да направите низ, използвайте Array#join вместо конкатенация. [jsPerf](http://jsperf.com/string-vs-array-concat/2).

    ```javascript
    var items,
        messages,
        length,
        i;

    messages = [{
        state: 'success',
        message: 'This one worked.'
    },{
        state: 'success',
        message: 'This one worked as well.'
    },{
        state: 'error',
        message: 'This one did not work.'
    }];

    length = messages.length;

    // лоша практика
    function inbox(messages) {
      items = '<ul>';

      for (i = 0; i < length; i++) {
        items += '<li>' + messages[i].message + '</li>';
      }

      return items + '</ul>';
    }

    // добра практика
    function inbox(messages) {
      items = [];

      for (i = 0; i < length; i++) {
        items[i] = messages[i].message;
      }

      return '<ul><li>' + items.join('</li><li>') + '</li></ul>';
    }
    ```

    **[[⬆]](#TOC)**


## <a name='functions'>Функции</a>

  - Функции-изрази:

    ```javascript
    // Анонимни функции изрази
    var anonymous = function() {
      return true;
    };

    // Именувание функции изрази
    var named = function named() {
      return true;
    };

    // Моментално изпълнени функции-изрази (IIFE)
    (function() {
      console.log('Welcome to the Internet. Please follow me.');
    })();
    ```

  - Никога не декларирайте функция в нефункционелен блок (if, while, etc). Дайте и стойност и на променлива вместо това. Браузърите ще дадат това, но ще го интерпретират различно, което не е никак добре.
  - **Забележка:** ECMA-262 дефинира `block` като лист с декларации/твърдения. Декларирането на фунция не е твърдение. [Read ECMA-262's note on this issue](http://www.ecma-international.org/publications/files/ECMA-ST/Ecma-262.pdf#page=97).

    ```javascript
    // лоша практика
    if (currentUser) {
      function test() {
        console.log('Nope.');
      }
    }

    // добра практика
    var test;
    if (currentUser) {
      test = function test() {
        console.log('Yup.');
      };
    }
    ```

  - Никога не кръщавайте параметър `arguments`, това ще вземе значението на `arguments`-обекта, който е деклариран по подразбиране във всеки скоуп.

    ```javascript
    // лоша практика
    function nope(name, options, arguments) {
      // ...stuff...
    }

    // добра практика
    function yup(name, options, args) {
      // ...stuff...
    }
    ```

    **[[⬆]](#TOC)**



## <a name='properties'>полета</a>

  - Използвайте '.' за достъпване на полета.

    ```javascript
    var luke = {
      jedi: true,
      age: 28
    };

    // лоша практика
    var isJedi = luke['jedi'];

    // добра практика
    var isJedi = luke.jedi;
    ```

  - Използвайте `[]` когато достъпвате полета със променлива.

    ```javascript
    var luke = {
      jedi: true,
      age: 28
    };

    function getProp(prop) {
      return luke[prop];
    }

    var isJedi = getProp('jedi');
    ```

    **[[⬆]](#TOC)**


## <a name='variables'>Променливи</a>

  - Винаги използвайте `var` за деклариране на променливи. В противен случай се декларират глобални променливи. Ние искаме да не 'замърсяване' глобалното пространство с тях. Капитан планета ни предупреди за тях.

    ```javascript
    // лоша практика
    superPower = new SuperPower();

    // добра практика
    var superPower = new SuperPower();
    ```

  - Използвайте една `var` декларация за много променливи и декларирайте всяка променлива на нов ред.

    ```javascript
    // лоша практика
    var items = getItems();
    var goSportsTeam = true;
    var dragonball = 'z';

    // добра практика
    var items = getItems(),
        goSportsTeam = true,
        dragonball = 'z';
    ```

  - Декларирайте променливи, непродобили стойност последни. Това е полезно, когато по-късно се нуждаете от променлива зависеща от някоя от предходно дефинираните променливи.

    ```javascript
    // лоша практика
    var i, len, dragonball,
        items = getItems(),
        goSportsTeam = true;

    // лоша практика
    var i, items = getItems(),
        dragonball,
        goSportsTeam = true,
        len;

    // добра практика
    var items = getItems(),
        goSportsTeam = true,
        dragonball,
        length,
        i;
    ```

  - Слагайте променливите винаги в началото на скоупа им. Това помага да се избегнат проблеми с декларации и неправомерно извикване на недефинирани променливи.

    ```javascript
    // лоша практика
    function() {
      test();
      console.log('doing stuff..');

      //..други..

      var name = getName();

      if (name === 'test') {
        return false;
      }

      return name;
    }

    // добра практика
    function() {
      var name = getName();

      test();
      console.log('doing stuff..');

      //..other stuff..

      if (name === 'test') {
        return false;
      }

      return name;
    }

    // лоша практика
    function() {
      var name = getName();

      if (!arguments.length) {
        return false;
      }

      return true;
    }

    // добра практика
    function() {
      if (!arguments.length) {
        return false;
      }

      var name = getName();

      return true;
    }
    ```

    **[[⬆]](#TOC)**


## <a name='hoisting'>Деклариране и използване на променливи</a>

  - Декларацията на променливи се поставя в началото на функция или обект, тяхното изпозлване става по-надолу.

    ```javascript
    // Това няма да работи (приемаме, че няма 
	// дефинирана notDefined в глобалния скоуп)
    function example() {
      console.log(notDefined); // => throws a ReferenceError
    }

    // Извикване на променлива, преди декларацията и ще работи,
	// но стойносттта и няма да се взима
	 
    function example() {
      console.log(declaredButNotAssigned); // => undefined
      var declaredButNotAssigned = true;
    }

	// Интерпретаторът поставя дефинирането в началото
    // на скоупа. Примерът може да се пренапише така :
	
    function example() {
      var declaredButNotAssigned;
      console.log(declaredButNotAssigned); // => undefined
      declaredButNotAssigned = true;
    }
    ```

  - Анонимните функции-изрази поставят най-горе дефиницията на променливи, но не и стойността/фунцкията,
    присвоена на променливата.

    ```javascript
    function example() {
      console.log(anonymous); // => undefined

      anonymous(); // => TypeError anonymous is not a function

      var anonymous = function() {
        console.log('anonymous function expression');
      };
    }
    ```

  - Именуваните функции-изрази прехвърлят името на променливата, а не името на функцията или тялото и.

    ```javascript
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      superPower(); // => ReferenceError superPower is not defined

      var named = function superPower() {
        console.log('Flying');
      };
    }

	
	// същото е вярно, когато името на функцията
    // е същото като името на променливата.
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      var named = function named() {
        console.log('named');
      }
    }
    ```

  - Декларацията на функцията прехвърля името и тялото на функция.

    ```javascript
    function example() {
      superPower(); // => Flying

      function superPower() {
        console.log('Flying');
      }
    }
    ```

  - За повече информация [JavaScript Scoping & Hoisting](http://www.adequatelygood.com/2010/2/JavaScript-Scoping-and-Hoisting) от [Ben Cherry](http://www.adequatelygood.com/)

    **[[⬆]](#TOC)**



## <a name='conditionals'>Условни изрази и равенства</a>

  - Използвайте `===` и `!==` вместо `==` и `!=`.
  - Условните изрази се проверяват чрез прехвърляне към `ToBoolean` метод и винаги следват тези прости правила:

    + **Objects** става **true**
    + **Undefined** става **false**
    + **Null** става **false**
    + **Booleans** става **the value of the boolean**
    + **Numbers** става **false** Ако **+0, -0, or NaN**, иначе **true**
    + **Strings** става **false** ако е празен низ`''`, иначе **true**

    ```javascript
    if ([0]) {
      // вярно 
      // Ако масивът е обект, обектите връщат true
    }
    ```
 
  - Използвайте кратки варианти.

    ```javascript
    // лоша практика
    if (name !== '') {
      // ...stuff...
    }

    // добра практика
    if (name) {
      // ...stuff...
    }

    // лоша практика
    if (collection.length > 0) {
      // ...stuff...
    }

    // добра практика
    if (collection.length) {
      // ...stuff...
    }
    ```

  - За повече информация [Truth Equality and JavaScript](http://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108) by Angus Croll

    **[[⬆]](#TOC)**


## <a name='blocks'>Блокове</a>

  - Използвайте скоби с всички многоредови блокове.

    ```javascript
    // лоша практика
    if (test)
      return false;

    // добра практика
    if (test) return false;

    // добра практика
    if (test) {
      return false;
    }

    // лоша практика
    function() { return false; }

    // добра практика
    function() {
      return false;
    }
    ```

    **[[⬆]](#TOC)**


## <a name='comments'>Коментари</a>

  - Използвайте `/** ... */` за многоредови коментари. Включете обяснение, изяснете типове и стойности за всички параметри и стойности, които се връщат от функцията.

    ```javascript
    // лоша практика
    // make() returns a new element
    // based on the passed in tag name
    //
    // @param <String> tag
    // @return <Element> element
    function make(tag) {

      // ...stuff...

      return element;
    }

    // добра практика
    /**
     * make() returns a new element
     * based on the passed in tag name
     *
     * @param <String> tag
     * @return <Element> element
     */
    function make(tag) {

      // ...stuff...

      return element;
    }
    ```

  - Използвайте `//` за едноредови коментари. Поставятйте едноредовите коментари на нов ред над предмета на обяснение. Поставете и празен ред преди коментара.


    ```javascript
    // лоша практика
    var active = true;  // is current tab

    // добра практика
    // is current tab
    var active = true;

    // лоша практика
    function getType() {
      console.log('fetching type...'); 
      var type = this._type || 'no type';

      return type;
    }

    // добра практика
    function getType() {
      console.log('fetching type...');
 
      var type = this._type || 'no type';

      return type;
    }
    ```

  - Поставяйте в коментарите `FIXME` или `TODO`, помагайки на други разработчици да разберат ако има проблем, който трябва да бъде прегледан или да предлагате решение на проблем.  Това са различни от обикновените коментари, защото 
те предлагат действие. 
`FIXME -- need to figure this out` or `TODO -- need to implement`.

  - Използвайте `// FIXME:` да анонсирате проблем

    ```javascript
    function Calculator() {

      // FIXME: shouldn't use a global here
      total = 0;

      return this;
    }
    ```

  - Използвайте `// TODO:` да анонсирате решение на проблема

    ```javascript
    function Calculator() {

      // TODO: total should be configurable by an options param
      this.total = 0;

      return this;
    }
  ```

    **[[⬆]](#TOC)**


## <a name='whitespace'>Празни пространства</a>

  - Използвайте табулации с 2 празни пространства

    ```javascript
    // лоша практика
    function() {
    ∙∙∙∙var name;
    }

    // лоша практика
    function() {
    ∙var name;
    }

    // добра практика
    function() {
    ∙∙var name;
    }
    ```
  - Поставяйте 1 празно пространство преди начална скоба.

    ```javascript
    // лоша практика
    function test(){
      console.log('test');
    }

    // добра практика
    function test() {
      console.log('test');
    }

    // лоша практика
    dog.set('attr',{
      age: '1 year',
      breed: 'Bernese Mountain Dog'
    });

    // добра практика
    dog.set('attr', {
      age: '1 year',
      breed: 'Bernese Mountain Dog'
    });
    ```
  - Поставяйте празен ред в края на файл.

    ```javascript
    // лоша практика
    (function(global) {
      // ...stuff...
    })(this);
    ```

    ```javascript
    // добра практика
    (function(global) {
      // ...stuff...
    })(this);

    ```

  - Използвайте йерархично подравняване при дълги вериги от методи.

    ```javascript
    // лоша практика
    $('#items').find('.selected').highlight().end().find('.open').updateCount();

    // добра практика
    $('#items')
      .find('.selected')
        .highlight()
        .end()
      .find('.open')
        .updateCount();

    // лоша практика
    var leds = stage.selectAll('.led').data(data).enter().append('svg:svg').class('led', true)
        .attr('width',  (radius + margin) * 2).append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);

    // добра практика
    var leds = stage.selectAll('.led')
        .data(data)
      .enter().append('svg:svg')
        .class('led', true)
        .attr('width',  (radius + margin) * 2)
      .append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);
    ```

    **[[⬆]](#TOC)**

## <a name='commas'>Запетаи</a>

  - Запетая на нов ред: **НЕ.**

    ```javascript
    // лоша практика
    var once
      , upon
      , aTime;

    // добра практика
    var once,
        upon,
        aTime;

    // лоша практика
    var hero = {
        firstName: 'Bob'
      , lastName: 'Parr'
      , heroName: 'Mr. Incredible'
      , superPower: 'strength'
    };

    // добра практика
    var hero = {
      firstName: 'Bob',
      lastName: 'Parr',
      heroName: 'Mr. Incredible',
      superPower: 'strength'
    };
    ```

  - Допълнителна запетая в края: **НЕ.** Може да създаде проблем с IE6/7 и IE9. Също, в някои реализации на ES3 може да добави дължина към масив ако има такава запетая. Това е изяснено в ES5 ([source](http://es5.github.io/#D)):

  > Издание 5 изяснява факта, че запетая в края на ArrayInitialiser не добавя дължина към масива. Това не е семантина промяна от издание 3, но някои реализации може да бъркат това.

    ```javascript
    // лоша практика
    var hero = {
      firstName: 'Kevin',
      lastName: 'Flynn',
    };

    var heroes = [
      'Batman',
      'Superman',
    ];

    // добра практика
    var hero = {
      firstName: 'Kevin',
      lastName: 'Flynn'
    };

    var heroes = [
      'Batman',
      'Superman'
    ];
    ```

    **[[⬆]](#TOC)**


## <a name='semicolons'>Точка и запетая</a>

  - **ДА.**

    ```javascript
    // лоша практика
    (function() {
      var name = 'Skywalker'
      return name
    })()

    // добра практика
    (function() {
      var name = 'Skywalker';
      return name;
    })();

    // добра практика
    ;(function() {
      var name = 'Skywalker';
      return name;
    })();
    ```

    **[[⬆]](#TOC)**


## <a name='type-coercion'>Преобразуване на типове</a>

  - Изпълнявайте преобразуването на типове в началото.
  - Низ:

    ```javascript
    //  => this.reviewScore = 9;

    // лоша практика
    var totalScore = this.reviewScore + '';

    // добра практика
    var totalScore = '' + this.reviewScore;

    // лоша практика
    var totalScore = '' + this.reviewScore + ' total score';

    // добра практика
    var totalScore = this.reviewScore + ' total score';
    ```

  - Use `parseInt` for Numbers and always with a radix for type casting.

    ```javascript
    var inputValue = '4';

    // лоша практика
    var val = new Number(inputValue);

    // лоша практика
    var val = +inputValue;

    // лоша практика
    var val = inputValue >> 0;

    // лоша практика
    var val = parseInt(inputValue);

    // добра практика
    var val = Number(inputValue);

    // добра практика
    var val = parseInt(inputValue, 10);
    ```

  
  - В случай, че решите да сте палави с  `parseInt` и това ви е ахилесовата пета и е нужно да използате смяна на битове [performance reasons](http://jsperf.com/coercion-vs-casting/3), оставете коментар какво и защо правите.
  
  - **Забележка:** Бъдете предпазливи като използвате промяна на битове. Числата са представени чрез [64-bit values](http://es5.github.io/#x4.3.19), но смяната на битове винаги връща 32-битов интиджер ([source](http://es5.github.io/#x11.7)). Смяната на битове може да доведе до неочаквано поведени при числови стойности на 32 бита. [Discussion](https://github.com/airbnb/javascript/issues/109)

    ```javascript
    // добра практика
    /**
     * parseInt беше причина кода ми да е бавен.
     * Промяната на битове в стринга
     * към числа го направи по-бърз.
     */
    var val = inputValue >> 0;
    ```

  - Булеви стойности:

    ```javascript
    var age = 0;

    // лоша практика
    var hasAge = new Boolean(age);

    // добра практика
    var hasAge = Boolean(age);

    // добра практика
    var hasAge = !!age;
    ```

    **[[⬆]](#TOC)**


## <a name='naming-conventions'>Наименуване практики</a>

  - Избягвайте имена от 1 буква. Обяснявайте какво наименувате.

    ```javascript
    // лоша практика
    function q() {
      // ...stuff...
    }

    // добра практика
    function query() {
      // ..stuff..
    }
    ```

  - Използвайте camelCase, когато именовате обекти, функции, и инстанции

    ```javascript
    // лоша практика
    var OBJEcttsssss = {};
    var this_is_my_object = {};
    function c() {};
    var u = new user({
      name: 'Bob Parr'
    });

    // добра практика
    var thisIsMyObject = {};
    function thisIsMyFunction() {};
    var user = new User({
      name: 'Bob Parr'
    });
    ```

  - Използвайте PascalCase когато именувате конструктори или класове

    ```javascript
    // лоша практика
    function user(options) {
      this.name = options.name;
    }

    var лоша практика = new user({
      name: 'nope'
    });

    // добра практика
    function User(options) {
      this.name = options.name;
    }

    var добра практика = new User({
      name: 'yup'
    });
    ```

  - Използвайте долно тире в началото `_`, когато именувате частни променливи

    ```javascript
    // лоша практика
    this.__firstName__ = 'Panda';
    this.firstName_ = 'Panda';

    // добра практика
    this._firstName = 'Panda';
    ```

  - Когато използвате референции към `this` използвайте `_this`.

    ```javascript
    // лоша практика
    function() {
      var self = this;
      return function() {
        console.log(self);
      };
    }

    // лоша практика
    function() {
      var that = this;
      return function() {
        console.log(that);
      };
    }

    // добра практика
    function() {
      var _this = this;
      return function() {
        console.log(_this);
      };
    }
    ```

  - Именувайте си функцийте. Това е добра практика за проследяване на стака.

    ```javascript
    // лоша практика
    var log = function(msg) {
      console.log(msg);
    };

    // добра практика
    var log = function log(msg) {
      console.log(msg);
    };
    ```

    **[[⬆]](#TOC)**


## <a name='accessors'>Достъпване</a>

  - Не са необходими функции за променливи
  - Ако има - ползвайте подобни на  getVal() и setVal('hello')

    ```javascript
    // лоша практика
    dragon.age();

    // добра практика
    dragon.getAge();

    // лоша практика
    dragon.age(25);

    // добра практика
    dragon.setAge(25);
    ```

  - Ако променлива е булева стойност, използвайте isVal() или hasVal()

    ```javascript
    // лоша практика
    if (!dragon.age()) {
      return false;
    }

    // добра практика
    if (!dragon.hasAge()) {
      return false;
    }
    ```

  - Може да се създадат get() и set() функции, но бъдете постоянни с тях.

    ```javascript
    function Jedi(options) {
      options || (options = {});
      var lightsaber = options.lightsaber || 'blue';
      this.set('lightsaber', lightsaber);
    }

    Jedi.prototype.set = function(key, val) {
      this[key] = val;
    };

    Jedi.prototype.get = function(key) {
      return this[key];
    };
    ```

    **[[⬆]](#TOC)**


## <a name='constructors'>Конструктор</a>

  - Присвоявайте методи на прототипа обект, вместо да пренаписвате прототипа с нов обект. Пренаписването на прототипа прави наследяването невъзможно: от задаване на нова стойност на прототипа, пренаписване базата!

    ```javascript
    function Jedi() {
      console.log('new jedi');
    }

    // лоша практика
    Jedi.prototype = {
      fight: function fight() {
        console.log('fighting');
      },

      block: function block() {
        console.log('blocking');
      }
    };

    // добра практика
    Jedi.prototype.fight = function fight() {
      console.log('fighting');
    };

    Jedi.prototype.block = function block() {
      console.log('blocking');
    };
    ```

  - Методите могат да връщат `this` помагайки с навързването на методи.

    ```javascript
    // лоша практика
    Jedi.prototype.jump = function() {
      this.jumping = true;
      return true;
    };

    Jedi.prototype.setHeight = function(height) {
      this.height = height;
    };

    var luke = new Jedi();
    luke.jump(); // => true
    luke.setHeight(20) // => undefined

    // добра практика
    Jedi.prototype.jump = function() {
      this.jumping = true;
      return this;
    };

    Jedi.prototype.setHeight = function(height) {
      this.height = height;
      return this;
    };

    var luke = new Jedi();

    luke.jump()
      .setHeight(20);
    ```


	- Може да дефинира и toString() метод, просто бъдете сигурни, че няма странични ефекти.

    ```javascript
    function Jedi(options) {
      options || (options = {});
      this.name = options.name || 'no name';
    }

    Jedi.prototype.getName = function getName() {
      return this.name;
    };

    Jedi.prototype.toString = function toString() {
      return 'Jedi - ' + this.getName();
    };
    ```

    **[[⬆]](#TOC)**


## <a name='events'>Събития</a>
  
    ```javascript 
    // лоша практика
    $(this).trigger('listingUpdated', listing.id);

    ...

    $(this).on('listingUpdated', function(e, listingId) {
      // do something with listingId
    });
    ```

    предпочитайте:

    ```javascript
    // добра практика
    $(this).trigger('listingUpdated', { listingId : listing.id });

    ...

    $(this).on('listingUpdated', function(e, data) {
      // do something with data.listingId
    });
    ```

  **[[⬆]](#TOC)**


## <a name='modules'>Модули</a>

  - Модул трябва да започва с `!`. Това осигурява, че дори друг модул да е забравил да постави на края точка и запетая, няма да има грешки в продукция, когато скриптовете се конкатенират. [Explanation](https://github.com/airbnb/javascript/issues/44#issuecomment-13063933)
  
  - Файлът се наименува с camelCase, в папка със същото име.
  
  
  - Добавете метод, казващ се noConflict(), поставящ експортираните модули към предишна версия и връщащ този.
  
  - Декларирайте `'use strict';` в началото на модул.

    ```javascript
    // fancyInput/fancyInput.js

    !function(global) {
      'use strict';

      var previousFancyInput = global.FancyInput;

      function FancyInput(options) {
        this.options = options || {};
      }

      FancyInput.noConflict = function noConflict() {
        global.FancyInput = previousFancyInput;
        return FancyInput;
      };

      global.FancyInput = FancyInput;
    }(this);
    ```

    **[[⬆]](#TOC)**


## <a name='jquery'>jQuery</a>

  - Поставете jQuery обекти променливи с `$`.

    ```javascript
    // лоша практика
    var sidebar = $('.sidebar');

    // добра практика
    var $sidebar = $('.sidebar');
    ``` 

    ```javascript
    // лоша практика
    function setSidebar() {
      $('.sidebar').hide();

      // ...stuff...

      $('.sidebar').css({
        'background-color': 'pink'
      });
    }

    // добра практика
    function setSidebar() {
      var $sidebar = $('.sidebar');
      $sidebar.hide();

      // ...stuff...

      $sidebar.css({
        'background-color': 'pink'
      });
    }
    ```

  //- For DOM queries use Cascading `$('.sidebar ul')` or parent > child `$('.sidebar > ul')`. [jsPerf](http://jsperf.com/jquery-find-vs-context-sel/16)
  - За DOM търсене използвайте каскадно `$('.sidebar ul')` or parent > child `$('.sidebar > ul')`. [jsPerf](http://jsperf.com/jquery-find-vs-context-sel/16)
  - Use `find` with scoped jQuery object queries.
  - Използвайте `find` в jQuery за по-лесно намиране в скоуп.

    ```javascript
    // лоша практика
    $('ul', '.sidebar').hide();

    // лоша практика
    $('.sidebar').find('ul').hide();

    // добра практика
    $('.sidebar ul').hide();

    // добра практика
    $('.sidebar > ul').hide();

    // добра практика
    $sidebar.find('ul');
    ```

    **[[⬆]](#TOC)**


## <a name='es5'>ECMAScript 5 Съвместимост</a>

  - Отнася се [Kangax](https://twitter.com/kangax/)'s ES5 [compatibility table](http://kangax.github.com/es5-compat-table/)

  **[[⬆]](#TOC)**


## <a name='testing'>Тестване</a>

  - **ДА.**

    ```javascript
    function() {
      return true;
    }
    ```

    **[[⬆]](#TOC)**


## <a name='performance'>Изпълнение</a>

  - [On Layout & Web Performance](http://kellegous.com/j/2013/01/26/layout-performance/)
  - [String vs Array Concat](http://jsperf.com/string-vs-array-concat/2)
  - [Try/Catch Cost In a Loop](http://jsperf.com/try-catch-in-loop-cost)
  - [Bang Function](http://jsperf.com/bang-function)
  - [jQuery Find vs Context, Selector](http://jsperf.com/jquery-find-vs-context-sel/13)
  - [innerHTML vs textContent for script text](http://jsperf.com/innerhtml-vs-textcontent-for-script-text)
  - [Long String Concatenation](http://jsperf.com/ya-string-concat)
  - Loading...

  **[[⬆]](#TOC)**


## <a name='resources'>Ресурси</a>


**Прочетете това**

  - [Annotated ECMAScript 5.1](http://es5.github.com/)

**Други стилови ръководства**

  - [Google JavaScript Style Guide](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml)
  - [jQuery Core Style Guidelines](http://docs.jquery.com/JQuery_Core_Style_Guidelines)
  - [Principles of Writing Consistent, Idiomatic JavaScript](https://github.com/rwldrn/idiomatic.js/)

**Други стилове**

  - [Naming this in nested functions](https://gist.github.com/4135065) - Christian Johansen
  - [Conditional Callbacks](https://github.com/airbnb/javascript/issues/52)
  - [Popular JavaScript Coding Conventions on Github](http://sideeffect.kr/popularconvention/#javascript)

**Допълнителни**

  - [Understanding JavaScript Closures](http://javascriptweblog.wordpress.com/2010/10/25/understanding-javascript-closures/) - Angus Croll
  - [Basic JavaScript for the impatient programmer](http://www.2ality.com/2013/06/basic-javascript.html) - Dr. Axel Rauschmayer

**Книги**

  - [JavaScript: The good Parts](http://www.amazon.com/JavaScript-good-Parts-Douglas-Crockford/dp/0596517742) - Douglas Crockford
  - [JavaScript Patterns](http://www.amazon.com/JavaScript-Patterns-Stoyan-Stefanov/dp/0596806752) - Stoyan Stefanov
  - [Pro JavaScript Design Patterns](http://www.amazon.com/JavaScript-Design-Patterns-Recipes-Problem-Solution/dp/159059908X)  - Ross Harmes and Dustin Diaz
  - [High Performance Web Sites: Essential Knowledge for Front-End Engineers](http://www.amazon.com/High-Performance-Web-Sites-Essential/dp/0596529309) - Steve Souders
  - [Maintainable JavaScript](http://www.amazon.com/Maintainable-JavaScript-Nicholas-C-Zakas/dp/1449327680) - Nicholas C. Zakas
  - [JavaScript Web Applications](http://www.amazon.com/JavaScript-Web-Applications-Alex-MacCaw/dp/144930351X) - Alex MacCaw
  - [Pro JavaScript Techniques](http://www.amazon.com/Pro-JavaScript-Techniques-John-Resig/dp/1590597273) - John Resig
  - [Smashing Node.js: JavaScript Everywhere](http://www.amazon.com/Smashing-Node-js-JavaScript-Everywhere-Magazine/dp/1119962595) - Guillermo Rauch
  - [Secrets of the JavaScript Ninja](http://www.amazon.com/Secrets-JavaScript-Ninja-John-Resig/dp/193398869X) - John Resig and Bear Bibeault
  - [Human JavaScript](http://humanjavascript.com/) - Henrik Joreteg
  - [Superhero.js](http://superherojs.com/) - Kim Joar Bekkelund, Mads Mobæk, & Olav Bjorkoy
  - [JSBooks](http://jsbooks.revolunet.com/)

**Блогове**

  - [DailyJS](http://dailyjs.com/)
  - [JavaScript Weekly](http://javascriptweekly.com/)
  - [JavaScript, JavaScript...](http://javascriptweblog.wordpress.com/)
  - [Bocoup Weblog](http://weblog.bocoup.com/)
  - [Adequately good](http://www.adequatelygood.com/)
  - [NCZOnline](http://www.nczonline.net/)
  - [Perfection Kills](http://perfectionkills.com/)
  - [Ben Alman](http://benalman.com/)
  - [Dmitry Baranovskiy](http://dmitry.baranovskiy.com/)
  - [Dustin Diaz](http://dustindiaz.com/)
  - [nettuts](http://net.tutsplus.com/?s=javascript)

  **[[⬆]](#TOC)**

## <a name='in-the-wild'>Къде се използват тези практики</a>
 

  - **Aan Zee**: [AanZee/javascript](https://github.com/AanZee/javascript)
  - **Airbnb**: [airbnb/javascript](https://github.com/airbnb/javascript)
  - **American Insitutes for Research**: [AIRAST/javascript](https://github.com/AIRAST/javascript)
  - **Compass Learning**: [compasslearning/javascript-style-guide](https://github.com/compasslearning/javascript-style-guide)
  - **ExactTarget**: [ExactTarget/javascript](https://github.com/ExactTarget/javascript)
  - **Gawker Media**: [gawkermedia/javascript](https://github.com/gawkermedia/javascript)
  - **GeneralElectric**: [GeneralElectric/javascript](https://github.com/GeneralElectric/javascript)
  - **GoodData**: [gooddata/gdc-js-style](https://github.com/gooddata/gdc-js-style)
  - **Grooveshark**: [grooveshark/javascript](https://github.com/grooveshark/javascript)
  - **How About We**: [howaboutwe/javascript](https://github.com/howaboutwe/javascript)
  - **Mighty Spring**: [mightyspring/javascript](https://github.com/mightyspring/javascript)
  - **MinnPost**: [MinnPost/javascript](https://github.com/MinnPost/javascript)
  - **ModCloth**: [modcloth/javascript](https://github.com/modcloth/javascript)
  - **National Geographic**: [natgeo/javascript](https://github.com/natgeo/javascript)
  - **National Park Service**: [nationalparkservice/javascript](https://github.com/nationalparkservice/javascript)
  - **Razorfish**: [razorfish/javascript-style-guide](https://github.com/razorfish/javascript-style-guide)
  - **Shutterfly**: [shutterfly/javascript](https://github.com/shutterfly/javascript)
  - **Userify**: [userify/javascript](https://github.com/userify/javascript)
  - **Zillow**: [zillow/javascript](https://github.com/zillow/javascript)
  - **ZocDoc**: [ZocDoc/javascript](https://github.com/ZocDoc/javascript)

## <a name='translation'>Преводи</a>

  Други преводи

  - :de: **Немски**: [timofurrer/javascript-style-guide](https://github.com/timofurrer/javascript-style-guide)
  - :jp: **Японски**: [mitsuruog/javacript-style-guide](https://github.com/mitsuruog/javacript-style-guide)
  - :br: **Портогалски**: [armoucar/javascript-style-guide](https://github.com/armoucar/javascript-style-guide)
  - :cn: **Китайски**: [adamlu/javascript-style-guide](https://github.com/adamlu/javascript-style-guide)
  - :es: **Испански**: [paolocarrasco/javascript-style-guide](https://github.com/paolocarrasco/javascript-style-guide)
  - :kr: **Корейски**: [tipjs/javascript-style-guide](https://github.com/tipjs/javascript-style-guide)
  - :fr: **Френски**: [nmussy/javascript-style-guide](https://github.com/nmussy/javascript-style-guide)
  - :ru: **Руски**: [sbezludny/javascript-style-guide](https://github.com/sbezludny/javascript-style-guide)
  - :bg: **Български**: [borislavvv/javascript-style-guide](https://github.com/sbezludny/javascript-style-guide)

## <a name='guide-guide'>JavaScript Стилово ръководство</a>

  - [Reference](https://github.com/airbnb/javascript/wiki/The-JavaScript-Style-Guide-Guide)

## <a name='authors'>Сътрудници</a>

  - [View Contributors](https://github.com/airbnb/javascript/graphs/contributors)


## <a name='license'>Лиценз</a>

(The MIT License)

Copyright (c) 2012 Airbnb

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

**[[⬆]](#TOC)**

# };

