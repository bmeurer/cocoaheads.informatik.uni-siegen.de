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
	puts '* Compressing files'
	system "java -jar _lib/websitecompressor-0.4.jar --compress-css --compress-js _site"
end

desc 'Run the jekyll dev server'
task :server => [:clean] do
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

