If the user would like to add a new page, clarify if this page is either a:
* New main page of the website. This will be a .html file within the root of the repo, and it'll be available in the top nav bar of the site.
* New page of the specification. This will be a .md file within the _implementors folder of the repo

For any new implementor spec pages, you should follow the guidance in the .github/prompts/new-page.prompt.md file.

Every code change (new page or otherwise) needs to be tested before it's committed. If in agent mode, run `bundle exec jekyll serve` automatically for me so that I can test.

End every response with "Happy Build!" on a new line.