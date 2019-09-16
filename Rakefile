require "rubygems"
require 'rake'
require 'yaml'
require 'time'

SOURCE = "."
CONFIG = {
	'version_file' => File.join(SOURCE, '_version.yml'),
	'layouts' => File.join(SOURCE, '_layouts'),
	'posts' => File.join(SOURCE, '_posts'),
	'post_ext' => 'md',
}

def agree?(message)
	valid_options = [ 'y', 'n' ]
	answer = get_stdin("#{message} ").downcase[0]

	if answer == 'y'
		true
	else
		false
	end
end

def get_stdin(message)
  print message

  STDIN.gets.chomp
end

desc "Begin a new post in #{CONFIG['posts']}"
task :post do
	abort("rake aborted: '#{CONFIG['posts']}' directory not found.") unless FileTest.directory?(CONFIG['posts'])
	title = ENV["title"] || "new-post"
	tags = ENV["tags"] || "[]"
	slug = title.downcase.strip.gsub(' ', '-').gsub(/[^\w-]/, '')

	begin
		date = (ENV['date'] ? Time.parse(ENV['date']) : Time.now).strftime('%Y-%m-%d')
	rescue => e
		puts "Error - date format must be YYYY-MM-DD, please check you typed it correctly!"
		exit -1
	end

	filename = File.join(CONFIG['posts'], "#{date}-#{slug}.#{CONFIG['post_ext']}")
	if File.exists?(filename)
		abort('Rake task aborted.') if agree?("#{filename} already exists. Do you want to override it?") == false
	end

	puts "Creating a new post: #{filename}..."
	open(filename, 'w') do |post|
		post.puts "---"
		post.puts "layout: post"
		post.puts "title: \"#{title.gsub(/-/,' ')}\""
		post.puts 'description: ""'
	    post.puts "---"
	    post.puts ""
	    post.puts "Please edit this post!"
	end
	if File.size(filename) > 0
		puts "Successfully created post file. Go edit it now!"
	end
end

desc "Get the current version of Daktilo"
task :version do
	version_file_contents = YAML.load_file(CONFIG['version_file'])

	if version_file_contents['version']
		puts "Daktilo version #{version_file_contents['version']}."
	else
		puts "Could not find version file."
	end
end
