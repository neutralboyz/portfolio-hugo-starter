---
title: Zone du dehors
slug: last-project
image: /medias/uploads/code-of-practice.jpg
---
Go templates are lightweight but extensible. Go itself supplies built-in functions, including comparison operators and other basic tools. These are listed in the Go template documentation. Hugo has added additional functions to the basic template logic.

![Paris, Belleville](https://78.media.tumblr.com/f3a8c6312ae306aac5e00cf379035bde/tumblr_os68sfBVdc1twkjb3o1_1280.jpg)

It also adds constance.

![Constance](/medias/uploads/untitled-image-5-.jpg)

![](/medias/uploads/untitled-image-11-.jpg)

Publish Mode



By default, all entries created or edited in the Netlify CMS are committed directly into the main repository branch.



The publish_mode option allows you to enable “Editorial Workflow” mode for more control over the content publishing phases. All unpublished entries will be arranged in a board according to their status, and they can be further reviewed and edited before going live.



You can enable the Editorial Workflow with the following line in config.yml:



publish_mode: editorial_workflow



From a technical perspective, the workflow translates editor UI actions into common Git commands:

Actions in Netlify UI … 	Perform these Git actions

Save draft 	Commits to a new branch (named according to the pattern cms/collectionName-entrySlug), and opens a pull request

Edit draft 	Pushes another commit to the draft branch/pull request

Approve and publish draft 	Merges pull request and deletes branch

Media and Public Folders



This setting is required.



Netlify CMS users can upload files to your repository using the Media Gallery. The following settings specify where these files are saved, and where they can be accessed on your built site.



Options



\    media_folder (required): Folder path where uploaded files should be saved, relative to the base of the repo.

\    public_folder (optional): Folder path where uploaded files will be accessed, relative to the base of the built site. For fields controlled by \[file] or \[image] widgets, the value of the field is generated by prepending this path to the filename of the selected file. Defaults to the value of media_folder, with an opening / if one is not already included.



Example



media_folder: "static/images/uploads"

public_folder: "/images/uploads"



Based on the settings above, if a user used an image widget field called avatar to upload and select an image called philosoraptor.png, the image would be saved to the repository at /static/images/uploads/philosoraptor.png, and the avatar field for the file would be set to /images/uploads/philosoraptor.png.

Display URL



When the display_url setting is specified, the CMS UI will include a link in the fixed area at the top of the page, allowing content authors to easily return to your main site. The text of the link consists of the URL less the protocol portion (e.g., your-site.com).



Example:



display_url: https://your-site.com



Slug Type



The slug option allows you to change how filenames for entries are created and sanitized. For modifying the actual data in a slug, see the per-collection option below.



slug accepts multiple options:



\    encoding

\    unicode (default): Sanitize filenames (slugs) according to RFC3987 and the WHATWG URL spec. This spec allows non-ASCII (or non-Latin) characters to exist in URLs.

\    ascii: Sanitize filenames (slugs) according to RFC3986. The only allowed characters are (0-9, a-z, A-Z, _, -, ~).

\    clean_accents: Set to true to remove diacritics from slug characters before sanitizing. This is often helpful when using ascii encoding.



Example



slug:

  encoding: "ascii"

  clean_accents: true



Collections



This setting is required.



The collections setting is the heart of your Netlify CMS configuration, as it determines how content types and editor fields in the UI generate files and content in your repository. Each collection you configure displays in the left sidebar of the Content page of the editor UI, in the order they are entered into config.yml.



collections accepts a list of collection objects, each with the following options:



\    name (required): unique identifier for the collection, used as the key when referenced in other contexts (like the relation widget)

\    Label: label for the collection in the editor UI; defaults to the value of name

\    label_singular: singular label for certain elements in the editor; defaults to the value of label

\    file or folder (requires one of these): specifies the collection type and location; details in Collection Types

\    filter: optional filter for folder collections; details in Collection Types

\    create: for folder collections only; true allows users to create new items in the collection; defaults to false

\    delete: false prevents users from deleting items in a collection; defaults to true

\    extension: see detailed description below

\    format: see detailed description below

\    frontmatter_delimiter: see detailed description under format

\    slug: see detailed description below

\    fields (required): see detailed description below

\    editor: see detailed description below



The last few options require more detailed information.

extension and format



These settings determine how collection files are parsed and saved. Both are optional—Netlify CMS will attempt to infer your settings based on existing items in the collection. If your collection is empty, or you’d like more control, you can set these fields explicitly.



extension determines the file extension searched for when finding existing entries in a folder collection and it determines the file extension used to save new collection items. It accepts the following values: yml, yaml, toml, json, md, markdown, html.



You may also specify a custom extension not included in the list above, as long as the collection files can be parsed and saved in one of the supported formats below.



format determines how collection files are parsed and saved. It will be inferred if the extension field or existing collection file extensions match one of the supported extensions above. It accepts the following values:



\    yml or yaml: parses and saves files as YAML-formatted data files; saves with yml extension by default

\    toml: parses and saves files as TOML-formatted data files; saves with toml extension by default

\    json: parses and saves files as JSON-formatted data files; saves with json extension by default

\    frontmatter: parses files and saves files with data frontmatter followed by an unparsed body text (edited using a body field); saves with md extension by default; default for collections that can’t be inferred. Collections with frontmatter format (either inferred or explicitly set) can parse files with frontmatter in YAML, TOML, or JSON format. However, they will be saved with YAML frontmatter.

\    yaml-frontmatter: same as the frontmatter format above, except frontmatter will be both parsed and saved only as YAML, followed by unparsed body text. The default delimiter for this option is ---.

\    toml-frontmatter: same as the frontmatter format above, except frontmatter will be both parsed and saved only as TOML, followed by unparsed body text. The default delimiter for this option is +++.

\    json-frontmatter: same as the frontmatter format above, except frontmatter will be both parsed and saved as JSON, followed by unparsed body text. The default delimiter for this option is { }.



frontmatter_delimiter: if you have an explicit frontmatter format declared, this option allows you to specify a custom delimiter like \~\~~. If you need different beginning and ending delimiters, you can use an array like \["(", ")"].

slug



For folder collections where users can create new items, the slug option specifies a template for generating new filenames based on a file’s creation date and title field. (This means that all collections with create: true must have a title field.)



Available template tags:



\    {{slug}}: a url-safe version of the title field for the file

\    {{year}}: 4-digit year of the file creation date

\    {{month}}: 2-digit month of the file creation date

\    {{day}}: 2-digit day of the month of the file creation date

\    {{hour}}: 2-digit hour of the file creation date

\    {{minute}}: 2-digit minute of the file creation date

\    {{second}}: 2-digit second of the file creation date



Example:



slug: "{{year}}-{{month}}-{{day}}_{{slug}}"



fields



The fields option maps editor UI widgets to field-value pairs in the saved file. The order of the fields in config.yml determines their order in the editor UI and in the saved file.



fields accepts a list of collection objects, each with the following options:



\    name (required): unique identifier for the field, used as the key when referenced in other contexts (like the relation widget)

\    label: label for the field in the editor UI; defaults to the value of name

\    widget: defines editor UI and inputs and file field data types; details in Widgets

\    default: specify a default value for a field; available for most widget types (see Widgets for details on each widget type)

\    required: specify as false to make a field optional; defaults to true

\    pattern: add field validation by specifying a list with a regex pattern and an error message; more extensive validation can be achieved with custom widgets



In files with frontmatter, one field should be named body. This special field represents the section of the document (usually markdown) that comes after the frontmatter.



Example:



fields:

\- label: "Title"

\    name: "title"

\    widget: "string"

\    pattern: \['.{20,}', "Must have at least 20 characters"]

\- {label: "Layout", name: "layout", widget: "hidden", default: "blog"}

\- {label: "Featured Image", name: "thumbnail", widget: "image", required: false}

\- {label: "Body", name: "body", widget: "markdown"}



editor



This setting changes options for the editor view of the collection. It has one option so far:



\    preview: set to false to disable the preview pane for this collection; defaults to true



Example:



  editor:

\    preview: false