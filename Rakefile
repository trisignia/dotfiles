desc "installs everything"
task :install => "install:all"
namespace :install do

  def install name, *files
    desc "installs #{name} configuration"
    task(name) do
      Dir[*files].collect do |file|
        full = File.join File.dirname(__FILE__), file
        Dir.chdir ENV["HOME"] do
          mkdir_p File.dirname(file)
          File.delete(file) if (File.exist? file and File.directory? full)
          sh "ln -sf #{full} #{file}"
        end
      end
    end
    task :all => name
  end

  install :irb, ".irbrc", ".config/irb/*.rb"
  install :dot, *%w(.bash_profile .bashrc .gemrc .global_gitignore .gitconfig .ackrc)
  install :bin, "bin/*"

  desc "Update all submodules"
  task :submodules do
    system "git submodule init && git submodule update"
  end

  desc "installs the custom texmf folder"
  task :texmf => :submodules do
    install :texmf_folder, "texmf"
    Rake::Task[:texmf_folder].invoke
  end

  desc "Update the janus bundle"
  task :vim => :submodules do
    install :vim, *%w(.vim .vimrc.local .janus.rake)

    Dir.chdir File.join(File.dirname(__FILE__), ".vim") do
      system "rake"
    end
  end

  task :all => [:texmf, :vim]
end

