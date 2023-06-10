#!/usr/bin/ruby -wKU

require 'rake/clean'

SAMPLES_DIR = Dir.pwd + "/samples"
SAMPLE_TAG  = SAMPLES_DIR + "/buildtag"

REPOS_DIR   = Dir.pwd + "/git_tutorial/repos"

CLOBBER.include("samples", "auto", "git_tutorial/repos")

desc "Clean the samples directory"
task :clean_samples do
  rm_r SAMPLES_DIR rescue nil
end

task :default => :labs

task :rebuild => [:clobber, :run, :labs]

task :see => [:rebuild, :view]

task :not_dirty do
  fail "Directory not clean" if /nothing to commit/ !~ `git status`
end

desc "Publish the Git Immersion web site."
task :publish => [:not_dirty, :build, :labs] do
  sh 'git checkout main'
  head = `git log --pretty="%h" -n1`.strip
  sh 'git checkout gh-pages'
  cp FileList['git_tutorial/html/*'], '.'
  sh 'git add .'
  sh "git commit -m 'Updated docs to #{head}'"
  sh 'git push'
  sh 'git checkout main'
end

directory "dist"

file "dist/git_tutorial.zip" => [:build, :labs, "dist"] do
  sh 'zip -r dist/git_tutorial.zip git_tutorial'
end

desc "Create the zipped tutorial"
task :package => [:not_dirty, "dist/git_tutorial.zip"]

desc "Create the zipped tutorial, but rebuild first"
task :repackage => [:clobber, :package]

desc "Upload the zipped tutorial to the download site."
task :upload => [:not_dirty, "dist/git_tutorial.zip"] do
  sh 'scp dist/git_tutorial.zip linode:htdocs/download/git_tutorial.zip'
end
