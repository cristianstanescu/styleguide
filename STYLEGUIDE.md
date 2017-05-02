# Programming with style

> The long-term value of software to an organization is in direct proportion to
> the quality of the codebase. Over its lifetime, a program will be handled by
> many pairs of hands and eyes. If a program is able to clearly communicate its
> structure and characteristics, it is less likely that it will break when
> modified in the never-too-distant future. Code conventions can help in
> reducing the brittleness of programs.
>
> -- Douglas Crockford [Code Conventions for the JavaScript Programming
> Language](http://javascript.crockford.com/code.html)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [Styles](#styles)
  - [Ruby](#ruby)
  - [Rails](#rails)
  - [CoffeScript](#coffescript)
  - [JavaScript](#javascript)
  - [Java](#java)
- [How to use the style guides](#how-to-use-the-style-guides)
  - [Overcommit](#overcommit)
  - [Guard](#guard)
  - [Editors](#editors)
    - [Sublime](#sublime)
    - [Vim](#vim)
    - [Atom](#atom)
- [More Static Analysis](#more-static-analysis)

---

<!-- END doctoc generated TOC please keep comment here to allow auto update -->
High level guidelines<sup>[[reference](https://github.com/thoughtbot/guides)]</sup>:

- Be consistent.
- Don't rewrite existing code to follow this guide.
- Don't violate a guideline without a good reason.
- A reason is good when you can convince a teammate.

Very quick start (Ruby):

```
cd /into/project/for/the/Ruby/version
gem install rubocop
gem install rubycritic
rubycritic app lib # or just a single file
rubocop app lib spec test # or just a single file
```

# Styles

## Ruby

Use [Ruby Style Guide](https://github.com/bbatsov/ruby-style-guide) as a reference for Ruby style. It's the Ruby community list of widely accepted and recommended best practices. These are recommendations that are based on real-world experience and are enforced automatically through [Rubocop](https://github.com/bbatsov/rubocop "Rubocop's Github page"), a static code analyzer that verifies code through little Ruby classes called *cops*.

The full list of enabled *cops* is listed [here](https://github.com/bbatsov/rubocop/blob/master/config/enabled.yml "Rubocop enabled cops") with the defaults [here](https://github.com/bbatsov/rubocop/blob/master/config/default.yml). The Swoop team's opinion overrides defaults through the root [.rubocop.yml](/.rubocop.yml).

**Styles different than the Rubocop default:**

- [Style/IndentArray](https://github.com/bbatsov/rubocop/blob/master/lib/rubocop/cop/style/indent_array.rb)

  Use `consistent` which means that the indentation of the first element shall
  always be relative to the first position of the line where the opening
  bracket is.

  ```ruby
  array = [
    :value
  ]

  and_in_a_method_call([
    :no_difference
  ])
  ```

- [Metrics/ClassLength](https://github.com/bbatsov/rubocop/blob/master/lib/rubocop/cop/metrics/class_length.rb)

  The default of 100 lines is probably based on  [Sandi Metz's
  recommendations](https://robots.thoughtbot.com/sandi-metz-rules-for-developers#100-line-classes) but it could be too drastic in many
  cases. **250** excluding comments should allow more freedom though still
  not violate the Single Responsibility Principle. 100 lines should still be
  the goal, Github also [recommends it](https://github.com/github/rubocop-github/blob/master/STYLEGUIDE.md).

- [Style/AlignParameters](https://github.com/bbatsov/rubocop/blob/master/lib/rubocop/cop/style/align_parameters.rb)

  Use `with_fixed_indentation` style which aligns the following lines with one
  level of indentation relative to the start of the line with the method call.

  ```ruby
  method_call(a,
    b)
  ```

- [Style/BlockDelimiters](https://github.com/bbatsov/rubocop/blob/master/lib/rubocop/cop/style/block_delimiters.rb)

  Use `semantic` style which enforces braces around functional blocks, where the
  primary purpose of the block is to return a value and *do..end* for procedural
  blocks, where the primary purpose of the block is its side-effects.

  See [Avdi Grimm's blog](http://www.virtuouscode.com/2011/07/26/the-procedurefunction-block-convention-in-ruby/) for the advantages of choosing a
  block style based on semantics rather than line length.

  Examples:

  ```ruby
  # functional block
  options = open('options.yml') { |f|
    YAML.load(f)
  }

  # procedural block
  open("$0.pid", 'w+') do |f|
    f.write($$.to_s)
  end
  ```

- [Style/DotPosition](https://github.com/bbatsov/rubocop/blob/master/lib/rubocop/cop/style/dot_position.rb)

  Use `trailing` so we know there's something going on to an object after the
  accepted line length (80 chars so it looks good when *diffing*) has ended.

- [Style/DoubleNegation](https://github.com/bbatsov/rubocop/blob/master/lib/rubocop/cop/style/double_negation.rb)

  Allowed as this can be considered a Ruby idiom.

- [Metrics/MethodLength](https://github.com/bbatsov/rubocop/blob/master/lib/rubocop/cop/metrics/method_length.rb)

  [Sandi Metz's   recommendations](https://robots.thoughtbot.com/sandi-metz-rules-for-developers#100-line-classes) recommends 5, Rubocop default was 10,
  **20** is allowed but having that many might be a sign of complexity.


## Rails

Also Enabled through [Rubocop](https://github.com/bbatsov/rubocop "Rubocop's Github page") and based on the recommendations at [Rails Style Guide](https://github.com/bbatsov/rails-style-guide). Note the organization of ActiveRecord classes regarding [macro-style methods](https://github.com/bbatsov/rails-style-guide#macro-style-methods).

**Styles different than the Rubocop default:**

- [Rails/HasAndBelongsToMany](https://github.com/bbatsov/rubocop/blob/master/lib/rubocop/cop/rails/has_and_belongs_to_many.rb)

    `has_and_belongs_to_many` should be fine in most cases. `has_many :through`
    can have a semantic value communicating that the many to many association
    is not just a cross reference but something more intersting is going on
    there.

- [Rails/HttpPositionalArguments](https://github.com/bbatsov/rubocop/blob/master/lib/rubocop/cop/rails/http_positional_arguments.rb)

    Specific to Rails 5.

**Other recommendations inspired by [Thoughtbot](https://github.com/thoughtbot/guides/tree/master/style/rails)**

- Order ActiveRecord associations alphabetically by association type, then attribute name. [Example](https://github.com/thoughtbot/guides/blob/master/style/rails/sample.rb#L2-L4)

- Order ActiveRecord validations alphabetically by attribute name.

- Order ActiveRecord associations above ActiveRecord validations.

- Order controller contents: filters, public methods, private methods.

## CoffeScript

## JavaScript

## Java

# How to use the style guides

While the styles guides are common for the whole project and all the
contributors should be aware of them, the way styling is enforced should be the responsibility of each developer.

Any aditional tool used that requires a configuration in the root of the project
and is a preference of the developer can be added and included into
`~/.gitignore_global` so it won't interfere with the way others are building
code.

## Overcommit

[Overcommit](https://github.com/brigade/overcommit) runs git hooks depending on the files staged for commit. If you will commit Ruby files, it will run Ruby related hooks ([Reek](https://github.com/brigade/overcommit/blob/master/lib/overcommit/hook/pre_commit/reek.rb), [RuboCop](https://github.com/brigade/overcommit/blob/master/lib/overcommit/hook/pre_commit/rubo_cop.rb)) for CoffeScript, CoffeScript related hooks ([CoffeeLint](https://github.com/brigade/overcommit/blob/master/lib/overcommit/hook/pre_commit/coffee_lint.rb)) for JavaScript, JavaScript hooks ([JsHint](https://github.com/brigade/overcommit/blob/master/lib/overcommit/hook/pre_commit/js_hint.rb), [Jscs](https://github.com/brigade/overcommit/blob/master/lib/overcommit/hook/pre_commit/jscs.rb)) and so on.

In addition to the hooks, Overcommit analyses the commit message and helps
enforcing a standard format: message subject line of 50 chars, body length of
72, configurable but according to recommendations, one of most cited being [Tim Pope](http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html).

## Guard

[Guard](https://github.com/guard/guard) can be used to watch file changes and
check for style or complexity violations when developing. It runs in the
terminal when you write code and verifies files through guard plugins: [Rubocop](https://github.com/yujinakayama/guard-rubocop), [Rubycritic](https://github.com/whitesmith/guard-rubycritic), [CoffeeLint](http://github.com/meagar/guard-coffeelint).

The plugins need to be installed separately from a [bunch of options](https://github.com/guard/guard/wiki/Guard-Plugins).

## Editors

### Sublime

Rubocop can be installed through packages like [RuboCop](https://packagecontrol.io/packages/RuboCop) or [SublimeLinter-rubocop](https://packagecontrol.io/packages/SublimeLinter-rubocop).

### Vim

### Atom

# More Static Analysis

[Reek](https://github.com/troessner/reek) analyses Ruby code and finds [code smells](https://github.com/troessner/reek/blob/master/docs/Code-Smells.md).

[Flog](https://github.com/seattlerb/flog) "reports the most tortured code in an easy to read pain report. The higher the score, the more pain the code is in."

[Flay](https://github.com/seattlerb/flay) "analyzes code for structural similarities."

All these three can be installed and run through
[Rubycritic](https://github.com/whitesmith/rubycritic) while generating a nice
looking HTML report of the problems inside the codebase:

```
gem install rubycritic
rubycritic app lib
```

There is also a [Guard plugin](https://github.com/whitesmith/guard-rubycritic) for Rubycritic allowing to run Reek, Flay and Flog while developing and getting
to see any problems live to allow fixing them on the spot.
