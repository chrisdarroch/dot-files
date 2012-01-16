require 'rake'
 
desc "install the dot files into user's home directory"
task :install do
  replace_all = false
  opt         = nil
  dot_files   = File.dirname(__FILE__)
  files       = Dir.entries('files').reject { |d| /^\.+$/ =~ d } # Exclude the . and .. directories
  
  files.each do |file|
    file_name        = file.split(/\//).last
    source_file      = File.join(dot_files, file)
    destination_file = File.join(ENV['HOME'], ".#{file_name}")

    if File.exist?(destination_file) || File.symlink?(destination_file)
      unless replace_all
        print "overwrite #{destination_file}? [ynaq] "
        opt = $stdin.gets.chomp.downcase
      end

      case opt
      when 'a'
        opt = 'y'
        replace_all = true
      when 'q'
        exit
      when 'n'
        puts "skipping #{destination_file}"
        next
      end
      replace_file(destination_file, source_file)
    else
      link_file(destination_file, source_file)
    end
  end
end
 
def replace_file(old_file, new_file)
  system %Q{rm "#{old_file}"}
  link_file(old_file, new_file)
end
 
def link_file(old_file, new_file)
  puts "#{old_file} => #{new_file}"
  system %Q{ln -fs "#{new_file}" "#{old_file}"}
end
