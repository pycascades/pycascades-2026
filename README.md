# pycascades-2026
The static website for 2026.pycascades.com built with Lektor

# Setup
1. Clone this repo somewhere on your machine
2. Install Python, [pipx](https://pipx.pypa.io/latest/installation/), and [Just](https://just.systems/man/en/)
3. Install Lektor: `just install`
    - Lektor uses `imagemagick`, you'll need to install this as well
4. Start the dev server with `just up`
    - This starts a web server at `http://127.0.0.1:5000/`, where you can preview the site and use
    the admin panel to add new pages or other content.

# Adding Content
Adding content in Lektor is relatively easy once you know how to do it. There are two main ways to
add content to the Lektor site:

- Via the localhost web server (accessed by running `lektor serve` and navigating to
    `http://127.0.0.1:5000/`)
- Adding a folder within the `content` folder, and putting a `contents.lr` file in it
    -  A `contents.lr` file is a key-value file, where each key and value is separated by `---`,
    which allows values to be multiline.

Each page on the static site has an associated model, the default model type is the `page` model,
defined in the `models/page.ini` file. You can change the model for each page by setting the
`_model` key in your `contents.lr` file. For more information see the
[Lektor docs](https://www.getlektor.com/docs/content/).

Each model has a default template associated with it, the template file matches the model name, so
the `page` model is rendered by the `page.html` template found in the `templates/` folder. The
template files are Jinja2 HTML templates. Additional information regarding templates can be found
in the [Lektor docs](https://www.getlektor.com/docs/templates/).

## Adding Pages
To add a new page navigate to the local host server, then navigate to the page that you want your
new page to be the child of. For example, to add a new page under the "about" section of the site:

1. Navigate to `http://127.0.0.1:5000/about/`
2. Click the pencil icon in the top right corner to navigate to the page admin mode
3. Click `Add Page` in the `Page Actions` on the left side of the screen
4. Set your model, title, and ID
    - Page is the default model, it will let you write page content in markdown
    - Title this will be rendered as the tab title, and what you see in the page admin mode when
    navigating between pages
    - ID is the "url slug", and the underlying folder name. It is autogenerated, typically you don't
    need to change the default value.
5. Fill out the additional fields on the next page
    - You can change the title at any time, but it will not change the ID. To change the ID you'll
    need to manually rename the folder
    - If you chose the "Page" model the only other thing you can edit is the "body", this is a
    Markdown field
6. Attachments can be added as well, for more information see the
[Lektor docs](https://www.getlektor.com/docs/content/attachments/).

## Adding Organizers
1. Navigate to `http://127.0.0.1:5000/about/the-team`
2. Click the pencil icon on the top right corner
3. Click `Add Page` in the `Page Actions` on the left side of the screen
4. Set your title, and ID
    - Note: you cannot choose a model here, because it is automatically selected to be a `blog-post`
    model, because the `news` page uses the `blog` model, which specifies that child pages use the
    `blog-post` model.
5. Fill out the additional fields on the next page
6. Add an attachment called `profile.jpg` that will be used as their photo on the team page

## Adding News Posts
1. Navigate to `http://127.0.0.1:5000/news/`
2. Click the pencil icon on the top right corner
3. Click `Add Page` in the `Page Actions` on the left side of the screen
4. Set your title, and ID
    - Note: you cannot choose a model here, because it is automatically selected to be a `blog-post`
    model, because the `news` page uses the `blog` model, which specifies that child pages use the
    `blog-post` model.
5. Fill out the `Author`, `Publication date`, `Intro`, and `Body` on the next page
    - `Intro` should be the first paragraph or so of content that you want visible on the `/news`
    page which lists all of the recent blog posts
    - `Body` is the main content of the post, but is hidden from the list view

## Adding/Removing Speakers and Talks
In order to add a new speaker and/or talk you must first log into the pretalx account and export the speakers and
sessions.

### Exporting sessions
1. Click "sessions" in the sidebar, then click "export"
2. Under "Export format" select "JSON export"
3. Under "Target group" select "confirmed"
4. Under "Data fields" click the "select all" checkbox
5. Click the save button
6. Copy the exported file to the root of this repo

### Exporting speakers
1. Click "speakers" in the sidebar, then click "export"
2. Under "Target group" click "With confirmed proposals"
3. Under "Data fields" click the "select all" checkbox
4. Undr "Export format" select "JSON export"
5. Click the save button
6. Copy the exported file to the root of this repo

### Adding a new speaker or talk
Run the `init_program.py` file (`just generate-program`) which will read the JSON exports and add a new page for every new talk or speaker

### Modifying an existing speaker or talk
1. Delete the associated folder(s) under the `content/program/talks` and/or `content/program/speakers`
2. Run `init_program.py` (`just generate-program`) which will re-generate a page for each speaker or talk you removed


## Updating the Schedule CSV

If the schedule gets changed, we should also update and deploy the schedule CSV. To do this:

1. Ensure your `PRETALX_TOKEN` environment variable is set in the `.env` file
2. Run `just generate-schedule`
3. Commit the changes and deploy
    
## Saving Your Changes
All changes you make, either edits to a specific page, or new pages added, are all saved to disk.
Once you are done you'll want to commit the new or changed files to your branch in Git, and open a
pull request. An organizer will review your changes and merge them if they are ready. Once merged
an organizer will deploy your changes to the live site.

# Deploying

Deploying is easy: `just deploy`

Lektor will commit the built site to the `gh-pages` branch, and push, which will get deployed by
GitHub automatically.
