Representative View
===================

"Representative View" integrates [Representative](http://github.com/mdub/representative) as a Rails template format, making it possible to generate XML and JSON representations of data <u>using the same template</u>.

Installing it
-------------

Simply add the '`representative_view`' gem to your `Gemfile` 

    gem "representative_view"
    
and run "`bundle install`".

Using it
--------

In your controller, declare that you can provide both XML and JSON, e.g.

    class BooksController < ApplicationController
  
      def index
        respond_to do |format|
          format.xml
          format.json
        end
      end
  
    end

Next, create a template with the suffix "`.rep`" (to select the Representative View template handler) and use the Representative DSL to generate elements and lists, e.g.

    # app/views/books/index.rep
    
    r.list_of :books, @books do
      r.element :title
      r.list_of :authors
    end

Note that it's "`index.rep`", not "`index.xml.rep`" or "`index.json.rep`"; those will work, too, but by omitting the format specifier, the same template can be used to render both formats.

### Partials

Representative View happily supports the use of partials, as long as they're also in Representative format.  Because Representative keeps track of the current "subject" as it renders, there's no need to explicitly pass an object into the partial:

    # app/views/books/index.rep

    r.list_of :books, @books do
      render :partial => 'book'
    end

    # app/views/books/_book.rep

    r.element :title
    r.element :published do
      r.element :by
    end

Configuration
-------------

Output can be controlled somewhat setting `json_options` or `xml_options` on the RepresentativeView module; these options will be passed when initialising the appropriate Representative::Nokogiri or Representative::JSON object.

For example, setting:

    RepresentativeView.json_options = {:naming_strategy => :camelCase}

causes JSON to be output with camelCase (rather than snake_case) labels.
