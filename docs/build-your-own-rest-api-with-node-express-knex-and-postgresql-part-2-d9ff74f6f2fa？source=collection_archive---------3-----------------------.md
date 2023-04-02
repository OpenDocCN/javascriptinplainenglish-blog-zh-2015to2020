# 用 Node、Express、Knex 和 PostgreSQL 构建您自己的 REST API 第 2 部分

> 原文：<https://javascript.plainenglish.io/build-your-own-rest-api-with-node-express-knex-and-postgresql-part-2-d9ff74f6f2fa?source=collection_archive---------3----------------------->

![](img/342f695501cece11b4a0dc82ddc37ff7.png)

在[的最后一篇文章](https://medium.com/@fajardocj/build-your-own-rest-api-with-node-express-knex-and-postgresql-aec98fe75e5)中，我们建立了我们的用户模型。让我们再添加两个模型，并探索它与用户模型的关系。

# 创建帖子模型

启动一个终端，再次输入`knex migrate:make create-posts`，这一次是针对评论栏:`knex migrate:make create-comments`。按照相同的顺序创建这些文件是很重要的，稍后您会明白为什么。这将创建两个名为 timestamp_create-posts.js 和 timestamp_create-comments.js 的文件。

太好了，我们现在有两个新的迁移，我们可以开始调整。让我们从 create-posts.js 迁移开始，添加一个 id、一个标题和一个主体:

```
exports.up = function (knex) {
    return knex.schema.createTable('posts', (table) => {
        table.increments(); // This is our id field
        table.string('title');
        table.text('body', 'longtext');
    });
};exports.down = function (knex) {
    return knex.schema.dropTable('posts')
};
```

# 添加关系

我们现在已经建立了 posts 迁移，但是我们还缺少一些东西。你想想，我们还是没有办法说出每个帖子**属于**的谁。我对我的 Rails 朋友使用粗体和大写。当您建立一个属于关系时，您所建立的是一个引用所述模型的“所有者”的列。让我们来看看:

我们将在帖子迁移中添加一个新行，并添加:

```
table.integer('user_id').notNullable().references('id').inTable('users').onDelete('cascade');
```

现在我们正在谈话！我们现在有一个名为`user_id`的专栏，可以用来跟踪我们的作者。我们在这一栏中输入的任何数字都将被视为帖子的所有者。

另外，请注意，当我们设置关系时，rails 提供了更多可用的方法，我们只是设置了一个简单的方法，但目前来说，这已经足够了。

因此，我们完成的帖子模型现在应该是这样的:

```
exports.up = function (knex) {
    return knex.schema.createTable('posts', (table) => {
        table.increments(); // This is our id field
        table.integer('user_id').unsigned().references('users.id');
        table.string('title');
        table.text('body', 'longtext');
    });
};exports.down = function (knex) {
    return knex.schema.dropTable('posts')
};
```

我选择在我的 id 之后添加我的关系，但是您可以选择任何您想要的地方，只要您仍然在表定义内。

# 在注释表中

没什么新的，除了这次我们将引用我们的另外两个表，所以一个评论同时属于一个帖子和一个用户:

```
exports.up = function (knex) {
    return knex.schema.createTable('comments', (table) => {
        table.increments(); // Again, this will be our id
        table.integer('user_id').notNullable().references('id').inTable('users').onDelete('cascade');
        table.integer('post_id').notNullable().references('id').inTable('posts').onDelete('cascade');
        table.text('body', 'longtext');
    })
};exports.down = function (knex) {
    return knex.schema.dropTable('comments');
};
```

这样我们的数据库就完全建立起来了。我们现在要做的就是逃跑

```
knex migrate:latest
```

把我们的新表格输入数据库。让我们通过运行`psql tutorial`来检查一下:

```
~/: psql tutorial
psql (12.2)
Type "help" for help.tutorial=# \d
                     List of relations
 Schema |              Name              |   Type   | Owner 
--------+--------------------------------+----------+-------
 public | comments                       | table    | john
 public | comments_id_seq                | sequence | john
 public | knex_migrations                | table    | john
 public | knex_migrations_id_seq         | sequence | john
 public | knex_migrations_lock           | table    | john
 public | knex_migrations_lock_index_seq | sequence | john
 public | posts                          | table    | john
 public | posts_id_seq                   | sequence | john
 public | users                          | table    | john
 public | users_id_seq                   | sequence | john
(10 rows)
```

太棒了。所有的东西都在那里，再加上 knex 创建的几个表来跟踪我们的 id。

# 添加更多种子数据

让我们添加一些数据，以确保我们有一些工作。运行`knex seed:make create_dummy_posts`并转到你新创建的`/seeds/create_dummy_posts.js`文件。编辑它，使它看起来像这样:

```
exports.seed = function (knex) {
  return knex('table_name').del()
    .then(function () {
      return knex('table_name').insert([
        {
          user_id: 1,
          title: 'JavaScript',
          body: 'JavaScript, often abbreviated as JS, is a programming language that conforms to the ECMAScript specification. JavaScript is high-level, often just-in-time compiled, and multi-paradigm. It has curly-bracket syntax, dynamic typing, prototype-based object-orientation, and first-class functions.'
        },
        {
          user_id: 1,
          title: 'Node.js',
          body: 'Node.js is an open-source, cross-platform, JavaScript runtime environment that executes JavaScript code outside of a web browser. Node.js lets developers use JavaScript to write command line tools and for server-side scripting—running scripts server-side to produce dynamic web page content before the page is sent to the user\'s web browser. Consequently, Node.js represents a \"JavaScript everywhere\" paradigm, unifying web-application development around a single programming language, rather than different languages for server- and client-side scripts.'
        },
        {
          user_id: 1,
          title: 'Express.js',
          body: 'Express.js, or simply Express, is a web application framework for Node.js, released as free and open-source software under the MIT License. It is designed for building web applications and APIs. It has been called the de facto standard server framework for Node.js.'
        }
      ]);
    });
};
```

我们有三篇文章映射到我们的第一个用户，有标题和正文。现在让我们对我们的评论做同样的事情。运行`knex seed:make create_dummy_comments`并编辑生成的`/seeds/create_dummy_comments.js`文件:

```
exports.seed = function (knex) {
  return knex('table_name').del()
    .then(function () {
      // Inserts seed entries
      return knex('table_name').insert([
        { user_id: 1, post_id: 1, body: 'Far far away, behind the word mountains, far from the countries Vokalia and Consonantia, there live the blind texts.' },
        { user_id: 1, post_id: 1, body: 'The European languages are members of the same family. Their separate existence is a myth. For science, music, sport, etc.' },
        { user_id: 1, post_id: 1, body: 'But I must explain to you how all this mistaken idea of denouncing pleasure and praising pain was born and I will give you a complete account of the system.' },
        { user_id: 1, post_id: 2, body: 'Lights creepeth may. Fowl itself you\'re. Dry given moved man gathered moved replenish living. Likeness you\'ll to his can\'t every air fruit for, morning under they\'re.' },
        { user_id: 1, post_id: 2, body: 'Perhaps a re-engineering of your current world view will re-energize your online nomenclature to enable a new holistic interactive enterprise internet communication solution.' },
        { user_id: 1, post_id: 2, body: 'Fundamentally transforming well designed actionable information whose semantic content is virtually null.' },
        { user_id: 1, post_id: 3, body: 'Empowerment in information design literacy demands the immediate and complete disregard of the entire contents of this cyberspace communication.' },
        { user_id: 1, post_id: 3, body: 'Doing business like this takes much more effort than doing your own business at home' },
        { user_id: 1, post_id: 4, body: ' The Big Oxmox advised her not to do so, because there were thousands of bad Commas' },
      ]);
    });
};
```

最后，我们有 9 条评论，每条评论对应一篇文章，都映射到同一个用户。如果您愿意，您可以修改您的用户种子文件，添加更多的用户，并在种子中使用他们的 id，这样就不会出现所有的评论都是由帖子作者本人在明显的精神病发作时写的。

最后，让我们检查数据库，看看我们的数据是否被正确导入:

```
tutorial=# SELECT * FROM users;
 id | username |        email        
----+----------+---------------------
  1 | John Doe | johndoe@example.com
  2 | Jane Doe | janedoe@example.com
(2 rows)tutorial=# SELECT * FROM posts;
 id | user_id |   title    |                                                                                                                                                                                       
                                                                                             body                                                                                                                  

----+---------+------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
------------------------------------------------------------------------------------------------------------------------------------------------------------------
  1 |       1 | JavaScript | JavaScript, often abbreviated as JS, is a programming language that conforms to the ECMAScript specification. JavaScript is high-level, often just-in-time compiled, and multi-paradig
m. It has curly-bracket syntax, dynamic typing, prototype-based object-orientation, and first-class functions.
  2 |       1 | Node.js    | Node.js is an open-source, cross-platform, JavaScript runtime environment that executes JavaScript code outside of a web browser. Node.js lets developers use JavaScript to write comm
and line tools and for server-side scripting—running scripts server-side to produce dynamic web page content before the page is sent to the user's web browser. Consequently, Node.js represents a "JavaScript ever
ywhere" paradigm, unifying web-application development around a single programming language, rather than different languages for server- and client-side scripts.
  3 |       1 | Express.js | Express.js, or simply Express, is a web application framework for Node.js, released as free and open-source software under the MIT License. It is designed for building web applicatio
ns and APIs. It has been called the de facto standard server framework for Node.js.
(3 rows)tutorial=# SELECT * FROM comments;
 id | user_id | post_id |                                                                                      body                                                                                      
----+---------+---------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  1 |       1 |       1 | Far far away, behind the word mountains, far from the countries Vokalia and Consonantia, there live the blind texts.
  2 |       1 |       1 | The European languages are members of the same family. Their separate existence is a myth. For science, music, sport, etc.
  3 |       1 |       1 | But I must explain to you how all this mistaken idea of denouncing pleasure and praising pain was born and I will give you a complete account of the system.
  4 |       1 |       2 | Lights creepeth may. Fowl itself you're. Dry given moved man gathered moved replenish living. Likeness you'll to his can't every air fruit for, morning under they're.
  5 |       1 |       2 | Perhaps a re-engineering of your current world view will re-energize your online nomenclature to enable a new holistic interactive enterprise internet communication solution.
  6 |       1 |       2 | Fundamentally transforming well designed actionable information whose semantic content is virtually null.
  7 |       1 |       3 | Empowerment in information design literacy demands the immediate and complete disregard of the entire contents of this cyberspace communication.
  8 |       1 |       3 | Doing business like this takes much more effort than doing your own business at home
  9 |       1 |       3 |  The Big Oxmox advised her not to do so, because there were thousands of bad Commas
(9 rows)
```

虽然这种布局很难看到，但我们可以放心，我们的数据就在那里。现在，我们已经拥有了开始添加路线所需的一切！下周回来看《T2》第三部。