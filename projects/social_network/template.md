# {{TABLE NAME}} Model and Repository Classes Design Recipe

_Copy this recipe template to design and implement Model and Repository classes for a database table._

## 1. Design and create the Table

If the table is already created in the database, you can skip this step.

Otherwise, [follow this recipe to design and create the SQL schema for your table](./single_table_design_recipe_template.md).

*In this template, we'll use an example table `students`*

```
# EXAMPLE

Table: user_accounts

Columns:
id | name | cohort_name


```

## 2. Create Test SQL seeds

Your tests will depend on data stored in PostgreSQL to run.

If seed data is provided (or you already created it), you can skip this step.

```sql
-- EXAMPLE
-- (file: spec/seeds_{table_name}.sql)

-- Write your SQL seed here. 

-- First, you'd need to truncate the table - this is so our table is emptied between each test run,
-- so we can start with a fresh state.
-- (RESTART IDENTITY resets the primary key)

TRUNCATE TABLE user_accounts RESTART IDENTITY; -- replace with your own table name.
TRUNCATE TABLE posts RESTART IDENTITY

-- Below this line there should only be `INSERT` statements.
-- Replace these statements with your own seed data.

INSERT INTO user_accounts (username, email_address) VALUES ('James', 'James@gmail.com');
INSERT INTO user_accounts (username, email_address) VALUES ('Jack', 'Jack@gmail.com');

INSERT INTO posts (title, content, user_accounts_id) VALUES ('My name', 'Is James', '1');
INSERT INTO posts (title, content, user_accounts_id) VALUES ('My age', 'Is 23', '2');
```

Run this SQL file on the database to truncate (empty) the table, and insert the seed data. Be mindful of the fact any existing records in the table will be deleted.

```bash
psql -h 127.0.0.1 your_database_name < seeds_{table_name}.sql
```

## 3. Define the class names

Usually, the Model class name will be the capitalised table name (single instead of plural). The same name is then suffixed by `Repository` for the Repository class name.

```ruby
# EXAMPLE
# Table name: students

class UserAccounts
end

class UserAccountsRepository
end


class Posts
end


class PostsRepository
end


```

## 4. Implement the Model class

Define the attributes of your Model class. You can usually map the table columns to the attributes of the class, including primary and foreign keys.

```ruby
# EXAMPLE
# Table name: students

# Model class
# (in lib/albums.rb)

class UserAccounts

  # Replace the attributes by your own columns.
  attr_accessor :id, :username, :email_address
end

class Posts

  # Replace the attributes by your own columns.
  attr_accessor :id, :title, :content, :user_account_id
end

# The keyword attr_accessor is a special Ruby feature
# which allows us to set and get attributes on an object,
# here's an example:
#


# album = Album.new
# album.title = 'American Teen'
# album.release_year = 2017

# album = Album.new
# album.title = 'American Teen'
# album.artist = 'Khalid'



```

*You may choose to test-drive this class, but unless it contains any more logic than the example above, it is probably not needed.*

## 5. Define the Repository Class interface

Your Repository class will need to implement methods for each "read" or "write" operation you'd like to run against the database.

Using comments, define the method signatures (arguments and return value) and what they do - write up the SQL queries that will be used by each method.

```ruby
# EXAMPLE
# Table name: albums

# Repository class
# (in lib/albums_repository.rb)

class User_accountsRepository

  # Selecting all records
  # No arguments
  def all
    # Executes the SQL query:
    SELECT id, username, email_address FROM user_accounts;

    # Returns an array of Album objects.
  end

  # Gets a single record by its ID
  # One argument: the id (number)
  def find(id)
    # Executes the SQL query:
    SELECT id, username, email_address FROM user_accounts; WHERE id = 1;

    # Returns a single Student object.
  end


  def create(user_account)
    INSERT INTO user_accounts (username, email_address) VALUES ('#{user_account.username}', '#{user_account.email_address}');
  end

  def delete(id)
    DELETE FROM user_accounts WHERE id = '#{id}';
end



class PostsRepository

  # Selecting all records
  # No arguments
  def all
    # Executes the SQL query:
    SELECT id, title, content, user_accounts_id FROM posts;

    # Returns an array of Album objects.
  end

  # Gets a single record by its ID
  # One argument: the id (number)
  def find(id)
    # Executes the SQL query:
    SELECT id, title, content, user_accounts_id FROM posts; WHERE id = 1;

    # Returns a single Student object.
  end


  def create(post)
    INSERT INTO posts (title, content, user_accounts_id) VALUES ('#{post.title', '#{post.content}', '#{post.user_accounts_id}');
  end

  def delete(id)
    DELETE FROM posts WHERE id = '#{id}';
end
```


## 6. Write Test Examples

Write Ruby code that defines the expected behaviour of the Repository class, following your design from the table written in step 5.

These examples will later be encoded as RSpec tests.

```ruby
# EXAMPLES

# 1
# Get all students

repo = UserAccountsRepository.new

users = repo.all

users.length # =>  2

users[0].id # =>  1
users[0].username # =>  'James'
users[0].email_address # =>  'James@gmail.com'

# 2
# Get a single student

repo = UserAccountsRepository.new

users = repo.find(2)

users.id # =>  2
users.title # =>  'Jack'
users.release_year # =>  'Jack@gmail.com'


repo = PostsRepository.new

posts = repo.find(1)

posts.id # =>  1
posts.title # =>  'My name is'
posts.content # =>  'James'
posts.artist_id # =>  '1'

# Add more examples for each method
```

Encode this example as a test.

## 7. Reload the SQL seeds before each test run

Running the SQL code present in the seed file will empty the table and re-insert the seed data.

This is so you get a fresh table contents every time you run the test suite.

```ruby
# EXAMPLE

# file: spec/albums_repository_spec.rb

def reset_social_network_table
  seed_sql = File.read('spec/seeds_albums.sql')
  connection = PG.connect({ host: '127.0.0.1', dbname: 'music_library_test' })
  connection.exec(seed_sql)
end

describe AlbumRepository do
  before(:each) do 
    reset_albums_table
  end

  # (your tests will go here).
end
```

## 8. Test-drive and implement the Repository class behaviour

_After each test you write, follow the test-driving process of red, green, refactor to implement the behaviour._

<!-- BEGIN GENERATED SECTION DO NOT EDIT -->

---

**How was this resource?**  
[😫](https://airtable.com/shrUJ3t7KLMqVRFKR?prefill_Repository=makersacademy%2Fdatabases&prefill_File=resources%2Frepository_class_recipe_template.md&prefill_Sentiment=😫) [😕](https://airtable.com/shrUJ3t7KLMqVRFKR?prefill_Repository=makersacademy%2Fdatabases&prefill_File=resources%2Frepository_class_recipe_template.md&prefill_Sentiment=😕) [😐](https://airtable.com/shrUJ3t7KLMqVRFKR?prefill_Repository=makersacademy%2Fdatabases&prefill_File=resources%2Frepository_class_recipe_template.md&prefill_Sentiment=😐) [🙂](https://airtable.com/shrUJ3t7KLMqVRFKR?prefill_Repository=makersacademy%2Fdatabases&prefill_File=resources%2Frepository_class_recipe_template.md&prefill_Sentiment=🙂) [😀](https://airtable.com/shrUJ3t7KLMqVRFKR?prefill_Repository=makersacademy%2Fdatabases&prefill_File=resources%2Frepository_class_recipe_template.md&prefill_Sentiment=😀)  
Click an emoji to tell us.

<!-- END GENERATED SECTION DO NOT EDIT -->
