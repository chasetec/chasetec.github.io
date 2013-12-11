---
layout: post
title:  "Starting a GitHub Blog"
categories: github jekyll
---
Like a lot of technically inclined people I have my own VPS and I've setup CMS/blogging platforms like Wordpress on several occasions but for my own blog I want simple (over the long run) and little to no maintenance. It turns out that GitHub will host *static* web pages for their users for free. GitHub hosted websites are referred to as [GitHub Pages][github-pages].

GitHub pages come in two varieties:

* Project pages: These are for creating project documentation. In the existing repo for a project create a `gh-pages` branch. Project Pages are served from USERNAME.github.io/PROJECTNAME.
* User/Organization pages: These are for creating pages about you or your organization. This is the type of pages I'll be using for a blog. In your GitHub account create a repo named USERNAME.github.io where USERNAME is your username. The master branch in this repo will be contain your pages.


I'm looking to make a set of user pages so step one is to go to <https://github.com/new> and create a repo named `chasetec.github.io` (replace *chasetec* with your username).


There is a web-based automatic page generator you can find in the repository's settings page. This will let you use GitHub's web-based markdown editor, pick one of several provided themes, and publish your pages with little fuss. For more about GitHub Pages see <http://pages.github.com/>. I used the page generator at first so I could see the structure of the repo. Be aware that the first time you publish your Pages repo (whether you use the generator or not) it will take 10 minutes before the your pages go live.

I don't want to use the page generator to maintain my blog so I'll need to switch to more traditional repository management. 

	git clone https://github.com/chasetec/chasetec.github.io.git
	cd chasetec.github.io/

One of the cool things about GitHub Pages is that they support [Markdown][markdown] along with some [GitHub extensions][github-markdown]. Create a `test.markdown` or `test.md` file in you Pages repo and it'll be converted to `test.html` by GitHub. The content of test.markdown would be something like:

    ---
    title: My Markdown test page
    ---
    
    # This is a H1
    
    [My GitHub Person Pages](http://chasetec.github.io/)
    
    Things to do:
    * Get my blog going
    * Get Jekyll working

Stage, commit, and push the file:

    git add test.markdown
    git commit -m "adding markdown test"
    git push origin master

The resulting page would be located at http://chasetec.github.io/test.html.

There is one big limitation with GitHub Pages that you should be aware of. They aren't dynamic in the traditional sense. You can't have pages with code in them that executes at request time on the server-side. That means no PHP, JSP, or ASPX type files. To overcome this limitation you can use Jekyll. Jekyll is a blog-aware site generator that GitHub supports. When using Jekyll you create your site as a collection of files which can use various markup languages and templating statements and then run the files through Jekyll which generates all of your HTML. GitHub runs your GitHub Pages through Jekyll when you push your files.

For testing, experimentation, and debugging you will probably want to have a local install of Jekyll although it isn't technically required.

### Setting up Jekyll

Install Ruby 1.9.3 and the Development Kit from <https://www.ruby-lang.org/en/downloads/>. Note: If running in Windows or Cygwin add .bat to the gem and bundle commands that most Jekyll tutorials show. I also had to make sure that Python was not installed in Cygwin because it wasn't working with some of the Ruby gems.
Install Bundler (Ruby package manager)

    gem.bat install bundler

Install Python 2.7.6 from <http://www.python.org/download/>. You'll need this for the syntax highlighting features of Jeykll provided by pygments.rb. The 3.X versions might cause issues, make sure the 2.7.X version is in your path first.

In your repo create a file named `Gemfile` with the content:

    source 'https://rubygems.org'
    gem 'pygments.rb', '0.5.0'
    gem 'github-pages'

Normally you wouldn't need the pygments.rb line because it is a dependency that gets dragged in but for me the current version (0.5.4) doesn't work on Win7 and I had to switch back to 0.5.0.

Install Jekyll.

    bundle.bat install

Create a basic Jekyll site.

    bundle.bat exec jekyll new . --force

Edit the generated `.gitingnore` file and add `Gemfile.lock` to the list of ignored files.

To start Jekyll and test your site locally run:

    bundle.bat exec jekyll serve

Note that for me ctrl-c to stop Jekyll was not working in cygwin and I had to use taskmgr to kill the ruby process. Running from cmd.exe doesn't have the same issue. If you get an error the second time you run Jekyll that says `error: Only one usage of each socket address (protocol/network address/port) is normally permitted.` then you might have the same problem.

Now you should be able to browse to <http://localhost:4000/> and see your new site.

The first step to editing your new blog is to modify the site name within the `_config.yml` file. After restarting Jekyll you should see the updated title on the index page.

Step two is to create a blog posting. Start by creating a `_drafts` folder and moving the sample blog post that was created with the site.

	mv _posts/2013-12-11-welcome-to-jekyll.markdown _drafts/template_post.md

Drafts are not published by default and I keep a starting blog template in the `_drafts` folder. Posts in the `_posts` directory should be named YEAR-MONTH-DAY-TITLE.MARKUPLANGUAGE and should start with a [YAML front-matter][front-matter] section like so:

```yaml
---
layout: post
title:  "Starting a GitHub Blog"
categories: github jekyll
---
```

You can view the raw text for this [blog posting](https://github.com/chasetec/chasetec.github.io/blob/master/_posts/2013-12-11-starting-a-github-blog.md) if you'd like to see how I've used Markdown.

The last step is to edit `_layouts/default.html` and update the information in the footer div. Test one last time and if everything looks good then push your blog to your Pages repo.

[github-pages]: http://pages.github.com/
[markdown]: http://daringfireball.net/projects/markdown/syntax
[github-markdown]: https://help.github.com/articles/github-flavored-markdown
[front-matter]: http://jekyllrb.com/docs/frontmatter/
