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
	puts '* Compressing CSS files'
	system 'find _site -name "*.css" -exec java -jar _lib/yuicompressor-2.4.6.jar --charset UTF-8 --type css "{}" -o "{}" ";"'
	puts '* Compressing HTML files'
	system 'find _site -name "*.html" -exec java -jar _lib/htmlcompressor-1.4.jar --charset UTF-8 --type html --compress-css --compress-js "{}" -o "{}" ";"'
	puts '* Compressing XML files'
	system 'find _site -name "*.xml" -exec java -jar _lib/htmlcompressor-1.4.jar --charset UTF-8 --type xml "{}" -o "{}" ";"'
end

desc 'Run the jekyll dev server'
task :server => [:compress] do
	system "jekyll --server --auto"
end

desc 'Build & Deploy'
task :deploy => [:compress] do
  require 'net/http'
  require 'uri'
  puts '* Deploying website'
  system 'rsync -rtzh --delete _site/ vcs:/srv/httpd/cocoaheads'
  puts '* Pinging Google about the sitemap'
  Net::HTTP.get('www.google.com', '/webmasters/tools/ping?sitemap=' + URI.escape('http://cocoaheads.informatik.uni-siegen.de/sitemap.xml'))
end

task :default => [:server]

