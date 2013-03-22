# Thesis

### Thesis is a CMS that integrates as seamlessly as possible into your current website.

Most Rails content management systems make you conform to their system from the start, 
making it difficult to just "drop in" the gem and make it work with your CMS. Thesis
tries to be a drop-in CMS that doesn't hijack your development workflow.

## Getting Started

### Installation

In your Gemfile:

    gem 'thesis'
    
Then run these from your command line:

    bundle install
    rails g thesis:install
    rake db:migrate
    
This will install thesis and add the database tables.

### Authentication

Thesis adds a method into your application_controller.rb file that
allows you to hook up your own authentication logic. If you return
`false` from this method, nothing will show up client-side nor will
the page be editable. If you return `true` from this method, the
Thesis editor will appear.

```ruby
# Thesis authentication
def page_is_editable?(page)
  # Add your own criteria here for editing privileges. Examples:
  # current_user.admin? # Basic admin
  # can? :update, page # CanCan
  true # EVERYONE has access right now.
end
```

### Page Templates

Thesis's installer will drop a `page_templates` folder into your `app/views` folder.
This is where you put different styles of pages for use in the CMS.
Thesis will install either an ERB or HAML version, depending on your configuration

#### Meta information

```haml
%title= current_page.title
%meta{ content: current_page.content("Description", :text), type: "description" }
```

#### Thesis Editor

```haml
%body
  = thesis_editor
```

#### Page title

```haml
%h1= current_page.title
```

#### Primary Navigation

Use `root_pages` to get a list of pages at the root level.

```haml
%nav
  %ul
    - root_pages.each do |p|
      %li= link_to p.title, p.url
```

#### Page content

Content areas are accessible from any page using the `content` method. This method
takes two arguments: name and type. Type defaults to `:html`. The only other type
is `:text` (for now) which is plain text, no HTML accepted.

Referencing a content area will create one if it doesn't exist already.

```haml
%article
  = current_page.content("Main Content", :html)
%aside
  = current_page.content("Sidebar Content", :html)  
%footer
  %p= current_page.content("Footer Content", :text)
```

### Routing
        
Thesis will also add a route handler to your `routes.rb` file:

    thesis_routes
    
This will handle routes for pages you create with Thesis.

**To improve performance, put this near the bottom of your `routes.rb` file.**

### 


## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Write tests for your new feature
5. Run `rake spec` to ensure that all tests pass # TODO: implement tests
6. Push to the branch (`git push origin my-new-feature`)
7. Create new Pull Request
