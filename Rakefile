require 'rake'
 
desc "install the dot files into user's home directory"
task :install do
  puts "installing dot-files..."
  walk_files('files')

  puts "installing binaries..."
  walk_files('bin', File.join(ENV['HOME'], 'bin'))

  puts "done!"
end

def file_list(folder)
  Dir.entries(folder).reject { |d| /^\.+$/ =~ d }
end

def walk_files(folder, destination_root = ENV['HOME'])
  source_root = File.join(File.dirname(__FILE__), folder)
  files       = file_list(source_root)
  replace_all = false
  opt         = nil

  files.each do |file|
    file_name        = file.split(/\//).last
    source_file      = File.join(source_root, file)
    destination_file = File.join(destination_root, "#{file_name}")

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
  system %Q{ln -vfs "#{new_file}" "#{old_file}"}
end
