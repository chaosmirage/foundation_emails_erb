## Installation

Node.js 0.12 or greater

1.  npm install
2.  Изменить  example.config.json
3.  Переименовать example.config.json в config.json
4.  npm start

###Resourses
* [Github](https://github.com/zurb/foundation-emails-template)
* [Docs](http://foundation.zurb.com/emails/docs/)

## Build Commands

Run `npm start` to kick off the build process. A new browser tab will open with a server pointing to your project files.

Run `npm run build` to inline your CSS into your HTML along with the rest of the build process.

Run `npm run litmus` to build as above, then submit to litmus for testing. *AWS S3 Account details required (config.json)*

Run `npm run mail` to build as above, then send to specified email address for testing. *SMTP server details required (config.json)*

Run `npm run zip` to build as above, then zip HTML and images for easy deployment to email marketing services.

##Description

Вёрстка писем основана на фрэймворке Foundation for Emails.

Каждый шаблон письма находится в /src/pages.

Т.к. бэк использует шаблонизатор erb, пришлось сделать костыль. Причины:

  1.  При использовании <%= params[:price] %> в шаблоне письма, после билдинга это превратится в &lt;&#37; = params[:price] &#37;&gt;

  2.  Можно было бы работать с сбилденным шаблоном письма и вставлять вручную <%= params[:price] %>, но это неудобно т.к. файл с инлайн-стилями + минифицированный.

  3.  {{{ <%= params[:price] %> }}} не решает проблему
      [http://handlebarsjs.com](http://handlebarsjs.com/#html-escaping)

## Пример: выводим данные по цене в шаблон письма

Допустим нужно вывести цену в шаблоне /src/pages/client_offer_updated.html

  1.  Нужно создать хэлпер в /src/helpers под названием price.js

      В price.js:

          module.exports = function(options) {
            var price = "&lt;&#37; = @params[:price] &#37;&gt;";
            return price;
          }

  2.  При билдинге мнемоники &lt, &gt, &#37, конвертятся в символы. По другому вывести <%= params[:client_name] %>  в сбилденную версию не получилось.

      [Основание](http://foundation.zurb.com/forum/posts/39407-how-to-use-inky-with-other-template-languages)

  3.  В шаблоне там где цена, нужно вывести хэлпер price.js

      Например в теге:

          <p>{{price}}</p>

  4.  Перезапустить сборку

  5.  В сбилденном файле /dist/client_offer_updated.html это превратится в:

          <%= @params[:price] %>

  6. Готово! Можно использовать /dist/client_offer_updated.html


## Markup workflow
  1.  Создать шаблон в /src/pages

  2.  Сверстать

  3.  Протестировать шаблон

          npm run mail

### Image src helper

    <img src="{{#image_path}}file_name.png{{/image_path}}">
