
## Forking and cloning the template

Go to [https://github.com/sib-swiss/course_website_template](https://github.com/sib-swiss/course_website_template), and click on **Use this template**:

<figure>
  <img src="../assets/images/use_this_as_template.png" width="500"/>
</figure>

Choose the namespace in which you want to use the website template, choose a name, and initiate the new repository by finalising with **Create repository from template**:

<figure>
  <img src="../assets/images/choose_owner.png" width="500"/>
</figure>

Now you can find the new repository at `https://github.com/[NAMESPACE]/[REPONAME]`. In order to clone the repository to a local directory, click on **Code** and copy the github address that you can use for cloning to your clipboard:

<figure>
  <img src="../assets/images/clone_repo.png" width="500"/>
</figure>

After that, you open a terminal (e.g. Windows Powershell or your favourite terminal) `cd` to a directory you want to clone your repository in (e.g. to `C:\Users\myname\Documents`) and type:

```sh
git clone https://github.com/[NAMESPACE]/[REPONAME].git # the last part can be pasted from github
```

## Serving a website locally

In order to work on your website, it is convenient if you can serve it locally and directly see the effects of your work. In order to do that use the terminal to go into the repository directory, (e.g. `C:\Users\myname\Documents\reponame`) and type:

```sh
mkdocs serve
```

Now type `http://localhost:8000` in your favourite browser, and your website should be visible.

Open the file `index.md` in your favourite text editor. Add some text to the page (e.g. `hello world!`) and save the file. See whether your changes are passed to the locally served website.

!!! note "Stopping `mkdocs serve`"
    After you have finished working on your website you will have to stop the serving process. Otherwise, it will continue in the background and keep port 8000 (and CPU) occupied. Stop the serving process with ++ctrl+"C"++ .

## The file structure

The total file structure of the template looks like this:

```
.
├── LICENCE
├── README.md
├── docs
│   ├── assets
│   │   └── images
│   │       ├── SIB_logo.svg
│   │       ├── reactions_zoom.png
│   │       ├── reply_in_thread.png
│   │       └── zoom_icons.png
│   ├── course_schedule.md
│   ├── exercises.md
│   ├── index.md
│   ├── precourse.md
│   └── stylesheets
│       └── extra.css
└── mkdocs.yml

4 directories, 12 files
```

The main directory contains:

* `LICENCE`: a licence file (in this case cc-by-4.0)
* `README.md`: the readme displayed at the github repository
* `docs`: a directory with all website content, including:
    * `assets`: a directory everything that is not directly rendered (e.g. images, pdfs)
    * files ending with `*.md`: the actual markdown files that are rendered into html
    * `stylesheets`: a directory with `.css` file(s) for defining the website style format
* `mkdocs.yml`: a YAML file that is used by `mkdocs` in which you specify:
    * Website structure
    * Meta information
    * Plugins


## Setting up the website infrastructure

Open `mkdocs.yml` in your favourite text editor. Have a look at the first part:

```yaml
site_name: Course template

nav:
    - Home: index.md
    - Precourse preparations: precourse.md
    - Course schedule: course_schedule.md
    - Exercises: exercises.md
```

The first line (`site_name`) let's you change website name. Change it to something that makes sense to you, and check whether it has changed in the locally hosted site.

With the part named `nav`, you can change the website structure and with that navigation. The file `index.md` should always be there, this is the 'homepage'.

Now we will generate a new page that is a subchapter of *Exercises*. In order to do so, follow the following steps:

- Generate a directory within the directory `docs` called `exercises`
- Within the `exercises` directory generate a new file called `exercises_day1.md`
- Adjust the `nav` part of `mkdocs.yml` like so:

```yaml
nav:
    - Home: index.md
    - Precourse preparations: precourse.md
    - Course schedule: course_schedule.md
    - Exercises:
      - Day 1: exercises/exercises_day1.md
```

Now a new collapsible menu will appear, containing your new page.

## Referring to the right repo

In `mkdocs.yml` have a look at the repository part:

```yaml
# Repository
repo_name: sib-swiss/course_website_template
repo_url: https://github.com/sib-swiss/course_website_template
```

The course website is now hosted at your own repository. Therefore, change the repository name and url according to your own.

## Markdown syntax

You can use general github markdown syntax in order to generate a formatted html page. Have a look [here](https://guides.github.com/features/mastering-markdown/).

Now, convert the rendered text below into markdown. Add your markdown text to the file `exercises_day1.md` and see whether you get the expected result while you type.

=== "Rendered markdown"
    ### My markdown exercise

    With plain markdown you can highlight in two ways:

    1. *Italic*
    2. **Bold**

    You can add a link to your favourite [website](https://www.sib.swiss/). Or add an image from that website (find it at `https://www.sib.swiss/images/banners/banner_research_infrastructure.jpg`):

    ![](https://www.sib.swiss/images/banners/banner_research_infrastructure.jpg)

    You can also add a local image (this one is stored in `../assets/images/zoom_icons.png`):

    ![](../assets/images/zoom_icons.png)

    Sharing a code is easy, inline you refer to code like this: `pip install mkdocs`. But often it's more convenient in a code block, e.g. with shell highlighting:

    ```sh
    FILE=my_genes.csv
    cat $FILE | cut -f 1,2 -d ','
    ```

    Or with R highlighting for example:

    ```r
    df <- read.csv('my_genes.csv')
    ```

=== "Answer"
    ````md
    ### My markdown exercise

    With plain markdown you can highlight in two ways:

    1. *Italic*
    2. **Bold**

    You can add a link to your favourite [website](https://www.sib.swiss/).
    Or add an image from that website (find it at `https://www.sib.swiss/images/banners/banner_research_infrastructure.jpg`):

    ![](https://www.sib.swiss/images/banners/banner_research_infrastructure.jpg)

    You can also add a local image (this one is stored in `../assets/images/zoom_icons.png`):

    ![](../assets/images/zoom_icons.png)

    Sharing a code is easy, inline you refer to code like this: `pip install mkdocs`.
    But often it's more convenient in a code block, e.g. with shell highlighting:

    ```sh
    FILE=my_genes.csv
    cat $FILE | cut -f 1,2 -d ','
    ```

    Or with R highlighting for example:

    ```r
    df <- read.csv('my_genes.csv')
    ```
    ````

## Additional features of Mkdocs material

Some additional features are very convenient for generating a website for teaching. For example [admonitions](https://squidfunk.github.io/mkdocs-material/reference/admonitions/):

=== "code"
    ```md
    !!! warning
        Do not overcommit the server!
    ```
=== "output"

    !!! warning
        Do not overcommit the server!

Also very convenient can be [content tabs](https://squidfunk.github.io/mkdocs-material/reference/content-tabs/):

*code:*

````md
=== "R"
    Generating a vector of integers:
    ```r
    a <- c(5,4,3,2,1)
    ```
=== "python"
    Generating a list of integers:
    ```python
    a = [5,4,3,2,1]
    ```
````

*output:*

=== "R"
    Generating a vector of integers:
    ```r
    a <- c(5,4,3,2,1)
    ```
=== "python"
    Generating a list of integers:
    ```python
    a = [5,4,3,2,1]
    ```

Mkdocs material comes with a very wide range of [emoticons and icons](https://squidfunk.github.io/mkdocs-material/reference/icons-emojis/). Use the search field in the link to search for icons. Here's an example:

=== "code"
    ```md
    Write an e-mail :material-send:, add a pdf :material-file-pdf: and wait :clock1:
    ```
=== "output"
    Write an e-mail :material-send:, add a pdf :material-file-pdf: and wait :clock1:

You can make a [button](https://squidfunk.github.io/mkdocs-material/reference/buttons/) like this:

=== "code"
    ```md
    [Download the presentation](../assets/pdf/sequencing_technologies.pdf){: .md-button }
    ```
=== "output"
    [Download the presentation](../assets/pdf/sequencing_technologies.pdf){: .md-button }

You can also add an icon to a button:

=== "code"
    ```md
    [:fontawesome-solid-file-pdf: Download the presentation](../assets/pdf/sequencing_technologies.pdf){: .md-button }
    ```
=== "output"
    [:fontawesome-solid-file-pdf: Download the presentation](../assets/pdf/sequencing_technologies.pdf){: .md-button }

Lastly, you can incorporate `html`. This can particularly be convenient if you want to control the size of images.

=== "code"
    ```html
    <figure>
      <img src="../assets/images/zoom_icons.png" width="300"/>
    </figure>

    <figure>
      <img src="../assets/images/zoom_icons.png" width="100"/>
    </figure>
    ```
=== "output"
    <figure>
      <img src="../assets/images/zoom_icons.png" width="300"/>
    </figure>

    <figure>
      <img src="../assets/images/zoom_icons.png" width="100"/>
    </figure>

## BYO workshop

If you have brought your own course material, now you can start with generating a page containing your own course material.

## Host the website at github.io

You can deploy your website as a github page by running the command:

```sh
mkdocs gh-deploy
```

It will become available at `[NAMESPACE].github.io/[REPONAME]`. This can take more than an hour if you are deploying for the first time. The next time you update your website, it will usually take less then a minute. 
