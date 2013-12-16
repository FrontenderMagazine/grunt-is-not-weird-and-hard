# Grunt для тех, кто считает штуки вроде Grunt странными и сложными

Разработчикам-фронтендерам часто говорят делать определённые вещи:

*   **Работать с настолько маленькими кусками CSS и JavaScript**, насколько это
    имеет смысл, а затем объединять их для продакшна.
   
*   **Сжимать CSS и минифицировать JavaScript** с целью сделать файлы на
    продашене как можно меньше по размеру.
   
*   **Оптимизировать изображения**, чтобы уменьшить размеры файлов без потерь в
    качестве.
   
*   **Использовать Sass** для написания CSS из-за всех полезных абстракций,
    которые он позволяет использовать.
   

Конечно, этот список не полон, но это такие вещи, которые делать необходимо.
Вы можете называть их *задачами*.

Бьюсь об заклад, Вы слышали про [Grunt][1]. Что ж, Grunt — это утилита для
*выполнения задач*. Grunt может делать все эти вещи для Вас. Стоит лишь
установить его, что кстати не так уж и сложно, и эти вещи будут происходить
автоматически, так что про них даже не придётся вспоминать.

Но посмотрим правде в глаза: Grunt — одна из тех новомодных штучек, которой
пользуются все эти крутые парни, но которая на первый взгляд выглядит странно
и пугающе. Я Вас понимаю. И эта статья для Вас.

## Давайте пресечём некоторые заблуждения в зародыше

Возможно, Вы *слышали* про Grunt, но никогда с ним не работали. Я уверен, это
относится ко многим из вас. Вероятно, какие-то из этих загвоздок относятся к
Вам.

### Мне не нужно то, что делает Grunt

Вообще-то, возможно, что оно Вам и нужно. Посмотрите на тот список сверху. Эти
вещи не просто неплохо было бы иметь. Они — жизненно важная часть разработки
сайтов в наши дни. Если Вы их уже делаете, это замечательно. Вероятно, Вы
используете для этого разнообразные утилиты. Grunt помогает собрать их все,
так сказать, под одной крышей. А если Вы их ещё не используете, то Вам,
возможно, стоило бы, и Grunt может с этим помочь. А потом, когда Вы будете их
использовать, Grunt может делать для Вас ещё большее, что, в общем-то, будет
означать, что Вы лучше делаете свою работу.

### Grunt работает на Node.js, я не знаю Ноду

Вам и не надо знать Ноду. Точно так же, как Вам не нужно знать Ruby, чтобы
пользоваться Sass. Или PHP, чтобы пользоваться WordPress. Или C++, чтобы
пользоваться Microsoft Word.

### Вещи, которые делает Grunt, я и так могу сделать, по-другому

Они все собраны в одном месте, настроены так, что запускаются автоматически
когда понадобятся, и есть у каждого, кто работает над проектом? Вряд ли, рискну
предположить.

### Grunt — консольная программа, а я всего лишь дизайнер

А я тоже дизайнер. Я предпочитаю пользоваться приложениями с графическими
интерфейсами, если могу их достать. Но я не думаю, что это произойдет с Grunt.

Степень, в которой придется работать с командной строкой, такая:

1.  Перейти в папку с проектом.
2.  Напечатать `grunt` и нажать **Return**.

После установки, которая, повторюсь, не такая уж сложная.

## Хорошо. Давайте установим Grunt

Node.js действительно является неоходимым условием для Grunt. Если Нода у Вас
не установлена, не беспокойтесь, это очень легко. Вы просто скачиваете
установочник и запускаете его. Щёлкните на большой кнопке **Install**
[на сайте Node.js][2].

Grunt устанавливается на каждый проект отдельно. Перейдите в папку проекта.
Требуется небольшой файл под названием *package.json* в корне. Вы можете его
просто создать и положить туда.

![package.json в корне проекта][3]

Содержимое этого файла должно быть таким:

    {
      "name": "example-project",
      "version": "0.1.0",
      "devDependencies": {
        "grunt": "~0.4.1"
      }
    }
    

Вы вполне можете поменять название проекта и версию, главное, `devDependencies`
должно остаться таким же, как есть.

Это то, как Node разруливает зависимости. В Node есть пакетный менеджер под
разванием [NPM][4].

Как только *package.json* будет на месте, откройте терминал, и перейдите в Вашу
папку. Те, кто мало смыслят в терминале, вроде меня, делают это примерно так:

![Колхозный способ смены папки в терминале][5]

Затем запустите команду:

    npm install
    

После того, как эта команда отработает, в проекте появится новая папка с именем
*node_modules*.

![Пример папки node_modules][6]

Другие файлы, что Вы там видите, *README.md* и *LICENSE*, находятся там потому
что я собираюсь выложить этот проект [на Github][7], и там это просто так
принято.

