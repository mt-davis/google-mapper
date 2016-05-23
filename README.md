# Google Mapper Project
Google Mapper is a Rails App created using: 
[gmaps4rails gem](https://github.com/apneadiving/Google-Maps-for-Rails)
[geocoder gem](https://github.com/alexreisner/geocoder)
[bootstrap generator gem](https://github.com/decioferreira/bootstrap-generators)

##Installtion
The steps to build a working version of this app is as follows:

## Getting started

## Installing Gems
Optional: Install bootstrap generator gem for front-end
```ruby
gem 'bootstrap-generators'
```
Install Geocoder like any other Ruby gem:
```ruby
gem 'geocoder'
```
Next add the gmaps4rails gem to the Gemfile
```ruby
gem 'gmaps4rails'
```
Run bundle install
```ruby
bundle install
```
##Generators
Install bootstrap .This will install all of the bootstrap components
```ruby
rails g bootstrap:install
```
As an example, create a User MVC with the following attrbutes:
```ruby
 "rails g scaffold User latitude:float longitude:float name:string address:string title:string"
 ```
Run
```ruby
"rake db:migrate"
```
##Configuration
If you like, change your "route.rb" file so that the root refelcts:
```ruby
root 'users#index'
```
This will ensure that  your home page will always default to the users/index.html.erb page.
At this point, it might be good to restart your rails server.

##Requirements
Add the following to your user model file "users.rb" which is located in models folder.
```ruby
class User < ActiveRecord::Base
    
    geocoded_by :address
    after_validation :geocode
    
end
```
Once you have successfully modified the model file, you should see the latitude and longitude in your users views. 
To get more information on the [geocode gem](https://github.com/alexreisner/geocoder)

Javascript Dependencies
you want to add the follwoing at the top of the page where you want to display your map.
```ruby
<script src="//maps.google.com/maps/api/js?v=3.23&sensor=false&client=&key=&libraries=geometry&language=&hl=&region="></script> 
<script src="//cdn.rawgit.com/mahnunchik/markerclustererplus/master/dist/markerclusterer.min.js"></script>
<script src='//cdn.rawgit.com/printercu/google-maps-utility-library-v3-read-only/master/infobox/src/infobox_packed.js' type='text/javascript'></script> <!-- only if you need custom infoboxes -->
```
You will be require to make a "underscore.js" file then copy and paste the "Production Version" of this code [here](http://underscorejs.org/underscore-min.js)  and place it in the
"underscore.js" you just created. Then move the file in to the "vendor\assets\javascripts" folder To find out more about Uderscore.js go to [underscorejs.org](http://underscorejs.org/)

You want to add the following into the appication.js file which is located in "app\assets\javascripts\appication.js"
```ruby
//= require underscore
//= require gmaps/google
```
Next run
```ruby
rails g gmaps4rails:copy_js
```
This will import the asset pipline files.

HTML
Add the div to hold the map code in the file you want to display the map, exmaple:
```ruby
<div style='width: 800px;'>
  <div id="map" style='width: 800px; height: 400px;'></div>
</div>
```

Next you want to add the following Javascript code on the page where you want your map to show. For example, I have mine below the div I created to 
hold hte map.
```ruby
<script type="text/javascript">
  handler = Gmaps.build('Google');
handler.buildMap({
    provider: {
      disableDefaultUI: false
      // pass in other Google Maps API options here
    },
    internal: {
      id: 'geolocation'
    }
  },
  function(){
    markers = handler.addMarkers(<%=raw @hash.to_json %>);
    handler.bounds.extendWith(markers);
    handler.fitMapToBounds();
  }
);
</script>
```
Next you want to add the follwoing to your user controller
```ruby
@hash = Gmaps4rails.build_markers(@users) do |user, marker|
  marker.lat user.latitude
  marker.lng user.longitude
  ```
  It should look something like this:
  ```ruby
  def index
    @users = User.all
    @hash = Gmaps4rails.build_markers(@users) do |user, marker|
  marker.lat user.latitude
  marker.lng user.longitude
end
end
```
By now, you should have a map on your page. Along with the locations of any users you have entered.
If you like you can add address or any other string about each user. Just add this line below the "marker.lng user.longitude" in the user controller file.
I'm using address as an example.
```ruby
marker.infowindow user.address
```
