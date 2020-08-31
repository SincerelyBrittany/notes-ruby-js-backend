# README

#Step By Step Build

#Setting Up The Rails API

1. Create a folder 
        ``mkdir todo-js-ruby``
2. cd into ``mkdir todo-js-ruby``
3. Create a new rails app -- ``rails new <app-name> -api --database-postgresql
4. cd into the new-app-name

#Gemfile 

1. Uncomment out ``gem 'rack-cors'``
2. Run bundle 
3. Go to your ``config/initializers/cors.rb`` file. 

Uncomment/add the following code: 

```
Rails.application.config.middleware.insert_before 0, Rack::Cors do
  allow do
    origins '*'

    resource '*',
      headers: :any,
      methods: [:get, :post, :put, :patch, :delete, :options, :head]
  end
end
```

#Create a controller:
1. rails g controller notes
2. Create a version folder. ``app/controllers/api/v1``
3. Add controller into version folder --> ``app/controllers/api/v1/notes_controller.rb``
4. Namespace the controller: 

```
class Api::V1::NotesController < ApplicationController
end
```

5. Add data into your controller -- rememeber to render json

```
class Api::V1::NotesController < ApplicationController
    def index
        @notes = Note.all
        render json: @notes, status: 200
    end 

    def show 
        @note = Note.find(params[:id])
        render json: @note, status: 200
    end 

    def create
        @note = Note.create(note_params)
        render json: @note, status: 200
    end 

    def update
        @note = Note.find(params[:id])
        @note.update(note_params)
        render json: @note, status: 200
    end 

    def destroy
        @note = Note.find(params[:id])
        @note.destroy

        render json: {noteId: @note.id}#, status: 200
    end 

    private
        def note_params
            params.require(:note).permit(:body)
        end 
end
```

#Routes:

1. Update the routes to reference the api/ make sure to namespace
```
  namespace :api do 
    namespace :v1 do
      resources :notes
    end 
  end
```

#Create a model
1. ``rails g model Note``
2. Update the table that is generated
3. rails db:create
4. rails db:migrate

#Create seed.rb 

```
Note.create([
    {body: "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nullam non."},
    {body: "Aliquam a ullamcorper arcu. Fusce consectetur, sem id egestas aliquet, velit eros laoreet lorem, non imperdiet eros tellus auctor turpis."},
    {body: "Praesent risus mi, rhoncus non augue nec, malesuada vehicula mauris. Sed at interdum odio. Donec varius porta leo efficitur eleifend." },
    {body: "Tauris erat lacus, ultricies non tortor nec, aliquam consequat ipsum. Donec ut elit placerat urna posuere congue a et neque."},

])
```

2. rails s
3. visit http://localhost:3000/api/v1/notes

Then work on the front end. . .keep the ruby server running so you can reference your database/json.
Next,create a new diretory inside of the main directory
1. cd into ``mkdir todo-js-ruby``
2. ``mkdir js-frontend``
3. ``cd js-frontend``