Последний шаг установки — установить Grunt CLI (command line interface,
«интерфейс командной строки»). Это то, что заставляет в терминале работать
команду `grunt`. Без неё запуск `grunt` вернёт ошибку вроде «Команда или файл не
найдена». Она устанавливается одтельно по причинам эффективности. Иначе, если бы
у Вас был десяток проектов, пришлось бы устанавливать десять копий GruntCLI.

Это опять одна строчка. Просто запустите эту команду в терминале:

    npm install -g grunt-cli
    

После этого следует закрыть и открыть заново окно терминала. Это вообще хороший
подход для того, чтобы убедиться, что всё работает. Это вроде того, как раньше
надо было перезагрузить компьютер после установки новой программы.

## Сделаем так, чтобы Grunt объединял файлы

Представим, что в нашем проекте есть три отдельных файла JavaScript:

1.  ***jquery.js*** – Библиотека, которую мы используем.
2.  ***carousel.js*** – Плагин для jQuery, который мы используем.
3.  ***global.js*** – Написанный нами файл JavaScript, где мы настраиваем и
    вызываем плагин.
   

На продакшн-сервере нам надо объединить все эти файлы в один для улучшения
производительности (один запрос лучше, чем три). Нам нужно сказать Grunt, чтобы
он это для нас сделал.

Но постойте. Grunt на самом деле ничего сам по себе не делает. Помните же,
Grunt — это утилита для *выполнения* задач. А сами задачи нам придется добавить.
Пока что мы ещё не настроили Grunt, чтобы он что-либо делал, поэтому давайте
этим займёмся.

Официальный плагин Grunt для объединения файлов — [grunt-contrib-concat][8]

    npm install grunt-contrib-concat --save-dev
    

Приятная особенность такого способа установки в том, что Ваш файл *package.json*
будет автоматически обновлён, и в него пропишется новая зависимость. Откройте
его и убедитесь. Там появится новая строка:

    "grunt-contrib-uglify": "~0.2.2"
    

Вот теперь мы готовы использовать этот плагин. А чтобы его использовать, нам
нужно начать настраивать Grunt и объяснять ему, что делать.

Указать Grunt, что ему делать, можно при комощи конфигурационного файла с
названием *Gruntfile.js*.

Точно так же, как и файл *package.json*, наш *Gruntfile.js* находится в особом
формате, который должен быть в точности правильным. Я бы не стал заморачиваться
по поводу того, что значит в нём каждое слово. Просто посмотрите на формат:

    module.exports = function(grunt) {
    
        // 1. Вся настройка находится здесь
        grunt.initConfig({
            pkg: grunt.file.readJSON('package.json'),
    
            concat: {
                // 2. Настройка для объединения файлов находится тут
            }
    
        });
    
        // 3. Тут мы указываем Grunt, что хотим использовать этот плагин
        grunt.loadNpmTasks('grunt-contrib-concat');
    
        // 4. Тут указываем, что делать, когда мы вводим «grunt» в терминале
        grunt.registerTask('default', ['concat']);
    
    };
    

Теперь нам нужно создать этот конфигурационный файл. Документация может быть
ошеломляющей. Давайте сосредоточимся на простеньком [примере][9].

Помните, у нас есть три файла JavaScript, которые мы пытаемся объединить. Мы
перечислим пути к ним в `src` в качестве массива с путями к файлам (как строки
в кавычках), и затем мы укажем файл назначения как `dest`. Файл назначения не
обязательно должен уже существовать. Он будет создан, когда эта задача будет
запущена и склеит все файлы в один.

