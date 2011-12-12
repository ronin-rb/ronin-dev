PROJECTS = %w[
  ronin-support
  ronin
  ronin-gen
  ronin-web
  ronin-exploits
  ronin-scanners
]

def uri(name)
  "http://github.com/ronin-ruby/#{name}.git"
end

def each_project(*names)
  names = PROJECTS if names.empty?

  names.each do |name|
    if File.directory?(name)
      Dir.chdir(name) { yield name }
    end
  end
end

def enter_projects(*names)
  each_project(*names) do |project|
    puts ">>> Entering #{project} ..."

    yield name

    puts ">>> Leaving #{project}."
  end
end

def run_in_all(command,*arguments)
  enter_projects { |name| system(command,*arguments) }
end

namespace :git do
  desc 'Clones ronin repositories'
  task :clone, :repo do |t,args|
    repos = if args.repo
              [args.repo]
            else
              PROJECTS
            end

    repos.each do |name| 
      puts "Cloning #{name} ..."
      system 'git', 'clone', uri(name)
    end
  end

  desc 'Pulls recent commits from the fork'
  task :mirror, :repo do |t,args|
    args.with_defaults(:repo => PROJECTS)

    enter_projects(args.repos) do |name|
      system 'git', 'push', 'mirror', 'master', '--tags'
    end
  end

  desc 'Pulls recent commits from the fork'
  task :pull do
    run_in_all 'git', 'pull', 'origin', 'master'
  end

  desc 'Checks the status of the repositories'
  task :status do
    run_in_all 'git', 'status'
  end

  desc 'Commit count'
  task :count do
    total = 0

    each_project do |name|
      count = `git rev-list --all | wc -l`.to_i
      total += count

      puts "#{name}: #{count}"
    end

    puts "Total: #{total}"
  end

  desc 'Checks the health of the repositories'
  task :fsck do
    enter_projects do |name|
      system 'git', 'fsck'
      system 'git', 'gc'
    end
  end

  desc 'Greps the repositories'
  task :grep, :pattern do |t,args|
    run_in_all 'git', 'grep', args.pattern
  end
end

task :git => ['git:pull']

namespace :bundle do
  desc 'Bundles each project'
  task :install, :repo do |t,args|
    args.with_defaults(:repo => PROJECTS)

    enter_projects(args.repo) do |name|
      system 'bundle', 'install'
    end
  end

  desc 'Upgrades a bundled dependency'
  task :update, :gem do |t,args|
    run_in_all 'bundle', 'update', args.gem
  end
end

task :install, [:repo] => ['git:clone', 'bundle:install']
task :update,  [:repo] => ['git:pull',  'bundle:install']
