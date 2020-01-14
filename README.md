## Quick Start

1. Set up ruby environment. First install ruby using brew:
   ```
   $ brew install ruby
   ```

   Install [bundler](http://bundler.io/)
   ```
   $ gem install bundler
   ```

   Add following line in ~/.gemrc
   ```
   gem: --user-install
   ```

   Check your ruby environment
   ```
   $ gem env
   ```
   It will show you gem path. Ensure that ge path shown there are in your PATH.

   If not, update your PATH in .bashrc
   ```
   export PATH="/usr/local/lib/ruby/gems/2.5.0:${PATH}"
   ```

2. Change directory to where you have cloned the repo.
   ```
   $ cd <github-pages-repo ...>
   $ bundle install
   ```
   This will install [Jekyll](https://jekyllrb.com/) and any other Ruby gems/plugins needed.

4. Build the site, it generates output in _site directory
   ```
   $ jekyll build
   $ ls -l <...>/_site
   $ cat <...>/_site/index.html
   ```

   To build production version:
   ```
   $ JEKYLL_ENV=production jekyll build
   ```

5. Start Jekyll Server
   ```
   $ jekyll serve
   ```

6. See website at [localhost:4000](http://localhost:4000/)


## Markdown

Most of the content is in [Markdown](https://en.wikipedia.org/wiki/Markdown) language, and here is an helpful [Cheetsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet). There are several online markdown editors, for example [JBT](https://jbt.github.io/markdown-editor/), [StackEdit](https://stackedit.io/editor), and [Dillinger](http://dillinger.io/).

Syntax highlighting stylesheet was generated as following:

```
$ rougify style github > assets/common/css/syntax-github.css
```

## Blog posts Directories

- `blog/_posts/` : English blog posts
- `blog/_posts/trekking` : Trekking blog posts
- `tech/_posts/` : Posts on programming, technology etc.
- `hindi/_posts/blog` : हिन्दी ब्लॉग
- `hindi/_posts/manpraan` : मन-प्राण
- `hindi/_posts/madhyaahan` : मध्याहन
- `hindi/_posts/besabab` : बेसबब
- `hindi/_posts/brihannala` : बृहन्नला
- `hindi/_posts/other` : अन्य
