desc 'Delete generated _site files'
task :clean do
	system "rm -fR _site"
end

desc 'Clean temporary files and compile the site'
task :compile => [:clean] do
	system "jekyll --no-server"
end

desc 'Compress CSS & HTML'
task :compress => [:compile] do
  system 'java -jar _lib/yuicompressor-2.4.6.jar --charset UTF-8 --type css _site/css/style.css -o _site/css/style.css'
  system 'java -jar _lib/htmlcompressor-1.4.jar --charset UTF-8 --type html --compress-css --compress-js _site/index.html -o _site/index.html'
  system 'java -jar _lib/htmlcompressor-1.4.jar --charset UTF-8 --type xml _site/sitemap.xml -o _site/sitemap.xml'
end

desc 'Run the jekyll dev server'
task :server => [:compress] do
	system "jekyll --server --auto"
end

desc 'Build & Deploy'
task :deploy => [:compress] do
  puts '* Deploying Website'
  system 'rsync -vrtzh --delete _site/ vcs:/srv/httpd/cocoaheads'
end

desc 'Notify Google of the new sitemap'
task :sitemap => [:deploy] do
  require 'net/http'
  require 'uri'
  puts '* Pinging Google about the sitemap'
  Net::HTTP.get('www.google.com', '/webmasters/tools/ping?sitemap=' + URI.escape('http://cocoaheads.informatik.uni-siegen.de/sitemap.xml'))
end

task :default => [:server]

