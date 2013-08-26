As an experiment, I wanted to create a CV covering my Rails experience, marked up using Haml and Sass. I used the [nanoc](http://nanoc.ws/) gem to generate the site and compile the templates.

Tutorial
========

Here's how I would create this if starting from scratch:

1. Installed nanoc
-------------------------

I already have ruby and rubygems installed, so I just need to install the nanoc gem:

```ruby    
gem install nanoc
```

2. Created the nanoc site
-------------------------

Following the instructions at http://nanoc.ws/docs/:

    % nanoc create_site dgms_cv

3. Initialised a git repo
-------------------------
    
    % cd dgms_cv
    % git init
    % git add .
    % git commit -m "Initial commit" 

4. Added a gitignore file
-------------------------

When we compile the site it will create some output files - we don't want to include these in the app:

```ruby    
#.gitignore
output/
tmp/
````

5. Set nanoc up to use haml
-------------------------

Adding nanoc into the gemfile is probably not strictly necessary, but gives me a warm, fuzzy feeling

```ruby    
#Gemfile
source 'http://rubygems.org'

gem 'nanoc'
gem 'haml'
```

```ruby    
#Rules
...

compile '*' do
  if item.binary?
    # don’t filter binary items
  else
    filter :haml
    layout 'default'
  end
end

...

layout '*', :haml
```
    % bundle
    

6. Add your content
-------------------------

I decided to use the [hResume](http://microformats.org/wiki/hresume) microformat for extra semantic goodness

What I actually Did
====================

Of course, things rarely work perfectly first time.
Here's what I actually did, recorded for my own reference, and for interest.

1. Created a directory and initialised a git repo

2. Added a gemfile and bundled to install the gem

3. Cool - the docs are pretty good for getting started <http://nanoc.ws/docs/>

3. Create the nanoc site:
    
    `% nanoc create_site dgms_cv`

4. Oh, wait, that created a directory called dgms_cv. I'd Better start again with `nanoc create_site` as the first step

5. Ok, that's better. Did git init and the initial commit made more sense.

6. Added the Gemfile again - maybe premature something...

7. Compiled the site (just run `nanoc`) - cool! It created a dummy index file and a css file - I can load the site!

8. Ok, that created some files - I probably need to gitignore them. <https://github.com/github/gitignore/blob/master/nanoc.gitignore> confirms this - should be just the output and tmp directories

9. Tried adding some haml to the index file - needed to install the sublime package manager first: <https://sublime.wbond.net/installation#st2>

10. Hmm... that didn't work - the text just displays inline...

11. Ah cool - needed to change the filter in the rules file from erb to haml

12. Still doesn't work - looks like it needs the haml gem??

13. Added haml to gemfile and bundled

14. Still not working - maybe I need to make the layout haml as well. Got the haml version of the default here: <http://www.owengriffin.com/posts/2011/03/23/Go_Nanoc3_Go-Using_Haml_templates.html>

15. Cool -working now. There are 2 filters in the rules file: one for layouts, the other for content - I was setting one but not the other.

16. Put some initial content in as Haml. Nice and easy.

17. In the docs it mentions unit tests. Ah crap - should I have been TDDing this? No, it looks OK - it's checks for valid markup and broken links. Let's try it.

18. Right, the checks need some gems: W3C validators and Nokogiri for checking the links. Add them to the gemfile and bundle.

19. Need to add some more content. Nice and semantic html5. 

20. Hmm... maybe I should think about microformats. Better to do that as I go along, rather than retrofitting? Oh cool: <http://microformats.org/wiki/hresume> exists. <http://veerasundar.com/resume/> is a nice example of its use.

21. Lets run the checks and see what they throw up. Silly html validation error (using "<<Summary goes here>>" obviously fails). Ah - seems like the validations are run on the compiled files. I guess that's obvious.
  
22. Fully following the hresume microformat seems like overkill - in particular marking up job titles as hcards with links to my name defined in the contact section is just nasty: http://microformats.org/wiki/hresume#Job_Titles

23. Err... what? Microformat2 is a thing now? Not convinced anything uses it... Doubling up on tags (e.g. name p-name) will make the haml even less readable... I think I'm going to ignore it for now. <http://microformats.org/wiki/microformats-2>

24. Finding http://diveintohtml5.info/semantics.html useful for a reminder about html5 semantics.

25. Running "nanoc" to compile each time is annoying - I think I saw an autocompile option... wtf? nanoc autcompile is deprecated? The docs don't mention that. Deprecation message suggests using nanoc-guard. Super-easy.

26. Ok, now I've added this skill list I'm itching to start styling things... Could use bootstrap, but I think I should probably fully hand-craft the CSS. Let's move this CSS file to scss in any case.

27. Oops - missed a couple of steps to set up scss. Firstly need to tell the rules file to use scss:

    compile '/stylesheet/' do
      filter :sass, syntax: :scss
    end

28. Also need to gitignore the sass cache. Cool - you can do it from command line: 

    echo .sass-cache >> .gitignore






