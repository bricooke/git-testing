require 'readline'

namespace :git do
  namespace :production do 
    desc "Requires LAST_RELEASE_NAME: Recreate the production branch based on master."
    task :create do
      last_release_name = ENV["LAST_RELEASE_NAME"] ? ENV["LAST_RELEASE_NAME"].to_s : nil
      raise "LAST_RELEASE_NAME is required" unless last_release_name
      
      puts "WARNING"
      puts "This will rename the current production branch to '#{last_release_name}'"
      puts "and recreate the production branch from master"
      puts "WARNING"
      puts ""
      if Readline.readline("Are you sure this is what you want? (y/n)", true).downcase == "y"
        # create the tag
        %x(git checkout production)
        
        if $? != 0
          puts "'git checkout production' failed. Do you have a local branch of 'production'? you need that."
          exit
        end
        
        %x(git tag -a '#{last_release_name}' -m 'tagging production as #{last_release_name}')
        %x(git push --tags)
        
        puts "Tagged production as #{last_release_name}"
        
        # remove the old production bronch
        %x(git push origin :heads/production)        
        puts "Removed old production branch from origin"
        
        # recreate the production branch from master
        %x(git checkout master)
        %x(git branch -f production)
        %x(git push origin production)
        
        puts "Recreated production branch based off master"
      end
    end
  end
end