И *jquery.js*, и *carousel.js* являются библиотеками. Мы, скорее всего, не
станем их трогатье. Так что, для порядка в проекте, мы будем держать их в папке
*/js/libs/*. Файл *global.js* — то, где мы пишем наш собственный код, поэтому он
будет находиться прямо в папке */js/*. Теперь скажем Grunt, как находить все эти
файлы и склеивать их в один файл *production.js*, названный так, чтобы показать,
что он будет использоваться на настоящем, живом сайте.

    concat: {   
        dist: {
            src: [
                'js/libs/*.js', // Все JS в папке libs
                'js/global.js'  // Конкретный файл
            ],
            dest: 'js/build/production.js',
        }
    }
    

**Примечание:** далее в этой статье будут приводиться такие же небольшие кусочки
конфигурационного кода, как этот. Их цель — обратить внимание на важные места,
но поначалу Вы можете не понять, как каждый кусочек вставлен в большой файл.
Если Вы запутаетесь, и Вам нужно будет посмотреть на них в контексте,
просмотрите [полный файл конфигурации][10].

Когда конфигурация `concat` окажется на своем месте, откройте терминал и
запустите команду:

    grunt
    
и смотрите, что произойдёт! *production.js* будет создан, и он будет
превосходным объединением трёх файлов. Для меня это был такой момент «ага!».
Почувствуйте силу, наполняющую Ваши вены. Давайте сделаем побольше таких штук!

## Let’s make Grunt minify that JavaScript

We have so much prep work done now, adding new tasks for Grunt to run is
relatively easy. We just need to:

1.  Find a Grunt plug-in to do what we want
2.  Learn the configuration style of that plug-in
3.  Write that configuration to work with our project

The official plug-in for minifying code is [grunt-contrib-uglify][11]. Just
like we did last time, we just run anNPM command to install it:

    npm install grunt-contrib-uglify --save-dev
    

Then we alter our *Gruntfile.js* to load the plug-in:

    grunt.loadNpmTasks('grunt-contrib-uglify');
    

Then we configure it:

    uglify: {
        build: {
            src: 'js/build/production.js',
            dest: 'js/build/production.min.js'
        }
    }
    

Let’s update that `default` task to also run minification:

    grunt.registerTask('default', ['concat', 'uglify']);
    

Super-similar to the concatenation set-up, right?

Run `grunt` at the terminal and you’ll get some deliciously minified
JavaScript:

![Minified JavaScript][12]

That *production.min.js* file is what we would load up for use in our *index.
html* file.

## Let’s make Grunt optimize our images

We’ve got this down pat now. Let’s just go through the motions. The
official image minification plug-in for Grunt is[grunt-contrib-imagemin][13].
Install it:

    npm install grunt-contrib-imagemin --save-dev
    

Register it in the *Gruntfile.js*:

    grunt.loadNpmTasks('grunt-contrib-imagemin');
    

Configure it:

    imagemin: {
        dynamic: {
            files: [{
                expand: true,
                cwd: 'images/',
                src: ['**/*.{png,jpg,gif}'],
                dest: 'images/build/'
            }]
        }
    }
    

Make sure it runs:

    grunt.registerTask('default', ['concat', 'uglify', 'imagemin']);
    

Run `grunt` and watch that gorgeous squishification happen:

![Squished images][14]

Gotta love performance increases for nearly zero effort.

## Let’s get a little bit smarter and automate

What we’ve done so far is awesome and incredibly useful. But there are a
couple of things we can get smarter on and make things easier on ourselves, as 
well as Grunt:

1.  Run these tasks automatically when they should
2.  Run only the tasks needed at the time

For instance:

1.  Concatenate and minify JavaScript when JavaScript changes
2.  Optimize images when a new image is added or an existing one changes

We can do this by watching files. We can tell Grunt to keep an eye out for
changes to specific places and, when changes happen in those places, run 
specific tasks. Watching happens through the official[grunt-contrib-watch][15]
plugin.

I’ll let you install it. It is exactly the same process as the last few plug-
ins we installed. We configure it by giving`watch` specific files (or folders,
or both) to watch. By watch, I mean monitor for file changes, file deletions or 
file additions. Then we tell it what tasks we want to run when it detects a 
change.

We want to run our concatenation and minification when anything in the */js/*
folder changes. When it does, we should run the JavaScript-related tasks. And 
when things happen elsewhere, we should*not* run the JavaScript-related tasks,
because that would be irrelevant. So:

    watch: {
        scripts: {
            files: ['js/*.js'],
            tasks: ['concat', 'uglify'],
            options: {
                spawn: false,
            },
        } 
    }
    

Feels pretty comfortable at this point, hey? The only weird bit there is the 
`spawn` thing. And you know what? I don’t even really know what that does.
From what I understand from the documentation it is the smart default. That’s 
real-world development. Just leave it alone if it’s working and if it’s not, 
learn more.

**Note:** Isn’t it frustrating when something that looks so easy in a
tutorial doesn’t seem to work for you? If you can’t get Grunt to run after 
making a change, it’s very likely to be a syntax error in your*Gruntfile.js*.
That might look like this in the terminal:

![Errors running Grunt][16]

Usually Grunt is pretty good about letting you know what happened, so be sure
to read the error message. In this case, a syntax error in the form of a missing
comma foiled me. Adding the comma allowed it to run.

## Let’s make Grunt do our preprocessing

The last thing on our list from the top of the article is using Sass — yet
another task Grunt is well-suited to run for us. But wait? Isn’t Sass 
technically in Ruby? Indeed it is. There is
[a version of Sass that will run in Node][17] and thus not add an additional
dependency to our project, but it’s not quite up-to-snuff with the main Ruby 
project. So, we’ll use the official[grunt-contrib-sass][18] plug-in which just
assumes you have Sass installed on your machine. If you don’t, follow the
[command line instructions][19].

What’s neat about Sass is that it can do concatenation and minification all
by itself. So for our little project we can just have it compile our main*
global.scss* file:

    sass: {
        dist: {
            options: {
                style: 'compressed'
            },
            files: {
                'css/build/global.css': 'css/global.scss'
            }
        } 
    }
    

We wouldn’t want to manually run this task. We already have the watch plug-in
installed, so let’s use it! Within the`watch` configuration, we’ll add
another subtask:

    css: {
        files: ['css/*.scss'],
        tasks: ['sass'],
        options: {
            spawn: false,
        }
    }
    

That’ll do it. Now, every time we change any of our Sass files, the CSS will
automaticaly be updated.

Let’s take this one step further (it’s absolutely worth it) and add
LiveReload. With LiveReload, you won’t have to go back to your browser and 
refresh the page. Page refreshes happen automatically and in the case ofCSS,
new styles are injected without a page refresh (handy for heavily state-based 
websites
).

It’s very easy to set up, since the LiveReload ability is built into the
watch plug-in. We just need to:

1.  Install the [browser plug-in][20] 
2.  Add to the top of the `watch` configuration: 
        . watch: {
            options: {
                livereload: true,
            },
            scripts: {   
            /* etc */

