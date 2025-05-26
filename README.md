# linux-ipsec.org web page

This repository contains the IPsec and Network Security e.V. (_non-profit_) web page, [linux-ipsec.org](https://linux-ipsec.org).
For instructions about writing content and contributing, see the [Instructions](#instructions) section.
For more information on the technology used, see the [Technology](#technology) section.

## Structure
The project contains two main directories, [content](/content), which contains the conference articles and slides,
and [themes/custom/templates](/themes/custom/templates), which contains the HTML templates.
The page styles and the content of the front page can also be found in [templates](/themes/custom/templates).

The content directory has three subdirectories:
- [Conferences](/content/conferences) contains each conference article in Markdown format, with some metadata for generating the article page
- [Images](/content/images) for the logos and other possible image files
- [Slides](/content/slides) for the presentation slides for the conferences, with a subdirectory for each year's conreference.


## Instructions

### Contributing
To contribute to the project, fork this repository, and submit a pull request with your changes.
This works for both changes to the HTML templates and adding a new article, with possible attachments.

### Writing content
#### Creating a new article
To create a new conference article, simply create a new Markdown file to the `content/conferences/` directory.
The files are named `{year}-workshop-{location}.md` for nice sorting by year.

After creating the file, we can now start writing the article.
First, we need to add some metadata, mainly the title, date, and summary.
After the metadata, we can write the content of the article with the standard Markdown syntax.

The format of the file should look like this:
```markdown
Title: <article title goes here>
Date: <yyyy-mm-dd>
Summary: <a short summary that is shown in the article list>

Now the content of the article can be written here.
```

#### Adding conference slides
For a new conference, create a new directory to the `content/slides/` directory, for example 'content/slides/2025'.
Then, add the slides to this directory.
To create a link in the article to one of these slides we can use the Markdown link syntax:
```markdown
[link text here]({static}/slides/<year>/<filename>)
```

#### More information
To find more information about writing content, see the [Pelican documentation](https://docs.getpelican.com/en/latest/content.html).


### Local development server
To see how the changes look, the page can be generated and served locally by running the following commands:
```
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
make devserver
```

## Technology
The page is built using a static site generator called [Pelican](https://getpelican.com/).
[Tailwind CSS](https://tailwindcss.com/) is used for creating the CSS styles for the page.
Tailwind is used via [pytailwindcss](https://pypi.org/project/pytailwindcss/) for easy installation and use with the Python based Pelican.
Minimum tested python version is 3.9.
