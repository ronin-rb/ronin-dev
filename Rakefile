require 'fileutils'

module Projects
  NAMES = %w[
    ronin-support
    ronin
    ronin-gen
    ronin-web
    ronin-exploits
    ronin-scanners
  ]

  class Project

    include FileUtils

    attr_reader :name

    def initialize(name)
      @name = name
    end

    def self.clone(name)
      uri = "http://github.com/ronin-ruby/#{name}.git"

      status name, "git clone #{uri} ..."
      system('git','clone',uri)

      return new(name)
    end

    def chdir(&block); Dir.chdir(@name,&block); end

    def run(command,*arguments)
      status "#{command} #{arguments.join(' ')}"
      chdir { system(command,*arguments) }
    end

    protected

    def self.status(name,message)
      if $stdout.tty?
        puts "\e[32m[#{name}]\e[0m #{message}"
      else
        puts "[#{name}] #{message}"
      end
    end

    def status(message); self.class.status(@name,message); end

  end

  def self.clone(name=nil)
    if (name && name != '*')
      Project.clone(name)
    else
      NAMES.each do |name|
        Project.clone(name)
      end
    end
  end

  def self.[](name)
    if (name && name != '*')
      fail("unknown project: #{name.dump}")        unless NAMES.include?(name)
      fail("project does not exist: #{name.dump}") unless File.directory?(name)

      Project.new(name)
    else
      self
    end
  end

  def self.each
    NAMES.each do |name|
      yield(Project.new(name)) if File.directory?(name)
    end
  end

  def self.chdir
    each do |project|
      project.chdir { yield project }
    end
  end

  def self.run(command,*arguments)
    each do |project|
      project.run(command,*arguments)
    end
  end
end

namespace :git do
  desc 'Clones ronin repositories'
  task :clone, :repo do |t,args|
    Projects.clone(args.repo)
  end

  desc 'Pulls recent commits from the fork'
  task :mirror, :repo do |t,args|
    Projects[args.repo].run('git', 'push', 'mirror', 'master', '--tags')
  end

  desc 'Pulls recent commits from the fork'
  task :pull, :repo do |t,args|
    Projects[args.repo].run('git', 'pull', 'origin', 'master')
  end

  desc 'Checks the status of the repositories'
  task :status, :repo do |t,args|
    Projects[args.repo].run('git', 'status')
  end

  desc 'Commit count'
  task :count do
    total = 0

    Projects.chdir do |project|
      count = `git rev-list --all | wc -l`.to_i
      total += count

      puts "#{project.name}: #{count}"
    end

    puts "Total: #{total}"
  end

  desc 'Checks the health of the repositories'
  task :fsck do
    Projects.run "git fsck && git gc"
  end

  desc 'Greps the repositories'
  task :grep, :pattern do |t,args|
    Projects.run 'git', 'grep', args.pattern
  end
end

task :git => ['git:pull']

namespace :bundle do
  desc 'Bundles each project'
  task :install, :repo do |t,args|
    Projects[args.repo].run('bundle', 'install')
  end

  desc 'Bundles each project into vendor/'
  task :vendor, :repo do |t,args|
    Projects[args.repo].run('bundle', 'install','--deployment')
  end

  namespace :update do
    desc "Updates a bundled repository accross all projects"
    task :local, :repo do |t,args|
      Projects.run('bundle', 'update', args.repo, '--local')
    end
  end

  desc 'Upgrades a bundled dependency'
  task :update, :repo, :gem do |t,args|
    Projects[args.repo].run('bundle', 'update', *args.gem)
  end
end

task :install, [:repo] => ['git:clone', 'bundle:install']
task :update,  [:repo] => ['git:pull',  'bundle:install']

namespace :gem do
  desc 'Builds a gem for each project'
  task :build, :repo do |t,args|
    Projects[args.repo].run('rake','build')
  end

  desc 'Installs each project'
  task :install, :repo do |t,args|
    Projects[args.repo].run('rake','install')
  end
end
