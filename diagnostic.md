# Rails API Relationships Diagnostic

Place your responses inside the fenced code-blocks where indivated by comments.

1.  Describe a reason why a join tables may be valuable.

```sh
# If you have a many-to-many relationship, a join table is how you can bring
# the two main tables together.  Without it, only one of the tables may have
# many of the other table's values.
```

1.  Provide a database table structure and explain the Entity Relationship that
describes a many-to-many relationship for `Profiles`, `Movies` and `Favorites`
(Think of Netflix). A `Profile` has a `given_name`, `surname` and `email` and a
`Movies` have `title`, `release_date`, and `length` and `Favorites` would be the
join table with references to `Movies` and `Profiles`.

```sh
# A profile would have many movies through Favorites and a movie would have many
# profiles through favorites as well.  Both would also have many favorites and
# favorites would belong to movie and profile.
```

1.  For the above example, what needs to be added to the Model files?

```rb
class Profile < ActiveRecord::Base
  has_many :movies, through: :favorites
  has_many :favorites, dependent: :destroy
end
```

```rb
class Movie < ActiveRecord::Base
  has_many :profiles, through: :favorites
  has_many :favorites
end
```

```rb
class Favorite < ActiveRecord::Base
  belongs_to :profile, inverse_of: :favorites
  belongs_to :movie, inverse_of: :favorites
end
```

1.  What is the purpose of a serializer? What would our `Profile` serializer look
like to show all movies favorited by a profile on
`http://localhost:3000/profiles/1`

```sh
# A serializer lets us determine what data we get returned from our SQL database.
# If you don't care about some particular data in a table, you can choose for it
# to not be returned.  Our 'profile' serializer would have to include :favorites
# as one of the attributes you would like returned.
```

```rb
class ProfileSerializer < ActiveModel::Serializer
  attributes :given_name, :surname, :email, :favorites
end
```

1.  What would the command be to _scaffold_ out a **join table** for Favorites from
the above `Movies` and `Profiles`.

```sh
  bundle exec rails g scaffold Favorites is_favorite:char(1) movie:references profile:references
```

1.  What is `Dependent: Destroy` and where/why would we use it?

```sh
# Dependent: destroy enables us to remove data from one table and have any other
# data in any other table that relies on the deleted information to be deleted
# as well.  The declaration would live in the model of the table where information.
# is often deleted.
```

1.  Think of **ANY** example where you would have a one-to-many relationship as well
as a many-to-many relationship in an application. You only need to list the
description about the resources and how they relate to one another.

```sh
  # In an app where there are comments and favoriting of posts.  A profile may
  # make many comments, but one comment belongs to just one profile.  But a profile
  # may favorite many posts, and posts my be favorited by many difference profiles.
```
