<variable name="title">Design</variable>
<frontmatter>
  title: "{{ title }}"
  layout: devGuide
  pageNav: default
</frontmatter>

# {{ title }}

<div class="lead">

This page gives you an overview of the MarkBind's internal design.
</div>

## Project structure

The MarkBind project is developed in a monorepo ([MarkBind/markbind](https://github.com/MarkBind/markbind)) of 3 packages:

* The command-line interface (CLI) application, which accepts commands from users and then uses the core library to parse and generate web pages, resides in the root.

* The core library, which parses and processes MarkBind's various syntaxes, resides in the `packages/core/` directory.

* The UI components library, which MarkBind authors can use to create content with complex and interactive structure, resides in the `packages/vue-components/` directory.

  Stacks used: *Node.js*, *Vue.js*

### MarkBind core library

**The core library mainly houses:**
1. Various key functionalities in processing a _singular_ page inside `src/Parser` that transforms MarkBind syntax into valid html output.

2. The `src/Page` model, which binds the functionalities exposed by `Parser` to generate a _single_ page.

3. The `src/Site` model, which uses the `Page` model's interface to generate pages, and performs various other utility-like functions related to site generation such as copying of external assets into the output folder.

4. Static assets of MarkBind, such as stylesheets and JavaScript libraries, which are located in `asset/` folder. They will be copied to the generated site and used in the generated pages.

5. MarkBind's [templates](https://markbind.org/userGuide/templates.html), used in the `markbind init` command.

**The key external libraries used are:**
1. [markdown-it](https://github.com/markdown-it/markdown-it), which does the Markdown parsing and rendering. There are also several customized markdown-it plugins used in MarkBind, which are located inside the `src/lib/markdown-it/` directory.

2. [htmlparser2](https://github.com/fb55/htmlparser2), a speedy and forgiving html parser which exposes a dom-like object structure to work on. To comply with the markdown spec, and our custom requirements, `src/patches/htmlparser2.js` patches various behaviours of this library.

3. [cheerio](https://cheerio.js.org/), which is a node.js equivalent of [jQuery](https://jquery.com/). Cheerio uses [htmlparser2](https://github.com/fb55/htmlparser2) to parse the html as well, hence our patches propagate here.


### MarkBind CLI

The CLI application uses and further builds on the interface exposed by the core library's `Site` model to provide functionalities for the author, such as `markbind serve` which initiates a live reload workflow.

The CLI program is built using [commander.js](https://github.com/tj/commander.js/).

### UI components library

This package consists of a mix of [Bootstrap](getbootstrap.com/components/) and proprietary components rewritten in [Vue.js](vuejs.org) based on our needs for educational websites.

We forked it from the original [yuche/vue-strap](https://github.com/yuche/vue-strap) repo into the [MarkBind/vue-strap](https://github.com/MarkBind/vue-strap) repo, and then later merged it into the main [MarkBind/markbind](https://github.com/MarkBind/markbind) repo.
