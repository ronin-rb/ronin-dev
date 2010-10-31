module Ronin
  PROJECTS = %w[
    ronin-support
    ronin
    ronin-gen
    ronin-exploits
    ronin-web
    ronin-asm
    ronin-scanners
    ronin-dorks
    ronin-sql
    ronin-php
    ronin-metasploit
    ronin-wiki
  ]

  def self.included(base)
    base.send :include, Thor::Actions
  end

  protected

  def each_project
    PROJECTS.each do |name|
      if File.directory?(name)
        inside(name) { yield name }
      end
    end
  end

  def enter_projects
    each_project do |name|
      say ">>> Entering #{name} ...", :green

      yield name

      say ">>> Leaving #{name}.", :green
    end
  end

  def remote
    if options.fork?
      'fork'
    elsif options.mirror?
      'mirror'
    else
      'origin'
    end
  end

  def run_in_all(command,*arguments)
    enter_projects { |name| system(command,*arguments) }
  end
end

class Git < Thor

  include Ronin

  desc 'clone [REPOS]', 'Clones ronin repositories'
  def clone(*repos)
    repos = PROJECTS if repos.empty?

    repos.each do |name| 
      uri = "http://github.com/ronin-ruby/#{name}.git"

      say "Cloning #{uri}", :green
      system 'git', 'clone', uri
    end
  end

  desc 'status', 'Checks the status of the repositories'
  def status
    run_in_all 'git', 'status'
  end

  desc 'count', 'Commit count'
  def count
    total = 0

    each_project do |name|
      count = `git rev-list --all | wc -l`.to_i
      total += count

      puts "#{name}: #{count}"
    end

    puts "Total: #{total}"
  end

  desc 'fsck', 'Checks the health of the repositories'
  def fsck
    run_in_all 'git', 'fsck'
  end

  desc 'gc', 'Repacks the repositories'
  def gc
    invoke :fsck

    run_in_all 'git', 'gc'
  end

  desc 'grep PATTERN', 'Greps the repositories'
  def grep(pattern)
    run_in_all 'git', 'grep', pattern
  end

  desc 'branches', 'Lists branches in all repositories'
  def branches
    run_in_all 'git', 'branch'
  end

  desc 'checkout NAME', 'Switches branches in all repositories'
  method_option :create, :type => :boolean, :aliases => '-b'
  def checkout(name)
    git_options = []
    git_options << '-b' if options.create?

    run_in_all('git','checkout',*git_options,name)
  end

  desc 'commit FILES', 'Commits changes in all repositories'
  method_option :message, :type => :string, :aliases => '-m'
  method_option :all, :type => :boolean, :aliases => '-a'
  method_option :amend, :type => :boolean
  def commit(*files)
    git_options = []

    git_options += files

    git_options << '-a' if options[:all]
    git_options << '--amend' if options[:amend]

    if options[:message]
      git_options += ['-m', options[:message].dump]
    end

    run_in_all('git','commit',*git_options)
  end

  desc 'pull', 'Pulls recent commits from the fork'
  method_option :origin, :type => :boolean
  method_option :mirror, :type => :boolean
  method_option :fork, :type => :boolean
  def pull
    run_in_all 'git', 'pull', remote, 'master'
  end

  desc 'push', 'Pushes recent commits from the fork'
  method_option :origin, :type => :boolean
  method_option :mirror, :type => :boolean
  method_option :fork, :type => :boolean
  def push
    run_in_all 'git', 'push', remote, 'master'
  end

end

class Bundle < Thor

  include Ronin

  default_task :install

  desc 'install', 'Bundles each project'
  def install
    run_in_all 'bundle', 'install'
  end

  desc 'update', 'Upgrades a bundled dependency'
  def update(*deps)
    run_in_all('bundle','update',*deps)
  end

end
