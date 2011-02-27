def jekyll(opts = nil)
  sh "rm -rf _site"
  jekyll = Dir["_jekyll/bin/jekyll"].first || "jekyll"
  sh [jekyll, opts].join(" ")
  cp "_htaccess", "_site/.htaccess"
end

desc "Start in --auto preview mode"
task :auto do
  require 'pathname'
  require 'yaml'
  require 'uri'

  config = YAML.load_file(Pathname.new(__FILE__).dirname.join('_config.yml'))
  uri = URI.parse(config["url"])
  uri.host.gsub!(/^www\./)
  uri.host = 'preview.' + uri.host

  jekyll ["--auto", "--url", uri.to_s]
end

desc "Start in --auto --server mode"
task :server do
  jekyll "--auto --server"
end

desc "Run a build of jekyll "
task :build do
  jekyll
end

task :default => :auto
task :deploy => "deploy:dreamhost"

namespace :deploy do
  desc "Deploy to Dreamhost"
  task :dreamhost => :build do
    rsync "penguin.dreamhost.com:~/sites/railslessons.com"
  end

  def rsync(destination)
    sh "rsync -rtz --delete _site/ #{destination}"
  end
end
