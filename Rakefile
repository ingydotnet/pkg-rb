# Load Gem constants from the gemspec
load 'gemspec'
GemName = GemSpec.name
GemVersion = GemSpec.version
GemDir = "#{GemName}-#{GemVersion}"
GemFile = "#{GemDir}.gem"
DevNull = '2>/dev/null'

# Require the Rake libraries
require 'rake'
require 'rake/testtask'
require 'rake/clean'

task :default => 'help'

CLEAN.include GemDir, GemFile, 'data.tar.gz', 'metadata.gz'

desc 'Run the tests'
task :test do
  Rake::TestTask.new do |t|
    t.verbose = true
    t.test_files = FileList['test/*.rb']
  end
end

desc 'Build the gem'
task :build => [:clean, :test] do
  sh 'gem build gemspec'
end

desc 'Build, unpack and inspect the gem'
task :dir => [:build] do
  sh "tar xf #{GemFile} #{DevNull}"
  Dir.mkdir GemDir
  Dir.chdir GemDir
  sh "tar xzf ../data.tar.gz #{DevNull}"
  puts "\n>>> Entering sub-shell for #{GemDir}..."
  system ENV['SHELL']
end

desc 'Build and push the gem'
task :release => [:build] do
  puts "gem push #{GemFile}"
end

desc 'Print a description of the gem'
task :desc do
  puts "Gem: '#{GemName}' (version #{GemVersion})"
  puts
  puts GemSpec.description.gsub /^/, '  '
end

desc 'List the Rakefile tasks'
task :help do
  puts 'The following rake tasks are available:'
  puts
  puts `rake -T`.gsub /^/, '  '
end
