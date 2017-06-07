# Asset pipeline: including manifest files, concationation & minification


## Explanation of what it does

## Where does this code go (file/folders)

## How to install or set it up

## Example Code


---------------
[Asset Pipeline](http://guides.rubyonrails.org/v3.2/asset_pipeline.html)
This guide covers the asset pipeline introduced in Rails 3.1. By referring to this guide you will be able to:

Understand what the asset pipeline is and what it does
	# What is an assest?
	# What does it do?

	## Snapshot
	* compile multiple assets into one
	* minify and compress assets
	* provide digesting of assets to avoid caching issues
	* pre-process alternative languages and turn them into Javascript and CSS.
	--------------

	# What is the Asset Pipeline?

	If you’re building a Rails application, you’ve probably heard of the asset pipeline. The asset pipeline can be thought of as the tools and mechanisms by which Javascript files, stylesheets, and images are processed and prepared for use by the browser. These processes can minify and compress assets, as well as pre-process assets that are written in other languages such as Coffeescript or Sass.

	The asset pipeline was created to solve a variety of problems related to static assets. One such issue is that each asset specified separately in the HTML markup must be retrieved separately, resulting in a higher number of HTTP requests and, in the end, a longer load time. Raw Javascript and CSS files can also waste a lot of bandwidth with comments, extra white space, and long variable names. Another issue that comes up involves caching. When you serve up a Javascript file from your server, for example, the browser will automatically cache that file for a period of time. That improves page load time, but what if that asset changes at a later point in time? The browser won’t know about it, so it will continue to use the cached asset until its cache life has expired. Finally, languages such as Coffeescript, Sass, Less, and Erb have made it easier to organize and write Javascript and CSS, but the browser can’t interpret them directly, so a pre-processor is needed to convert those files into their appropriate counterparts before they are sent off to the browser.

	Making the asset pipeline a core feature of Rails means that all developers can benefit from the power of having their assets pre-processed, compressed and minified by one central library, Sprockets.
	[rails.docs](http://guides.rubyonrails.org/v3.2/asset_pipeline.html)


	# In a nutshell
	Using the asset pipeline properly can improve the overall quality of your application in terms of performance, resilience, and code cleanliness. By using the asset pipeline’s features, you can automatically overcome some of the biggest issues related to coding and delivering static assets.

	### What is the Asset Pipeline?
	The asset pipeline provides a framework to concatenate and minify or compress JavaScript and CSS assets. It also adds the ability to write these assets in other languages such as CoffeeScript, Sass and ERB.
	Rails 3.1 is integrated with Sprockets through Action Pack which depends on the sprockets gem, by default.
	all developers can benefit from the power of having their assets pre-processed, compressed and minified by one central library, Sprockets. 

	It's enables by default but can be skipped by adding this to your `config/application.rb` file

	```ruby
		config.assets.enabled = false # avoid the default behavior of having your assets pre-processed, compressed and minified by one central library

	```

	You can also disable the asset pipeline while creating a new application by passing the —skip-sprockets option.
	In your terminal run: 

	```
		rails new appname --skip-sprockets
	```
	Just in case you're wondering, you probably shouldn't disable this funcationality unless you have a specific reason. But it's helpful to have this information for furture refference.

Properly organize your application assets
	CONFIGURATION

	In Rails 3.1, the asset pipeline is enabled by default. It can be disabled in config/application.rb by putting this line inside the application class definition:

	```
	config.assets.enabled = false
	```

	### 2.1 Asset Organization
	Pipeline assets can be placed inside an application in one of three locations: 
	* app/assets
	  for assets that are owned by the application, such as custom images, JavaScript files or stylesheets.
	* lib/assets
	  for your own libraries’ code that doesn’t really fit into the scope of the application or those libraries which are shared across applications.
	* vendor/assets
	  for assets that are owned by outside entities, such as code for JavaScript plugins and CSS frameworks.

	2.1.1 Search paths
	When a file is referenced from a manifest or a helper, Sprockets searches the three default asset locations for it.
	The default locations are: app/assets/images and the subdirectories javascripts and stylesheets in all three asset locations.
	app/assets/javascripts/home.js would be referenced in a manifest like this: //= require home
	lib/assets/javascripts/moovinator.js would be referenced in a manifest like this: //= require moovinator
	vendor/assets/javascripts/slider.js would be referenced in a manifest like this: //= require slider

	app/assets/javascripts/sub/something.js would be referenced in a manifest like this: //= require sub/something

	You can view the search path by inspecting Rails.application.config.assets.paths in the Rails console.

	Additional (fully qualified) paths can be added to the pipeline in config/application.rb. 
	config.assets.paths << Rails.root.join("app", "assets", "flash")

	It is important to note that files you want to reference outside a manifest must be added to the precompile array or they will not be available in the production environment.


	2.1.2 Using index files
	Sprockets uses files named index (with the relevant extensions) for a special purpose.

	For example, if you have a jQuery library with many modules, which is stored in lib/assets/library_name, the file lib/assets/library_name/index.js serves as the manifest for all files in this library. 

	This file could include a list of all the required files in order, or a simple require_tree directive.

	The library as a whole can be accessed in the site’s application manifest like so:

	//= require library_name

	### 2.3 Manifest Files and Directives
	Sprockets uses manifest files to determine which assets to include and serve. These manifest files contain directives — instructions that tell Sprockets which files to require in order to build a single CSS or JavaScript file. With these directives, Sprockets loads the files specified, processes them if necessary, concatenates them into one single file and then compresses them (if Rails.application.config.assets.compress is true). By serving one file rather than many, the load time of pages can be greatly reduced because the browser makes fewer requests.

	### 2.4 Preprocessing
	The file extensions used on an asset determine what preprocessing is applied.
	Keep in mind that the order of these preprocessors is important. For example, if you called your JavaScript file app/assets/javascripts/projects.js.erb.coffee then it would be processed with the CoffeeScript interpreter first, which wouldn’t understand ERB and therefore you would run into problems.

	5.4 Changing the assets Path
	This can be changed to something else:
	config.assets.prefix = "/some_other_path"
	This is a handy option if you are updating an existing project (pre Rails 3.1) that already uses this path or you wish to use this path for a new resource.

	### 5 Customizing the Pipeline
	5.1 CSS Compression

	The following line enables YUI compression, and requires the yui-compressor gem.
	```
	config.assets.css_compressor = :yui
	```
	The config.assets.compress must be set to true to enable CSS compression.

	# Use Uglifier as compressor for JavaScript assets
	gem 'uglifier', '>= 1.3.0'

	### 2 How to Use the Asset Pipeline

	In previous versions of Rails, all assets were located in subdirectories of public such as images, javascripts and stylesheets.

	With the asset pipeline, the preferred location for these assets is now the app/assets directory. Files in this directory are served by the Sprockets middleware included in the sprockets gem.

	Any assets under public will be served as static files by the application or web server. You should use app/assets for files that must undergo some pre-processing before they are served.

	In production, Rails precompiles these files to public/assets by default. The precompiled copies are then served as static assets by the web server. The files in app/assets are never served directly in production.


	For example, if you generate a ProjectsController, Rails will also add a new file at app/assets/javascripts/projects.js.coffee and another at app/assets/stylesheets/projects.css.scss. You should put any JavaScript or CSS unique to a controller inside their respective asset files, as these files can then be loaded just for these controllers with lines such as <%= javascript_include_tag params[:controller] %> or <%= stylesheet_link_tag params[:controller] %>.

	### 3 In Development
	In development mode, assets are served as separate files in the order they are specified in the manifest file.

	This manifest app/assets/javascripts/application.js:

	```
	//= require core => would generate this HTML: <script src="/assets/core.js?body=1" type="text/javascript"></script>
	//= require projects => would generate this HTML: <script src="/assets/projects.js?body=1" type="text/javascript"></script>
	//= require tickets => would generate this HTML: <script src="/assets/tickets.js?body=1" type="text/javascript"></script>

	```
	Assets are compiled and cached on the first request after the server is started. Sprockets sets a must-revalidate Cache-Control HTTP header to reduce request overhead on subsequent requests — on these the browser gets a 304 (Not Modified) response.


	If any of the files in the manifest have changed between requests, the server responds with a new compiled file.

	### 4 In Production
	By default Rails assumes that assets have been precompiled and will be served as static assets by your web server.

	Rails comes bundled with a rake task to compile the asset manifests and other files in the pipeline to the disk.


	You can call this task on the server during deployment to create compiled versions of your assets directly on the server. If you do not have write access to your production file system, you can call this task locally and then deploy the compiled assets.

	The rake task is:

	```
	bundle exec rake assets:precompile
	```

Understand the benefits of the asset pipeline
	### 1.1 Main Features
	* to concatenate assets
	  why?: This is important in a production environment, because it can reduce the number of requests that a browser must make to render a web page, fewer requests can mean faster loading for your application
	   Rails (3.1+) defaults to concatenating all JavaScript files into one master .js file and all CSS files into one master .css file which can later be customized to group files any way you like.
	   In production, Rails inserts an MD5 fingerprint into each filename so that the file is cached by the web browser.
	* asset minification or compression
		why?:  For CSS files, this is done by removing whitespace and comments. For JavaScript, more complex processes can be applied. You can choose from a set of built in options or specify your own.
	* allows coding assets via a higher-level language, with precompilation down to the actual assets
	  why?: Obvious. Sass, CoffeeScript, & ERB for both by default.

	### 1.2 What is Fingerprinting and Why Should I Care?

	Fingerprinting is a technique that makes the name of a file dependent on the contents of the file. 
		When the file contents change, the filename is also changed. For content that is static or infrequently changed, this provides an easy way to tell whether two versions of a file are identical, even across different servers or deployment dates. When the content is updated, the fingerprint will change. This will cause the remote clients to request a new copy of the content. This is generally known as cache busting.

	The technique that Rails uses for fingerprinting is to insert a hash of the content into the name, usually at the end. For example a CSS file global.css could be renamed with an MD5 digest of its contents:
	```
	# new method of renaming files using an MD5 digest of its contents:
	global-908e25f4bf641868d8683022a5b62f54.css

	# old method using a date-based query string on every asset
	/stylesheets/global.css?1309495796

	```
	The old method was also linked with a built-in helper
	Rails has migrated to fingerprinting because there were a few disadvantages to using string queries, like sometimes they wouldn't be cashed, diferent servers giving diferent values to the query string because there was no garunteed that the requests were made at the same time, and too much cache invalidation--causing browsers to fetch when in fact no changes had occured within the files :(

	Fingerprinting ensuring that filenames are consistent based on their content, and avoids these problems by not using a query string based on dates.

	Fingerprinting is enabled by default for production and disabled for all other environments. You can enable or disable it in your configuration through the config.assets.digest option.

<!-- Add a pre-processor to the pipeline -->

Package assets with a gem
	### 7 Adding Assets to Your Gems
	Assets can also come from external sources in the form of gems.

	A good example of this is the jquery-rails gem which comes with Rails as the standard JavaScript library gem. This gem contains an engine class which inherits from Rails::Engine. By doing this, Rails is informed that the directory for this gem may contain assets and the app/assets, lib/assets and vendor/assets directories of this engine are added to the search path of Sprockets.

--------------------------------------------------------------------------------------------------------------

# Acknowledgements
A special thanks to launch school's blog, (Everything You Should Know About the Rails Asset Pipeline)[https://launchschool.com/blog/rails-asset-pipeline-best-practices]
