require 'pathname'
require 'rubygems'
require 'rake'
require 'rake/clean'

ROOT = Pathname(__FILE__).dirname.expand_path

require ROOT + 'lib/do_sqlite3/version'

JRUBY    = RUBY_PLATFORM =~ /java/
IRONRUBY = defined?(RUBY_ENGINE) && RUBY_ENGINE == 'ironruby'
WINDOWS  = Gem.win_platform? || (JRUBY && ENV_JAVA['os.name'] =~ /windows/i)
SUDO     = WINDOWS ? '' : ('sudo' unless ENV['SUDOLESS'])
BINARY_VERSION = '3_6_21'

CLEAN.include(%w[ {tmp,pkg}/ **/*.{o,so,bundle,jar,log,a,gem,dSYM,obj,pdb,exp,DS_Store,rbc,db} ext/do_sqlite3/Makefile ext-java/target ])

begin
  gem 'jeweler', '~> 1.4'
  require 'jeweler'

  Jeweler::Tasks.new do |gem|
    gem.name        = 'do_sqlite3'
    gem.version     = DataObjects::Sqlite3::VERSION
    gem.summary     = 'DataObjects Sqlite3 Driver'
    gem.description = 'Implements the DataObjects API for Sqlite3'
    gem.platform    = Gem::Platform::RUBY
    gem.files       = FileList['lib/**/*.rb', 'spec/**/*.rb', 'tasks/**/*.rake',
                               'ext/**/*.{rb,c,h}', 'LICENSE', 'Rakefile',
                               '*.{markdown,rdoc,txt,yml}']
    gem.extra_rdoc_files = FileList['README*', 'ChangeLog*', 'LICENSE']
    gem.test_files  = FileList['spec/**/*.rb']

    # rake-compiler should generate gemspecs for other platforms (e.g. 'java')
    # and modify dependencies and extensions appropriately
    gem.extensions << 'ext/do_sqlite3/extconf.rb'

    gem.add_dependency 'data_objects', DataObjects::Sqlite3::VERSION

    gem.add_development_dependency 'bacon',         '~>1.1'
    gem.add_development_dependency 'rake-compiler', '~>0.7'

    gem.has_rdoc = false
    gem.rubyforge_project = 'dorb'
    gem.authors     = [ 'Dirkjan Bussink' ]
    gem.email       = 'd.bussink@gmail.com'
  end

  if JRUBY
    Rake::Task['build'].clear_actions   if Rake::Task.task_defined?('build')
    Rake::Task['install'].clear_actions if Rake::Task.task_defined?('install')
    task :build => [ :java, :gem ]
    task :install do
      sh "#{Config::CONFIG['RUBY_INSTALL_NAME']} -S gem install pkg/do_sqlite3-#{DataObjects::Sqlite3::VERSION}-java.gem"
    end
  end

  Jeweler::GemcutterTasks.new

  FileList['tasks/**/*.rake'].each { |task| import task }
rescue LoadError
  puts 'Jeweler (or a dependency) not available. Install it with: gem install jeweler'
end
