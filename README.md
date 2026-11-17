# news_portal

A small news portal in plain PHP with a hand-written MVC structure: news list, article pages with comments, and forms to add and delete news.

## Context

This is a test assignment for a junior PHP developer position, written in February 2017 and touched up in early March 2017. It is kept unchanged as an early-career snapshot: it shows how the owner approached a web application without a framework at the very start of a PHP backend career. It does not reflect current coding standards (see [Status](#status)).

## Features

- Home page listing up to 10 news items (title, date, author, short text).
- Article page with the full text and its comments.
- Comment form (name, email, website, comment) with required-field checks.
- Form to create a news item with required-field checks.
- Delete page with a confirmation form.
- Clean URLs via a front controller and `.htaccess` rewrite rules:
  - `/` and `/news/index` - news list
  - `/news/view/<id>` - article with comments
  - `/news/create/new` - create a news item
  - `/news/delete/<id>` - delete a news item

## Tech stack

- PHP (no framework, no Composer)
- MySQL via PDO
- Apache with `mod_rewrite`
- HTML/CSS layout with Font Awesome 3.2.1

## Architecture

```
index.php              front controller: defines ROOT, loads components, runs the router
components/Router.php  matches the request URI against regex rules and calls Controller::actionX()
components/Db.php      creates a PDO connection from config/db_params.php
config/routes.php      URI pattern -> controller/action map
config/db_params.php   database credentials
controllers/           NewsController (index, view, create, delete)
models/                News and Comments (static methods, prepared statements)
views/                 PHP templates and shared header/footer layouts
web/                   images, scripts, styles
```

Key decisions:

- Single entry point and a regex-based router instead of one PHP file per page.
- Controller/model/view separation implemented by hand to show understanding of MVC without a framework.
- Database writes use PDO prepared statements.

The `controllers/Test.php` and `controllers/test*.php` files are empty classes from practising Git workflows in March 2017 and are not part of the application.

## Getting started

Requirements: PHP with the PDO MySQL extension, MySQL, and Apache with `mod_rewrite` (or PHP's built-in server).

1. Create the database. The default settings in `config/db_params.php` are host `localhost`, database `blog`, user `root`, empty password; adjust them if needed.

   ```bash
   mysql -u root -e "CREATE DATABASE blog"
   ```

2. Create the tables. The repository does not contain a schema dump; the columns used by the models are:
   - `news`: `id`, `title`, `date`, `author_name`, `short_content`, `content`
   - `comments`: `id`, `post_id`, `comment_text`, `author`, `author_mail`, `author_website`, `date`

3. Serve the project root, either as an Apache document root (the included `.htaccess` routes all requests to `index.php`) or with PHP's built-in server:

   ```bash
   php -S localhost:8000
   ```

## Testing

There are no automated tests.

## Status

Archived. Written in February-March 2017 and kept for history only; no further development is planned. Known limitations from that time: no output escaping in the templates, no CSRF protection, no authentication for creating or deleting news, and no database schema file.

## License

MIT, see [LICENSE](LICENSE).
