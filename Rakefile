require "jekyll"
require "tmpdir"

GITHUB_REPONAME = "agileaddicts/agileaddicts.com"
GITHUB_REMOTE = "https://#{ENV['GH_TOKEN']}@github.com/#{GITHUB_REPONAME}"

desc "Generate page files"
task :generate do
    Jekyll::Site.new(Jekyll.configuration({
        "source"      => ".",
        "destination" => "_site"
    })).process
end

desc "Serve page from localhost"
task :serve do
    system "jekyll serve --watch --force_polling"
end

desc "Generate and publish page to gh-pages"
task :publish => [:generate] do
    fail "Not on Travis" if "#{ENV['TRAVIS']}" != "true"

    Dir.mktmpdir do |tmp|
        cp_r "_site/.", tmp

        pwd = Dir.pwd
        Dir.chdir tmp

        system "git init"
        system "git config user.name 'Sebastian Göttschkes'"
        system "git config user.email 'sebastian@agileaddicts.com'"

        system "git add ."
        message = "Site updated at #{Time.now.utc}"
        system "git commit -m #{message.inspect}"
        system "git remote add origin #{GITHUB_REMOTE}"
        system "git push --force origin master:gh-pages"

        Dir.chdir pwd
    end
end
