# Personal Names

The goal of this gem is to carry on where Basecamp's "Name of Person" left off.  Name of Person is limited to the naming conventions of countries where English is the dominant language (and any others which follow the same conventions). I'll refer to this as the "Western European" naming conventions—although as I do more research into the personal naming conventions of the world's cultures I will likely need to refine that nomenclature.

From the original README file:
> Presenting names for English-language applications where a basic model of first and last name(s) combined is sufficient. This approach is not meant to cover all possible naming cases, deal with other languages, or even titulations. Just the basics.

The conventions of Western European naming is ubiquitous in our interconnected world, and people whose native conventions differ have come to find ways of dealing with systems that don't understand their names.  For example, I am not aware of Facebook and Twitter offering a localized version in Nigeria for Yoruba users who might have an arbitrarily long list of names, or a localized version in China that accounts for surnames coming before given names.  Even if they do that in China—which is actually quite plausible—there are undoubtedly other sites that do not.

However, why should it be this way?  Why not extend the courtesy to our users of honoring their naming systems?

The main reasons it is not done—I am supposing—are that 
1. many developers, managers, and UX designers in western countries are unaware of the differences;
2. not only would it require a lot of extra research, but it would require a lot of extra development effort;
3. the amount of effort required for each of these is completely unknowable ahead of time, so on a project timeline it would create a giant blackhole of resource use;
4. no one else is doing it, so everyone feels that using Western naming conventions is "good enough"; and
5. each new set of naming conventions makes not only the code for names more complex, and the database more complex, but potentially any code that interacts with either.

My use of language here is very deliberate: this is a courtesy which I desire to extend whenever possible, but which has a cost—a cost which cannot be overlooked.  No one is owed this courtesy as a debt, and yet I believe it fits perfectly with the ideals that I want to always strive towards and I hope other developers also want to strive towards. I believe our work is inherently better when it adapts to our users' needs rather than forcing our users to adapt to its needs.

## Examples

```ruby
# Relies on User having a schema which fits the supported naming system—in this case first_name and last_name columns.
class User < ApplicationRecord
  has_personal_names [:western]
end

# Saves a new record using { first_name: "David", last_name: "Heinemeier Hansson", familiar_name: "David" }
user = User.create! name: "David Heinemeier Hansson"

user.name.full        # => "David Heinemeier Hansson"
user.name.first       # => "David"
user.name.last        # => "Heinemeier Hansson"
user.name.initials    # => "DHH"
user.name.familiar    # => "David"  # other people might use "Dave", but I
user.name.abbreviated # => "D. Heinemeier Hansson"
user.name.sorted      # => "Heinemeier Hansson, David"
user.name.mentionable # => "davidh"
user.name.possessive  # => "David Heinemeier Hansson's"
user.name.possessive(:first)  # => "David's"
user.name.possessive(:last)  # => "Hansson's"
user.name.possessive(:initials)  # => "DHH's"
user.name.possessive(:sorted)  # => "Heinemeier Hansson, David's"
user.name.possessive(:abbreviated)  # => "D. Heinemeier Hansson's"


# Use directly
name = NameOfPerson::PersonName.full("David Heinemeier Hansson")
name.first # => "David"
# 🚨 This cannot accurately distinguish between middle names and composite last names
```

## Differences with Original "Name of Person" gem ##

The `familiar` method is used very differently in this gem, for the following reasons
1. there is sometimes a need to distinguish between someone actual legal first name, and a more casual form that is used by everyone who knows them.  Richard might go by Rich or Richie or Rick or even Dick.  Robert might go by Bob or Bobby or Rob or Robby.  None of those things can be derived from the actual first name.  It is necessary to obtain this information independently and store it in the database.
2. I can't see what purpose the previous `familiar` method had.

## Installation ##

```ruby
# Gemfile
gem 'personal_names'
```

If you are using this outside of Rails, make sure `ActiveRecord` and/or `ActiveModel` are manually required.

```ruby
require 'active_record'
# and/or
require 'active_model'
```

## Further development

The original gem that this comes from is frozen, as the developers have decided to only deal with full names going forward in their applications. This is a fork with additional plans as described above.

## License

Personal Names is released under the [MIT License](https://opensource.org/licenses/MIT).