3.  Restart the browser and click the LiveReload icon to activate it.
4.  Update some Sass and watch it change the page automatically.

![Live reloading browser][21]

Yum.

## Leveling up

As you might imagine, there is a *lot* of leveling up you can do with your
build process. It surely could be a[full time job][22] in some organizations.

Some hardcore devops nerds might scoff at the simplistic setup we have going
here. But I’d advise them to slow their roll. Even what we have done so far is 
tremendously valuable. And don’t forget this is all free and open source, which 
is amazing.

You might level up by adding more useful tasks:

*   Running your CSS through [Autoprefixer][23] (A+ Would recommend) instead of
    a preprocessor add-ons.
   
*   Writing and running JavaScript unit tests (example: [Jasmine][24]).
*   Build your image sprites and SVG icons automatically (example: 
    [Grunticon][25]).
*   Start a server, so you can link to assets with proper file paths and use
    services that require a real
   URL like TypeKit and such, as well as remove the need for other tools that
    do this, like
   MAMP.
*   Check for code problems with [HTML-Inspector][26], [CSS Lint][27], or 
    [JS Hint][28].
*   Help you commit or push to a version control repository like GitHub.
*   Add version numbers to your assets (cache busting).
*   Help you deploy to a staging or production environment (example: 
    [DPLOY][29]).

You might level up by simply understanding more about Grunt itself:

## Let’s share

I think some group sharing would be a nice way to wrap this up. If you are
installing Grunt for the first time (or remember doing that), be especially 
mindful of little frustrating things you experience(d) but work(ed) through. 
Those are the things we should share in the comments here. That way we have this
safe place and useful resource for working through those confusing moments 
without the embarrassment. We’re all in this thing together!

Perhaps a decent middleground is this [Grunt DevTools][30] Chrome add-on.

 [1]: http://gruntjs.com/
 [2]: http://nodejs.org/
 [3]: img/package-json-file.gif
 [4]: https://npmjs.org/
 [5]: img/drag-folder.gif
 [6]: img/node_modules.gif
 [7]: https://github.com/chriscoyier/My-Grunt-Boilerplate
 [8]: https://github.com/gruntjs/grunt-contrib-concat
 [9]: https://github.com/gruntjs/grunt-contrib-concat#usage-examples

 [10]: https://github.com/chriscoyier/My-Grunt-Boilerplate/blob/master/Gruntfile.js
 [11]: https://github.com/gruntjs/grunt-contrib-uglify
 [12]: img/uglify-code.gif
 [13]: https://github.com/gruntjs/grunt-contrib-imagemin
 [14]: img/squished-images.gif
 [15]: https://github.com/gruntjs/grunt-contrib-watch
 [16]: img/error-running-grunt.gif
 [17]: https://github.com/sindresorhus/grunt-sass
 [18]: https://github.com/gruntjs/grunt-contrib-sass
 [19]: http://sass-lang.com/install

 [20]: http://feedback.livereload.com/knowledgebase/articles/86242-how-do-i-install-and-use-the-browser-extensions-
 [21]: img/style-injection.gif
 [22]: http://www.smashingmagazine.com/2013/06/11/front-end-ops/
 [23]: http://css-tricks.com/autoprefixer/
 [24]: https://github.com/pivotal/jasmine
 [25]: https://github.com/filamentgroup/grunticon
 [26]: http://philipwalton.com/articles/introducing-html-inspector/
 [27]: http://csslint.net/
 [28]: http://www.jshint.com/
 [29]: http://leanmeanfightingmachine.github.io/dploy/
 [30]: https://github.com/vladikoff/grunt-devtools