---
title: Ruby Copy Files Recursively With an Exclusion Set
date: 2019-04-21 20:15:08
tags:
- ruby
- new
categories: 
- [ruby, file system]
---
### This is a Ruby file system tutorial.

``` ruby
require 'find'

# copy all files/dirs in ${source_path} to ${target_path}
# e.g. copy_without_files('src/dir', 'tgt/dir', Set.new(['a.rb', 'b.txt'])) will
# copy all files including directories to tgt/dir from src/dir
def copy_without_files(source_path, target_path, skipping_map)    
  Find.find(source_path) do |source|
    target = source.sub(/^#{source_path}/, target_path)
    if File.directory? source
      FileUtils.mkdir target unless File.exists? target
    else
      Find.prune if skipping_map.include?(File.basename(source))
      FileUtils.copy source, target
    end
  end
end

```
