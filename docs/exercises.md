
## Forking and cloning the template

Go to [https://github.com/GeertvanGeest/course_website_template](https://github.com/GeertvanGeest/course_website_template), and click on **Use this template**:

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
├── LICENCE
├── README.md
├── docs
│   ├── assets
│   │   └── images
│   │       ├── SIB_LogoQ_GBv.svg
│   │       ├── SIB_logo.svg
│   │       ├── choose_owner.png
│   │       ├── clone_repo.png
│   │       ├── dna-svgrepo-com.svg
│   │       ├── reactions_zoom.png
│   │       ├── reply_in_thread.png
│   │       ├── use_this_as_template.png
│   │       └── zoom_icons.png
│   ├── course_schedule.md
│   ├── exercises.md
│   ├── index.md
│   ├── precourse.md
│   └── stylesheets
│       └── extra.css
└── mkdocs.yml
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
