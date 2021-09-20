# PostgreSQL

## I. About PostgreSQL

PostgreSQL is an ACID-compliant Object Relational Database Management System, or ORDBMS. Put simply, it is a database that allows you to relate one piece of data to another. It runs on nearly any operating system including Linux, Unix, and Windows. It is high performance and highly scalable, capable of handling huge amounts of data and high-load internet applications with thousands of concurrent users. Its unique combination of simplicity and power makes it a popular choice for individual users and small businesses, but enterprise businesses like Yahoo, Uber, Disqus, and TripAdvisor as well.

Postgres supports a long list of database features, including several enterprise features. Aside from standard relational database features, some of the most notable features in Postgres are:

- Streaming replication
- Schemas
- User-defined objects like operators, data types, and functions
- Nested transactions
- Table inheritance
- Partitioning
- Several unusual data types, like Money, Geometry, IP addresses, JSON, and data ranges.
<br />
<hr>

## II. Getting started

The first thing we’re going to do is install Postgres. There are two main ways to get Postgres onto your machine:
- Using a graphical installer like [Postgres.app](https://postgresapp.com/)
- Using a package manager to install via the command line.

We are going to use our terminal of course.

### 1. Getting [homebrew](https://brew.sh/)

You probably have this installed already, but in case if you dont - copy this command into your terminal:
```sh
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

Now when we have homebrew, we can proceed to install PostgreSQL.

### 2. Installing PostgreSQL

Installing PostgreSQL is very easy, just run this command in your terminal to install PostgreSQL using homebrew:

```sh
brew install postgresql
```

Let’s go ahead and start Postgres, and to make sure Postgres starts every time your computer starts up. Execute the following command:
```sh
pg_ctl -D /usr/local/var/postgres start && brew services start postgresql
```

To check if you did everything correctly, you can run the following command:
```sh
postgres -V
```
This should tell you what version of PostgreSQL you are running.
That's it. Now you have PostgreSQL installed.
<br />
<br />
<br />
<hr>

## III. Configuring Postgres

Postgres works pretty hard to make itself usable right out of the box without you having to do anything. By default, it automatically creates the user postgres. Let’s see what other users it has created. Let’s start by using the psql utility, which is a utility installed with Postgres that lets you carry out administrative functions without needing to know their actual SQL commands.

Start by entering the following on the command line:
```sh
psql
```
And you should see something like this: <br /> <br />
<img width="416" alt="Screenshot at Sep 20 15-27-00" src="https://user-images.githubusercontent.com/50056973/134022392-ecbe6bc8-8d77-4633-8a8b-b9c2d35de89f.png">

That’s the psql command line. We can now enter a command to see what users are installed:
```sh
brunocale=# \du
```
This command executes an SQL query that gets all the users in the database. On my machine, it returns the following: <br /> <br />
<img width="784" alt="Screenshot at Sep 20 15-32-09" src="https://user-images.githubusercontent.com/50056973/134023010-a7c98bcc-2ebf-4cb1-8030-a13f1aea9d23.png">
<br />
So when Postgres is installed, it automatically creates a database user that matches your username, so that you can get started right away.

### 1. Creating a role

A role is an entity that can own database objects and have database privileges; a role can be considered a "user", a "group", or both depending on how it is used.

Let's create a new role. The basic syntax for creating a role looks like this:
```sh
CREATE ROLE username WITH LOGIN PASSWORD 'quoted password' [OPTIONS]
```
Where instead of `username` we put the name of the role we want to create and the password goes in the quotes. Like this:
<br />
<br />
<img width="789" alt="Screenshot at Sep 20 16-14-20" src="https://user-images.githubusercontent.com/50056973/134023705-f6b28f82-99d8-4c8a-98ce-5745be917a17.png">
<br />

But as you can see the list of attributes is empty, why is that? <br /> <br />
This is how Postgres securely manages defaults. This user can read any database, table, or row it has permissions for, but nothing else—it cannot create or manage databases and has no admin powers. This is a good thing! It helps keep your database secure.
<br /> <br /> 
Ok, so let's give that role a permission to create a databse. We will do that by running the following code:
```sh
brunocale=# ALTER ROLE demorole CREATEDB; 
```
And we get this: <br /> <br />
<img width="797" alt="Screenshot at Sep 20 16-20-56" src="https://user-images.githubusercontent.com/50056973/134026772-ca0fdfdb-ca60-4f35-80e4-60824498dc6b.png">

<br />
If you want to remove/delete that role, just run:

```sh
brunocale=# DROP ROLE demorole; 
```
But we won't do that now, let's first create a database.

### 2. Creating a database

Syntax for creating a databse is:
```sh
CREATE DATABASE databasename;
```
Let's now use our new role that we made (demorole), run this command (not while you are in psql terminal, first exit that, you can do that with `\q`):
```sh
psql postgres -U demorole
```
You will get something like this: <br /> <br />
<img width="658" alt="Screenshot at Sep 20 16-35-31" src="https://user-images.githubusercontent.com/50056973/134026945-a4aa3d52-a5c2-44cb-a331-b0b5ac2c1ea9.png">

<br />
You’ll notice the prompt is slightly different – the `#` has changed to a `>`. This indicates you’re no longer using a Super User account.

Now let's just run the above command for creating a database. <br /> <br />
<img width="820" alt="Screenshot at Sep 20 16-39-22" src="https://user-images.githubusercontent.com/50056973/134027088-30609601-477c-4994-b638-c93dac176734.png">

<br />
You can see that third database is the one we created just now and that it's owner is `demorole`.
<br /> <br />

Here are some commands that you can be used in psql:

* `\list` -> lists all databases in psql
* `\connect` -> connect to a specific database
* `dt` -> list all the tables in the currently connected database
